# UC-001: Kết nối Zalo OA

**Service:** `api-mcm-connector-zalo`  
**Loại:** Use Case  
**Nguồn:** `api-mcm-connector-zalo/docs/feature-zalo-connect/`

## Hàm bị ảnh hưởng
- `Zalo/Connect`
- `Zalo/Callback`

## Tóm tắt
Khởi tạo và hoàn thành luồng kết nối Zalo OA giữa Connector Zalo và ứng dụng Zalo. Tính năng này khởi động luồng OAuth + PKCE, nhận callback từ Zalo sau khi chủ OA xác nhận, đổi authorization code lấy thông tin xác thực, và lưu thông tin OA đã kết nối để xử lý tin nhắn về sau.

## Đầu vào
### `GET /api/zalo/connect`
- Không có request body.
- Host và protocol của request được dùng để tạo callback URL trả về cho Zalo.

### `GET /api/zalo/callback`
- Query parameter `code`: authorization code do Zalo trả về.
- Query parameter `state`: PKCE state do Connector Zalo tạo ra trong bước `connect`.
- Query parameter `code_challenge`: implementation hiện tại yêu cầu trường này phải có trong callback request.
- Query parameter `oa_id`: định danh Zalo OA trong callback khi Zalo cung cấp.

## Đầu ra
### `GET /api/zalo/connect`
- Trả về JSON có dạng:

```json
{
  "url": "https://oauth.zaloapp.com/v4/oa/permission?...",
  "statusCode": 302
}
```

- `url` trỏ đến màn hình xác nhận của Zalo OA kèm `app_id`, `redirect_uri`, `code_challenge`, `code_challenge_method` và `state`.

### `GET /api/zalo/callback`
- Khi thành công, trả về:

```json
{
  "success": true,
  "zalo": "<oa_id>"
}
```

- Khi thất bại, trả về:

```json
{
  "success": false,
  "error": "<error message>"
}
```

- Nếu thiếu các query parameter bắt buộc trong callback, service từ chối request với lỗi bad request.

## Tác động phụ
- `connect` lưu PKCE code verifier vào Redis, khoá theo `state` đã tạo, với TTL ngắn.
- `callback` đọc và xóa PKCE verifier khỏi Redis.
- `callback` gọi Zalo OAuth API để đổi authorization code lấy `access_token`, `refresh_token` và thời hạn token.
- `callback` gọi Zalo OA API để lấy thông tin profile của OA.
- Các access token còn hiệu lực cũ của cùng OA được đánh dấu là đã xóa.
- Một bản ghi access token mới được lưu vào `access_tokens`.
- Metadata của Zalo OA được insert hoặc update trong `zalo_oa`.
