# Feature-004: Gửi tin nhắn đến Zalo (qua OmniChannel)

**Service:** `api-mcm-connector-zalo`  
**Loại:** Feature  
**Nguồn:** `api-mcm-connector-zalo/docs/feature-receive-message-from-omnichannel/`

## Hàm bị ảnh hưởng
- `Zalo/ReceiveMessage`

## Tóm tắt
Nhận tin nhắn outbound từ Omni Channel, xác thực payload, chuyển đổi Omni message sang định dạng gửi của Zalo, gửi tin đến user Zalo mục tiêu bằng access token OA đã kết nối, và trả lại metadata giao tin cho bên gọi.

## Đầu vào
### `POST /api/zalo/messages`
- Request body có thể là:
  - một Omni message document đầy đủ, hoặc
  - một object chứa `omniMessage` là Omni message document.
- Các object bắt buộc trong effective payload:
  - `channelContext`: object.
  - `recipient`: object.
- Các trường bắt buộc để thực sự gửi được đến Zalo:
  - `channelContext.accountId`: Zalo OA id đã kết nối, dùng để tải access token mới nhất.
  - `recipient.id`: Zalo user id của người nhận.
- Các trường tuỳ chọn của Omni message được endpoint chấp nhận:
  - `omniMessageId`: định danh tin outbound. Tự sinh khi bỏ qua.
  - `sourceMessageId`: định danh tin nguồn. Tự sinh khi bỏ qua.
  - `conversationId`: định danh hội thoại. Tự sinh khi bỏ qua.
  - `sender`: object participant người gửi.
  - `messageType`: mặc định là `messaging`.
  - `content`: nội dung văn bản thuần được gửi dạng `message.text`.
  - `contentType`: service chuẩn hoá khi bỏ qua hoặc không hỗ trợ.
  - `attachments`: mảng Omni attachment.
  - `replyTo.messageId`: được map sang Zalo `quote_message_id`.
  - `metadata`, `createdAt`, `status`: được chấp nhận và chuẩn hoá khi thiếu.
- Quy tắc nội dung tin sau khi chuẩn hoá:
  - Ít nhất phải có `content` hoặc attachment được hỗ trợ.
  - Nếu cả hai đều thiếu, request bị từ chối với lỗi `Either content or a supported attachment is required for Zalo outbound messages`.
  - Chỉ item đầu tiên trong `attachments` được sử dụng để tạo Zalo attachment payload.
- Các loại attachment hỗ trợ:
  - `image` hoặc `gif`: gửi dạng Zalo media template với `elements[0].url`.
  - `sticker`: gửi dạng Zalo media template với `elements[0].attachment_id` lấy từ `url` của attachment.
  - `file`, `audio` hoặc `video`: gửi dạng Zalo file attachment với `payload.token` lấy từ `url` của attachment.
  - Các loại attachment khác bị bỏ qua khi tạo payload gửi Zalo.

## Đầu ra
- Khi gửi thành công đến Zalo, trả về:

```json
{
  "channel_message_id": "<zalo_message_id>",
  "status": "forwarded",
  "quota": [
    {
      "total": 8,
      "remaining": 7,
      "valid_from": null,
      "valid_to": null,
      "apply_to": "user"
    }
  ],
  "channel": "zalo",
  "channel_connected_account_id": "<oa_id>"
}
```

- Nếu thiếu các trường bắt buộc, service từ chối request với lỗi bad request.
- Khi gửi đến Zalo thất bại, trả về response pending:

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
  "channel": "zalo",
  "channel_connected_account_id": "<oa_id>"
}
```

## Tác động phụ
- Chuẩn hoá đầu vào thành Omni message document đầy đủ khi các trường tuỳ chọn bị bỏ qua.
- Đọc access token hiệu lực mới nhất của OA mục tiêu.
- Lưu bản ghi tin outbound vào `zalo_message` trước khi gọi Zalo send-message API.
- Gọi Zalo Open API `POST /v3.0/oa/message/cs` với recipient và tin đã chuyển đổi.
- Cập nhật bản ghi tin outbound với Zalo `message_id` cuối cùng và thời điểm gửi sau khi giao tin thành công.
- Nếu gửi thất bại, ghi log lỗi và trả về pending queue id được tạo để xử lý retry.
