# Feature-003: Nhận tin nhắn từ Zalo (Webhook)

**Service:** `api-mcm-connector-zalo`  
**Loại:** Feature  
**Nguồn:** `api-mcm-connector-zalo/docs/feature-receive-message-from-zalo/`

## Hàm bị ảnh hưởng
- `Zalo/Webhook`

## Tóm tắt
Xử lý các sự kiện webhook inbound do Zalo gửi đến Connector Zalo. Tính năng này xác minh chữ ký webhook, lưu payload Zalo gốc, bổ sung thông tin profile khách hàng khi có thể, chuyển đổi payload sang định dạng Omni message, và chuyển tiếp tin nhắn chuẩn hoá đến Omni Channel.

## Đầu vào
### `POST /api/zalo/webhook`
- Header `X-ZEvent-Signature`: bắt buộc ngoài môi trường debug.
- Request body: Zalo webhook payload.
- Request raw body: dùng cùng `app_id`, `timestamp` và `ZALO_WEBHOOK_SECRET_KEY` để xác minh chữ ký Zalo.
- Các dạng payload được hỗ trợ được định nghĩa trong các file sự kiện tại `docs/webhook/`, bao gồm: tin nhắn user, tin OA, reaction, click và seen.

## Đầu ra
- Khi thiếu chữ ký ở chế độ non-debug:

```json
{
  "success": false,
  "message": "Missing signature"
}
```

- Khi chữ ký không hợp lệ ở chế độ non-debug:

```json
{
  "success": false,
  "message": "Invalid signature, webhook ignored"
}
```

- Khi xử lý thành công:

```json
{
  "success": true,
  "message": "Successfully processed webhook",
  "result": {
    "success": true,
    "stored": true,
    "enriched": true,
    "userId": "<zalo_user_id>",
    "forwarded": true,
    "omniMessageId": "<omni_message_id>",
    "forwardResult": {}
  }
}
```

- Khi nhận được tin nhắn inbound trùng lặp, xử lý trả về response thành công kèm metadata xử lý trùng lặp thay vì tạo lại bản ghi.
- Khi xử lý gặp lỗi không mong đợi, endpoint trả về `success: false` và thông điệp lỗi.

## Tác động phụ
- Lưu hoặc cập nhật payload webhook inbound vào `zalo_message` theo khoá Zalo message id.
- Xác định OA id và user id từ sự kiện đến, bao gồm cả sự kiện phía khách hàng gửi và phía OA gửi.
- Khi người gửi là user và tồn tại access token của OA, lấy thông tin profile user từ Zalo và insert hoặc update `zalo_user`.
- Tải metadata OA từ `zalo_oa` khi có sẵn.
- Chuẩn hoá payload webhook thành Omni message document với channel `zalo`, direction `inbound`, participants, nội dung, file đính kèm, thông tin quote và metadata payload gốc.
- Chuyển tiếp tin nhắn chuẩn hoá đến Omni Channel qua `OMNI_CHANNEL_URL` khi được cấu hình.
- Giữ lại webhook đã lưu ngay cả khi chuyển tiếp thất bại, đảm bảo sự kiện inbound vẫn có thể kiểm toán.
