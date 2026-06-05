# Yêu cầu Chức năng: Form "Đăng ký nhận Bảng tin" (Newsletter Subscription Form)

## 1. Tổng quan

Chức năng này cải tiến nội dung và giao diện của form Đăng ký nhận Bảng tin (Newsletter Subscription Form) nhằm cung cấp trải nghiệm thu thập thông tin người nhận chuyên nghiệp, đồng thời hỗ trợ lựa chọn kênh nhận tin và hiển thị thông báo đăng ký thành công đồng bộ.

- **Tiêu đề form:** `Đăng ký nhận Bảng tin từ MDFoods | Submit for MDFoods Newsletter`
- **Liên quan:** [FEATURE-004: Hỗ trợ Đa Ngôn Ngữ (EN/VI)](/mdfoods.vn/feature-004-i18n-en-vi.md)

## 2. Bối cảnh / Vấn đề hiện tại

- Giao diện form đăng ký nhận bảng tin cũ có các nhãn (label) rườm rà.
- Thiếu các lựa chọn chi tiết về việc gửi bảng tin tới một hoặc tất cả địa chỉ email của người dùng.
- Giao diện xác nhận đăng ký thành công chưa được định dạng nổi bật.

## 3. Yêu cầu Chức năng

### 3.1. Cấu trúc trường nhập liệu (Form Fields)

- Lồng trực tiếp chỉ dẫn (placeholder) vào các hộp văn bản (textbox), không hiển thị nhãn label riêng biệt bên ngoài:
  - `Danh xưng (Mr/Ms)`
  - `Tên người nhận | Recipient Name`
  - `Số điện thoại | Phone No.`
  - `Email`

### 3.2. Kênh nhận Bảng tin (Subscription Channel Settings)

Người dùng chọn phạm vi email nhận tin:
- Lựa chọn 1: `Chỉ email này | This email only`
- Lựa chọn 2: `Tất cả email của bạn mà chúng tôi có | All your emails that we have`

### 3.3. Dữ liệu đầu ra & Đồng bộ hóa

- Toàn bộ thông tin đăng ký nhận bảng tin phải được lưu trữ vào Cơ sở dữ liệu của MDFoods để hỗ trợ đồng bộ tự động với hệ thống DP MIS sau này.

### 3.4. Xác nhận đăng ký thành công (Success State)

Khi người dùng nhấn gửi thông tin thành công:
- Ẩn form nhập liệu.
- Hiển thị thông điệp thành công căn giữa trang, in đậm, màu xanh lục nổi bật:
  - Tiếng Việt: `Bạn đã thực hiện thành công việc Đăng ký nhận bảng tin từ MDFoods!`
  - Tiếng Anh: `You have successfully subscribed to MDFoods' newsletter!`
- Hiển thị phần xem lại (preview) toàn bộ thông tin khách hàng đã nhập.
- Thêm dòng cảm ơn:
  - Tiếng Việt: `Chúng tôi chân thành cảm ơn và hân hạnh được phục vụ quí vị!`
  - Tiếng Anh: `We sincerely thank you and are honored to serve you!`
- Nút điều hướng: `Trở về trang chủ | Back to Homepage` dẫn về trang chủ (`/[lang]`).

## 4. Tiêu chí Hoàn thành (Acceptance Criteria)

| # | Tiêu chí |
|---|----------|
| 1 | Form đăng ký sử dụng placeholder thay vì label cho các trường nhập liệu. |
| 2 | Hỗ trợ đầy đủ song ngữ (VI/EN) cho tiêu đề, placeholder, lựa chọn và trang kết quả. |
| 3 | Người dùng có thể chọn kênh nhận tin qua các tùy chọn radio/checkbox tương ứng. |
| 4 | Thông báo thành công hiển thị đúng format (căn giữa, màu xanh lá cây, in đậm). |
| 5 | Dữ liệu đăng ký được lưu trữ chính xác trong database. |
