# Feature-006: Đồng bộ dữ liệu giữa Connector Internal và Web Messenger

**Service:** `api-mcm-connector-internal`  
**Loại:** Feature  
**Nguồn:** `api-mcm-connector-internal/docs/feature-sync-data-between-connector-internal-and-web-messenger/`

## Hàm bị ảnh hưởng
- `Internal/Pull/Events`
- `Internal/Pull/Collection`
- `Internal/Push/Collection`

## Tóm tắt
Đồng bộ dữ liệu theo tenant giữa Connector Internal và Web Messenger. Tính năng hỗ trợ ba chế độ: kéo snapshot hoặc delta của một collection, kéo luồng sự kiện gộp từ nhiều collection, và đẩy document của collection vào Connector Internal. Khi các document được đẩy thuộc collection `messages`, Connector Internal sẽ chuẩn hoá tin nội bộ sang định dạng Omni message và chuyển tiếp đến Omni Channel.

## Đầu vào
### Yêu cầu chung cho tất cả endpoint sync
- Header `Authorization: Bearer <access_token>` là bắt buộc.
- Header `X-Tenant-Id: <tenantId>` là bắt buộc.
- User đã xác thực phải có quyền chuyển sang tenant yêu cầu qua `Authentication/Switch-Division`.

### `GET /api/internal/sync/pull/events`
- Query parameter `since`: chuỗi timestamp hoặc sequence number.
- Chỉ trả về thay đổi của tenant đang xác thực.
- Luồng sự kiện bao gồm các collection:
  - `conversations`
  - `customers`
  - `messages`
  - `users`

### `GET /api/internal/sync/pull`
- Query parameter `collection`: chuỗi bắt buộc.
- Các giá trị collection hỗ trợ:
  - `conversations`
  - `customers`
  - `messages`
  - `users`
- Query parameter `lastUpdatedAt`: chuỗi timestamp tuỳ chọn.
- Nếu bỏ qua `lastUpdatedAt`, endpoint trả về toàn bộ snapshot của collection theo tenant.
- Nếu có `lastUpdatedAt`, trả về các document có `updatedAt > lastUpdatedAt` theo tenant.

### `POST /api/internal/sync/push`
- JSON body:

```json
{
  "collection": "messages",
  "docs": [
    {
      "messageId": "msg-123",
      "tenantId": "KD",
      "updatedAt": "2026-04-24T00:00:00.000Z"
    }
  ]
}
```

- `collection`: chuỗi bắt buộc.
- `docs`: mảng document cần upsert, bắt buộc.
- Các giá trị collection hỗ trợ:
  - `conversations`
  - `customers`
  - `messages`
  - `users`
- Các trường nội bộ của Mongo như `_id` và `__v` sẽ bị bỏ qua.
- Mỗi document được gán buộc vào tenant đang xác thực bằng cách thay thế hoặc set `tenantId`.
- Nếu document đến đã có `tenantId` khác, document đó bị bỏ qua.
- Với `messages`, trường định danh là `messageId`; với các collection khác là `id`.

## Đầu ra
### `GET /api/internal/sync/pull/events`
- Trả về mảng sự kiện sắp xếp theo `createdAt` tăng dần.
- Mỗi item có cấu trúc:

```json
{
  "operation": "upsert",
  "collection": "messages",
  "data": {},
  "createdAt": "2026-04-24T00:00:00.000Z"
}
```

- `operation` là `delete` khi document nền có `isDeleted = true`; ngược lại là `upsert`.
- Nếu đã có thay đổi kể từ timestamp yêu cầu, trả về ngay lập tức.
- Nếu chưa có thay đổi, endpoint long-poll đến 30 giây, sau đó:
  - trả về một sự kiện mới nếu có thay đổi theo tenant trong khoảng đó, hoặc
  - trả về `[]` nếu hết thời gian chờ.

### `GET /api/internal/sync/pull`
- Trả về mảng document của collection theo tenant.
- Trả về `[]` khi không có document nào khớp.

### `POST /api/internal/sync/push`
- Trả về object tổng hợp kết quả:

```json
{
  "upserted": 1,
  "conflicts": 0,
  "forwarded": 1,
  "forwardFailures": 0,
  "forwardResults": [
    {
      "messageId": "msg-123",
      "omniMessageId": "omni-msg-123",
      "success": true,
      "httpStatus": 200,
      "response": {}
    }
  ]
}
```

- `upserted`: số document đã insert hoặc update.
- `conflicts`: số document bị bỏ qua do document hiện tại mới hơn hoặc bằng.
- `forwarded`: số document `messages` được push thành công chuyển tiếp đến Omni Channel.
- `forwardFailures`: số document `messages` được push thất bại khi chuẩn hoá hoặc chuyển tiếp.
- `forwardResults`: kết quả chi tiết từng tin cho các document `messages` được push.

## Tác động phụ
- Auth guard xác thực bearer token với `Authentication/Validate`.
- Tenant context interceptor xác thực quyền truy cập Khối qua `Authentication/Switch-Division` và gán `tenantId` vào request.
- Các endpoint pull chỉ đọc dữ liệu theo tenant.
- Endpoint push upsert bản ghi theo tenant vào MongoDB cho collection chỉ định.
- Các upsert thành công phát sự kiện thay đổi trong bộ nhớ dùng cho long-poll consumer.
- Khi push `messages` sẽ thêm:
  - tải hội thoại liên quan,
  - chuẩn hoá tin nội bộ sang Omni message,
  - POST `{ channel: "internal", payload: omniMessage }` đến `OMNI_CHANNEL_URL`,
  - ghi kết quả chuyển tiếp vào response tổng hợp.
- Nếu `OMNI_CHANNEL_URL` không được cấu hình hoặc gọi downstream thất bại, push request vẫn trả về kết quả tổng hợp nhưng `forwardFailures` sẽ tăng.
