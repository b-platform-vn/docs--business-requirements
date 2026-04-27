# Bug-005: Connector/Internal dùng MongoDB DNS cũ sau khi migrate sang replica set

**Service:** `api-mcm-connector-internal`, `platform-fluxcd`  
**Báo cáo:** 2026-04-26  
**Mức độ:** High  
**Cluster / Tenant:** `k8s-dpsrv-prd`, tenants `MDF`, `ODL`

---

## Tóm tắt

Sau khi migrate MongoDB PRD từ standalone sang replica set, service DNS `mongodb.database.svc.cluster.local` không còn tồn tại. Connector/Internal vẫn đọc `MONGODB_URI` cũ trỏ vào host này nên phát sinh lỗi `ENOTFOUND` liên tục và Change Stream không khởi động được.

---

## Cách tái hiện

1. MongoDB chạy replica set qua chart Bitnami, chỉ còn service `mongodb-headless`.
2. Giữ `MONGODB_URI` của backend ở dạng:

```text
mongodb://root:***@mongodb.database.svc.cluster.local:27017/dpmis?authSource=admin
```

3. Restart Connector/Internal.
4. Kiểm tra log pod backend.

---

## Hành vi thực tế

```text
Change stream error for messages tenant MDF: getaddrinfo ENOTFOUND mongodb.database.svc.cluster.local
MongoServerSelectionError: getaddrinfo ENOTFOUND mongodb.database.svc.cluster.local
```

---

## Hành vi mong muốn

- Connector/Internal kết nối thành công đến MongoDB replica set.
- Không còn lỗi DNS `ENOTFOUND` cho host MongoDB.
- Change Stream khởi động và duy trì bình thường.

---

## Nguyên nhân gốc

- Cấu hình `MONGODB_URI` trong `backend-secret` vẫn dùng hostname của kiến trúc cũ.
- Sau migrate, service hợp lệ là `mongodb-headless.database.svc.cluster.local` và cần thêm tham số `replicaSet=rs0`.

---

## Hướng dẫn sửa

1. Cập nhật `.keys/k8s-dpsrv-prd/backend.env`:

```text
MONGODB_URI=mongodb://root:<password-encoded>@mongodb-headless.database.svc.cluster.local:27017/dpmis?authSource=admin&replicaSet=rs0
```

2. Regenerate secret bằng script chuẩn repo:

```bash
sh ./scripts/generate_secret.sh k8s-dpsrv-prd backend backend
```

3. Commit/push `k8s-dpsrv-prd/backend/backend-secret.yaml`.
4. Restart deployment `api-mcm-connector-internal-backend-api` để nạp env mới.

---

## Kết quả xác minh

- Pod Connector/Internal mới nhận đúng `MONGODB_URI` trỏ về `mongodb-headless`.
- DNS resolve thành công tới IP service MongoDB.
- Không còn xuất hiện lỗi `ENOTFOUND mongodb.database.svc.cluster.local` trong log pod mới.