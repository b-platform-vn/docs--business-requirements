# Feature-002: Tự động làm mới Access Token

**Service:** `api-mcm-connector-zalo`  
**Loại:** Feature  
**Nguồn:** `api-mcm-connector-zalo/docs/feature-auto-refresh-token/`

## Hàm bị ảnh hưởng
- Không có endpoint được expose trong `app.yaml`.
- Luồng worker nội bộ: tự động làm mới access token Zalo OA sau lần kết nối thành công đầu tiên.

## Tóm tắt
Giữ cho các Zalo OA đã kết nối luôn hoạt động mà không cần nhà vận hành kết nối lại thủ công. Một background worker có lịch kiểm tra token đang hoạt động mới nhất của từng OA, làm mới các token sắp hết hạn, và đánh dấu token hết hạn là không hợp lệ khi không thể làm mới được nữa.

## Đầu vào
- Không có đầu vào API công khai.
- Nguồn trigger: cron worker bên trong Connector Zalo.
- Lịch chạy: mỗi phút.
- Nguồn dữ liệu: access token mới nhất chưa bị xóa của mỗi OA đã kết nối.
- Điều kiện để làm mới: token sắp hết hạn và vẫn còn refresh token.

## Đầu ra
- Không có HTTP response.
- Kết quả vận hành được ghi qua application log.
- Với mỗi OA đủ điều kiện, worker sẽ:
  - làm mới và lưu token mới, hoặc
  - ghi log lỗi làm mới.

## Tác động phụ
- Tải access token đang hoạt động mới nhất của mỗi OA đã kết nối.
- Phát hiện token sắp hết hạn trong ngưỡng cấu hình và gửi yêu cầu lấy token mới từ Zalo OAuth API.
- Đánh dấu các token cũ còn hoạt động của cùng OA là đã xóa.
- Lưu `access_token`, `refresh_token` và thời hạn đã được làm mới vào `access_tokens`.
- Nếu làm mới thất bại và token hiện tại đã hết hạn, đánh dấu token đó là đã xóa để ngăn sử dụng trong tương lai.
- Giữ nguyên các token còn tốt.
