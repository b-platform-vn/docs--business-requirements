# Bug-004: PRD MongoDB chạy standalone làm hỏng Change Stream của Connector/Internal

**Service:** `api-mcm-connector-internal`, `platform-fluxcd`  
**Báo cáo:** 2026-04-26  
**Mức độ:** Critical  
**Cluster / Tenant:** `k8s-dpsrv-prd`, tenants `MDF`, `ODL`  
**Nguồn:** runtime log của `api-mcm-connector-internal`, manifest `platform-fluxcd/k8s-dpsrv-prd/database/mongodb.yaml`

---

## Tóm tắt

MongoDB tại PRD đang được triển khai với `architecture: standalone`, trong khi `api-mcm-connector-internal` đã khởi động luồng đồng bộ thời gian thực bằng MongoDB Change Stream theo thiết kế của UC-007. Kết quả là service liên tục ghi lỗi:

```text
Change stream error for messages tenant MDF: The $changeStream stage is only supported on replica sets
Change stream error for messages tenant ODL: The $changeStream stage is only supported on replica sets
Change stream error for conversations tenant MDF: The $changeStream stage is only supported on replica sets
```

Pod vẫn ở trạng thái `Running`, nhưng toàn bộ tính năng realtime sync dựa trên Change Stream bị vô hiệu hóa ở runtime.

**Workaround hiện tại:** Không có workaround tương đương realtime. Có thể đọc/ghi dữ liệu MongoDB theo CRUD thông thường, nhưng không thể thay thế Change Stream cho luồng SSE realtime.

---

## Cách tái hiện

1. Deploy `api-mcm-connector-internal` bản có `SyncService` khởi động Change Stream trong `OnModuleInit`.
2. Giữ MongoDB PRD ở mode standalone.
3. Khởi động pod Connector/Internal hoặc restart pod.
4. Đọc log pod:

```bash
kubectl logs -n backend <connector-internal-pod> --tail=200
```

5. Quan sát lỗi `$changeStream stage is only supported on replica sets` lặp lại cho các tenant đang được watch.

---

## Phân tích Nguyên nhân Gốc

### Cấu hình hạ tầng hiện tại

Manifest MongoDB PRD đang khai báo:

```yaml
values:
  architecture: standalone
```

Change Stream của MongoDB chỉ hoạt động khi MongoDB chạy trong topology replica set hoặc sharded cluster. Với standalone server, driver vẫn có thể kết nối bình thường cho CRUD, nhưng mọi lệnh `watch()` sẽ thất bại ngay khi mở cursor.

### Luồng lỗi ở runtime

```
Connector/Internal start
  → SyncService.OnModuleInit()
      → start change stream for tenant MDF / ODL
          → MongoDB collection.watch(...)
              → server returns error:
                 The $changeStream stage is only supported on replica sets
          → service log error
          → service tiếp tục retry / restart luồng watch
              → lỗi lặp lại liên tục
```

### Vì sao đây là bug mức Critical

- UC-007 yêu cầu realtime sync qua Change Stream + SSE.
- PRD hiện không đáp ứng precondition hạ tầng tối thiểu để tính năng này chạy.
- Ứng dụng có thể vẫn đọc/ghi dữ liệu MongoDB, nên lỗi không làm pod chết ngay; tuy nhiên chức năng realtime bị hỏng âm thầm và dễ bị bỏ sót nếu chỉ nhìn readiness của Kubernetes.

---

## Endpoint / Chức năng bị ảnh hưởng

| Endpoint / Chức năng | Vai trò | Ảnh hưởng |
|----------|----------|----------|
| `GET /api/internal/sync/sse` | Đẩy realtime events cho web | Không nhận được event từ Change Stream |
| `GET /api/internal/sync/pull/events` | Fallback bắt kịp dữ liệu sau reconnect | Không thay thế được hoàn toàn luồng realtime kỳ vọng của UC-007 |
| `SyncService` change-stream watchers | Theo dõi `messages`, `conversations` theo tenant | Lỗi runtime liên tục, không mở được watch cursor |

---

## Đầu vào / Đầu ra

### Đầu vào cấu hình hiện tại (Lỗi)

```yaml
values:
  architecture: standalone
```

### Đầu ra mong muốn

```json
{
  "mongodbTopology": "replicaSet",
  "changeStreamSupport": true,
  "connectorInternalLog": "Started change streams tenantId=MDF/ODL without replica set error"
}
```

### Đầu ra thực tế (Lỗi)

```json
{
  "mongodbTopology": "standalone",
  "changeStreamSupport": false,
  "connectorInternalLog": "The $changeStream stage is only supported on replica sets"
}
```

---

## Tác động nghiệp vụ

- Nhân viên dùng Web MCM Messenger không nhận được cập nhật realtime ổn định theo thiết kế.
- Các thay đổi ở `messages` và `conversations` không được broadcast tức thời đến client SSE.
- Hệ thống tạo log lỗi lặp liên tục, làm nhiễu việc giám sát runtime và che khuất lỗi khác.

---

## Hướng dẫn sửa

### Hạ tầng

Chuyển MongoDB PRD từ standalone sang replica set.

**Thay đổi manifest:**

```yaml
values:
  architecture: replicaset
  replicaCount: 1
```

Replica set 1 node đủ để kích hoạt Change Stream, đồng thời giảm rủi ro tài nguyên trong giai đoạn đầu migration.

### Bảo toàn dữ liệu

Vì topology thay đổi, cần thực hiện quy trình an toàn:

1. Export toàn bộ dữ liệu trước migration.
2. Commit thay đổi manifest để FluxCD reconcile.
3. Sau khi MongoDB replica set lên ổn định, import lại dữ liệu từ backup.
4. Xác minh số lượng document chính và log của Connector/Internal sau migration.

### Checklist xác minh sau sửa

- `rs.status().ok = 1`
- `messages`, `conversations` còn đủ dữ liệu sau restore
- Connector/Internal không còn log lỗi `$changeStream stage is only supported on replica sets`
- Web realtime sync hoạt động lại theo tenant

### Tiêu chí nghiệm thu nghiệp vụ

- Tin nhắn và hội thoại mới của tenants `MDF`, `ODL` xuất hiện lại theo thời gian thực trên Web MCM Messenger sau khi migration hoàn tất.

---

## File cần thay đổi

| File | Mục đích |
|------|----------|
| `platform-fluxcd/k8s-dpsrv-prd/database/mongodb.yaml` | Đổi MongoDB từ standalone sang replica set |
| `platform-fluxcd/.gitignore` | Chặn commit nhầm backup files local |

---

## Ghi chú vận hành

- Backup archive local chỉ phục vụ migration, không được commit vào Git.
- Thư mục backup nên được ignore ở repo level trước khi thực hiện commit manifest.