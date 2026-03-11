# Yêu cầu Chức năng: Đăng nhập

## 1. Tổng quan

Chức năng này cho phép người dùng đăng nhập vào hệ thống B2B. Mức độ bảo mật yêu cầu xác thực 2 bước (Mật khẩu và Mã PIN qua email).

## 2. Quy trình Đăng nhập

1. Người dùng cung cấp email và mật khẩu trên trang đăng nhập.
2. Hệ thống xác thực email và mật khẩu. Nếu hợp lệ, hệ thống gửi một email chứa mã PIN 6 số đến địa chỉ email của người dùng để xác nhận tính khả dụng.
3. Người dùng nhập mã PIN này vào màn hình đăng nhập hiện tại.
4. Hệ thống hoàn thành việc xác thực sau khi kiểm tra mã PIN hợp lệ.

## 3. Các kịch bản sau khi xác thực thành công (Logic nghiệp vụ)

Sau khi hoàn tất xác thực bằng mã PIN, hệ thống kiểm tra số lượng Doanh Nghiệp mà người dùng đang liên kết:

### Trường hợp 1: Người dùng có liên kết với 1 Doanh Nghiệp

1. Chuyển người dùng đến trang chủ.

### Trường hợp 2: Người dùng có liên kết với 2 Doanh Nghiệp trở lên

1. Yêu cầu người dùng chọn 1 trong những Doanh Nghiệp đã liên kết.
2. Chuyển người dùng đến trang chủ (của doanh nghiệp đã chọn).
