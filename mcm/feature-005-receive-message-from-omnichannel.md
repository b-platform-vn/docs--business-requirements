# Feature-005: Nhận tin nhắn từ OmniChannel (Connector Internal)

**Service:** `api-mcm-connector-internal`  
**Loại:** Feature  
**Nguồn:** `api-mcm-connector-internal/docs/feature-receive-message-from-omnichannel/`

## Hàm bị ảnh hưởng
- `Internal/ReceiveMessage`

## Tóm tắt
Nhận Omni message đã chuẩn hoá từ Omni Channel và lưu vào cấu trúc dữ liệu của Connector Internal. Tính năng này xác định tenant mục tiêu, đảm bảo tồn tại bản ghi customer, user và conversation, chuyển đổi Omni message sang schema tin nội bộ, lưu vào MongoDB và trả về kết quả giao tin cho bên gọi.

## Đầu vào
### `POST /api/internal/messages`
- Request body có thể là:
  - một Omni message document đầy đủ, hoặc
  - một object chứa `omniMessage` là Omni message document đầy đủ.
- Các object bắt buộc trong effective payload:
  - `channelContext`
  - `sender`
  - `recipient`
- Các trường quan trọng được chấp nhận:
  - `omniMessageId`
  - `channel`
  - `direction`
  - `messageType`
  - `sourceMessageId`
  - `conversationId`
  - `channelContext.accountId`
  - `channelContext.metadata`
  - `sender.participantType`, `sender.id`
  - `recipient.participantType`, `recipient.id`
  - `content`
  - `contentType`
  - `attachments`
  - `replyTo.messageId`, `replyTo.preview`, `replyTo.senderName`
  - `metadata`
  - `createdAt`, `updatedAt`
- Quy tắc xác định tenant:
  - trước tiên tra `tenant_routes` theo `channel` + `channelContext.accountId` với `isActive = true`
  - tiếp theo thử `channelContext.metadata.tenantId`
  - tiếp theo thử `metadata.tenantId`
  - nếu `channel = internal`, fallback sang `channelContext.accountId`
- Nếu không xác định được tenant với channel không phải internal, request không thể xử lý đầy đủ.

## Đầu ra
- Khi thành công, trả về:

```json
{
  "channel_message_id": "<internal_message_id>",
  "status": "forwarded",
  "quota": [
    {
      "total": -1,
      "remaining": -1,
      "valid_from": null,
      "valid_to": null,
      "apply_to": "account"
    }
  ],
  "channel": "internal",
  "channel_connected_account_id": "<channel_account_id>"
}
```

- Khi chuẩn hoá hoặc lưu thất bại, trả về pending response:

```json
{
  "channel_message_id": "<generated_queue_id>",
  "status": "pending",
  "quota": [
    {
      "total": -1,
      "remaining": -1,
      "valid_from": null,
      "valid_to": null,
      "apply_to": "account"
    }
  ],
  "channel": "internal",
  "channel_connected_account_id": "<channel_account_id>"
}
```

- Nếu payload sai cấu trúc, service từ chối bằng lỗi bad request trước khi vào luồng pending.

## Tác động phụ
- Chuẩn hoá request thành Omni message document đầy đủ với các giá trị mặc định khi trường tuỳ chọn bị bỏ qua.
- Xác định tenant id đích từ tenant routing hoặc metadata.
- Đảm bảo tồn tại bản ghi `customer` cho participant phía khách hàng.
- Đảm bảo tồn tại bản ghi `user` cho participant không phải khách hàng có id.
- Đảm bảo tồn tại bản ghi `conversation` và cập nhật:
  - nhân viên được phân công,
  - số tin chưa đọc,
  - preview tin nhắn cuối,
  - thời điểm tin nhắn cuối.
- Xác định reply preview và reply sender name từ tin nội bộ tồn tại khi có `replyTo.messageId`.
- Chuyển đổi Omni message sang schema tin nội bộ, bao gồm cả attachment và external metadata được serialize.
- Upsert tin nội bộ vào collection `messages` và phát sự kiện thay đổi `messages`.
- Khi xử lý thất bại, ghi log cảnh báo retry hoãn và trả về queue id được tạo mà không ghi bản ghi queue trong implementation hiện tại.
