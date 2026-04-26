# UC-007: Đồng bộ thời gian thực qua MongoDB Change Streams + SSE

**Service:** `api-mcm-connector-internal`, `web-mcm-messenger`  
**Loại:** Use Case  
**Nguồn:** `api-mcm-connector-internal/docs/feature-realtime-sync-sse-changestreams/`

## Quyết định & Ràng buộc

| # | Câu hỏi | Quyết định |
|---|---------|----------|
| 1 | Browser `EventSource` không thể gửi header tùy chỉnh | Dùng polyfill `@microsoft/fetch-event-source` trong Web Worker. Token đặt trong header `Authorization` — không bao giờ đưa vào URL. |
| 2 | Yêu cầu tier MongoDB Atlas | Change Streams yêu cầu replica set M10+. **MCM Developer phải xác nhận tier Atlas trước khi bắt đầu triển khai.** Nếu tier thấp hơn M10, chuyển sang Phase 1 (poll 5s + sửa seq cursor) trước. |
| 3 | Phạm vi counter `seq` | **Một counter mỗi tenant**, dùng chung cho tất cả các collection. Một giá trị cursor duy nhất cho client — logic reconnect đơn giản hơn. |
| 4 | `seq` được gán ở đâu | **Trước khi ghi** (Phương án A): lấy `seq` từ collection `counters` → ghi trường `seq` vào document → Change Stream tự nhiên mang theo. `seq` được lưu trữ vĩnh viễn trong document `messages` và `conversations`. |
| 5 | `customers` / `users` qua SSE | **Không** — giữ nguyên hai collection này với poll định kỳ 60s. SSE chỉ áp dụng cho `messages` và `conversations`. |
| 6 | Thread nào sở hữu `EventSource` | **Web Worker** — toàn bộ logic sync tập trung một chỗ. `@microsoft/fetch-event-source` hoạt động trong ngữ cảnh Worker. |
| 7 | Token hết hạn khi SSE tồn tại lâu | Khi nhận 401 từ SSE stream → đóng kết nối → chờ message `UPDATE_TOKEN` từ worker (gửi bởi `token-refresh.service.ts` hiện có) → mở lại SSE với token mới. |
| 8 | Phase 1 hay Phase 2 | **Bỏ qua Phase 1** — đi thẳng sang Change Streams + SSE (Phase 2). |

---

## Hàm bị ảnh hưởng

| Hàm | File | Loại thay đổi |
|-----|------|---------------|
| `SyncService.pullEvents()` | `src/app/sync/sync.service.ts` | **Thay thế** — xóa long-poll + EventEmitter |
| `SyncController.pullEvents()` | `src/app/sync/sync.controller.ts` | **Thay thế** — `GET /sync/pull/events` đổi thành `GET /sync/sse` |
| `SyncService.emitChange()` | `src/app/sync/sync.service.ts` | **Xóa** — thay bằng Change Stream watcher |
| Constructor `SyncService` | `src/app/sync/sync.service.ts` | **Sửa** — khởi động Change Stream trong `OnModuleInit` |
| Destructor `SyncService` | `src/app/sync/sync.service.ts` | **Sửa** — đóng Change Stream cursor trong `OnModuleDestroy` |
| `SyncController` (mới) | `src/app/sync/sync.controller.ts` | **Thêm** endpoint SSE `GET /sync/sse` |

---

## Kiến trúc hiện tại (Vấn đề)

```
POST /api/internal/messages  (được gọi bởi omni-channel)
  └─► SyncService.receiveMessage()
        └─► this.emitChange("messages", data)      ← Node.js EventEmitter (chỉ trong process)
              │
              └─► pullEvents() long-poll promise
                    giải quyết ngay nếu có waiter trên POD NÀY
                    HOẶC chờ 30 giây rồi trả về []

GET /api/internal/sync/pull/events?since=<timestamp>
  └─► SyncService.pullEvents(siteId, since)
        if (tồn tại doc từ timestamp) → trả về ngay
        else → giữ kết nối 30s, giải quyết khi emitChange() hoặc timeout

Web MCM Messenger (sync-worker)
  └─► setInterval(() => pullEvents(), 60_000)    ← định kỳ 60s
        └─► GET /sync/pull/events?since=<lastPullTimestamp>
```

### Vấn đề
1. **Lệch giao tin giữa các pod**: `EventEmitter` chạy in-process. Nếu tin Zalo được pod A nhận nhưng Web client đang long-poll với pod B, client phải chờ đủ 30 giây timeout.
2. **Cursor `updatedAt` không đáng tin**: Lệch đồng hồ và timestamp trùng nhau gây mất tin giữa các lần poll.
3. **Poll 60 giây**: Độ trễ worst-case end-to-end **hơn 90 giây**.
4. **Tải kết nối**: Giữ HTTP connection 30 giây trên nhiều tab trình duyệt tạo ra nhiều connection cùng lúc.

---

## Kiến trúc mới

```
POST /api/internal/messages  (được gọi bởi omni-channel)
  └─► SyncService.receiveMessage()
        └─► MongoDB write (messages / conversations / customers / users)
              │
              ▼
        MongoDB Atlas Change Stream (dựa trên oplog, toàn cluster)
              │
              ▼  Tất cả pod nhận sự kiện thay đổi
        SyncService change stream cursor
              │
              └─► phát tới tất cả SSE client của POD NÀY thuộc tenant tương ứng

GET /api/internal/sync/sse   (thay thế /sync/pull/events)
  └─► SyncController.streamEvents()
        └─► Trả về EventStream (text/event-stream)
              Client gửi: Authorization + X-Tenant-Id
              Server đẩy: { operation, collection, data, seq } mỗi khi có thay đổi
              Heartbeat: ":\n\n" mỗi 15s để nginx không đóng kết nối

Web MCM Messenger (sync-worker) — mới
  └─► new EventSource('/api/internal/sync/sse', { headers })
        onmessage → áp dụng upsert/delete vào Dexie → Dexie live query render lại React
  └─► setInterval(() => pullEvents(), 60_000)    ← giữ lại chỉ như fallback reconnect/bắt kịp
```

---

## Hợp đồng Endpoint mới

### `GET /api/internal/sync/sse`

**Headers**:
- `Authorization: Bearer <access_token>` — bắt buộc (gửi qua `@microsoft/fetch-event-source`, không bao giờ đưa vào URL)
- `X-Tenant-Id: <tenantId>` — bắt buộc

**Response** (`Content-Type: text/event-stream`):

```
id: 1042
data: {"operation":"upsert","collection":"messages","data":{...},"seq":1042}

id: 1043
data: {"operation":"upsert","collection":"conversations","data":{...},"seq":1043}

: heartbeat
```

- `operation`: `"upsert"` hoặc `"delete"`.
- `collection`: `"messages"` | `"conversations"` mà thôi.
- `seq`: số thứ tự monotonic do server gán cho tenant.
- `id`: giá trị SSE `Last-Event-ID`, bằng `seq`. Dùng cho browser tự động replay khi reconnect.
- Khi nhận 401, server đóng stream. Client chờ token mới rồi mới kết nối lại.

### `GET /api/internal/sync/pull/events` — cập nhật
- Tham số `since` đổi kiểu từ **chuỗi ISO timestamp → số nguyên sequence number**.
- Trả về các sự kiện có `seq > since` theo tenant, trên `messages` và `conversations`.
- Trả về `[]` ngay lập tức nếu không tìm thấy — **không giữ kết nối** trong thiết kế mới.

---

## Phương pháp Kiểm thử

### Unit Tests (`sync.service.spec.ts`)

| Trường hợp kiểm thử | Phương pháp |
|------|-------------|
| Change stream gửi sự kiện đến SSE subscriber | Mock MongoDB cursor; xác nhận `broadcastToSseTenantClients()` được gọi đúng payload |
| `pullEvents(siteId, since: number)` chỉ trả document có `seq > since` | Mock repo; kiểm tra filter dùng `seq` không dùng `updatedAt` |
| SSE heartbeat phát mỗi 15s | Mock `setInterval`; xác nhận chuỗi heartbeat được phát |
| Ngắt kết nối client xóa subscriber khỏi tenant map | Giả lập `res.on('close')`; xác nhận subscriber đã bị xóa |
| Change stream gặp lỗi kích hoạt khởi động lại cursor | Mock sự kiện `error` của cursor; xác nhận `restartChangeStream()` được gọi |

### Kiểm thử tích hợp / Thủ công

| Khịch bản | Kết quả mong muốn |
|------------|--------------------|
| Gửi tin Zalo → quan sát Web MCM Messenger | Tin xuất hiện trong < 500ms |
| 2 pod chạy; tin Zalo xử lý bởi pod A; browser kết nối pod B | Tin xuất hiện trong < 500ms (qua Change Stream trên pod B) |
| Tab trình duyệt offline 60s rồi bật lại | Logic reconnect gọi `GET /sync/pull/events?since=<lastSeq>` để bù lại, sau đó mở lại SSE |
| Pod server khởi động lại khi SSE đang mở | Trình duyệt tự động kết nối lại; gửi header `Last-Event-ID`; server replay từ seq đó |
| 3 tab trình duyệt mở cùng lúc | Cả 3 nhận sự kiện trong vòng 500ms |
| Trường `updatedAt` trùng nhau giữa 2 tin | Cả 2 tin xuất hiện (seq là duy nhất — không xung đột) |

---

## Rủi ro Di cư & Giảm thiểu

| Rủi ro | Giảm thiểu |
|---------|-----------|
| MongoDB Atlas không hỗ trợ Change Streams | Change Streams yêu cầu replica set M10+. Xác nhận tier Atlas trước khi implement. |
| Change Stream cursor mất kết nối do lỗi mạng | Implement exponential backoff + khởi động lại cursor khi gặp `ChangeStreamError`. Lưu resume token. |
| Số kết nối SSE tăng nhanh | Một kết nối SSE/tab/tenant. Với 10 nhân viên × 3 tab = 30 kết nối/pod. Chấp nhận được. Thêm header `Connection: keep-alive` và nginx `proxy_read_timeout 3600`. |
| `seq` counter trở thành bottleneck | Một `findOneAndUpdate` trên collection `counters` mỗi lần ghi tin. Ử OK với lưu lượng thấp. |

---

## File cần Tạo / Chỉnh sửa

```
api-mcm-connector-internal/
  src/
    app/
      sync/
        sync.service.ts          ← Xóa emitChange(); thêm startChangeStream(), broadcastToSseClients(), nextSeq()
        sync.controller.ts       ← Thêm @Sse() GET /sync/sse; cập nhật pullEvents() nhận integer since
        sync.service.spec.ts     ← Unit test mới cho change stream + SSE
      entities/
        message.entity.ts        ← Thêm cột seq: number (indexed, nullable cho migration)
        conversation.entity.ts   ← Thêm cột seq: number (indexed, nullable cho migration)
        counter.entity.ts        ← Entity mới: { id: string, seq: number }
```

## Checklist trước khi implement

- [ ] Xác nhận tier MongoDB Atlas cluster ≥ M10 (bắt buộc cho Change Streams)
- [ ] Thêm `@microsoft/fetch-event-source` vào package.json của `web-mcm-messenger`
- [ ] Xác nhận nginx `proxy_read_timeout` ≥ 3600s cho kết nối SSE (hoặc ghi lại thay đổi nginx cần thực hiện)
