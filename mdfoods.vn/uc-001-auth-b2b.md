# Yêu cầu Chức năng: Đăng ký

## 1. Tổng quan

Chức năng này cho phép các doanh nghiệp (nhà hàng, khách sạn, đại lý,...) đăng ký tham gia hệ thống cung ứng của MDFoods thông qua nền tảng Web. Hệ thống phân định rõ trách nhiệm xác thực giữa quản trị viên hệ thống (MDFoods) và quản trị viên của từng doanh nghiệp.

## 2. Thông tin thu thập khi đăng ký

Người dùng đăng ký tài khoản doanh nghiệp trên website https://mdfoods.vn

### 2.1. Thông tin Doanh nghiệp

- **Tên công ty**: Theo Giấy phép kinh doanh (Bắt buộc).
- **Mã số thuế (MST)**: (Bắt buộc).

### 2.2. Thông tin Người đại diện / Người đăng ký

- **Họ và Tên**: (Bắt buộc).
- **Số điện thoại**: (Bắt buộc - dùng để liên lạc/xác thực).
- **Email**: (Bắt buộc - dùng làm định danh đăng nhập).
- **Chức danh**: Vị trí công tác tại công ty (Ví dụ: Chủ sở hữu, Quản lý mua hàng, Kế toán).

## 3. Các kịch bản Đăng ký (Logic nghiệp vụ)

Hệ thống tự động kiểm tra sự tồn tại của Email (Người dùng) và Mã số thuế (Công ty) để điều hướng theo 3 trường hợp:

### Trường hợp 1: Người dùng và Công ty đều chưa có trong hệ thống

- **Khởi tạo Công ty**: Hệ thống thêm mới thông tin công ty với trạng thái Chưa xác thực.
- **Khởi tạo Người dùng**: Hệ thống tạo tài khoản người dùng mới.
- **Thiết lập liên kết**: Tạo liên kết giữa Người dùng và Công ty với chức danh tương ứng.
- **Phân quyền**: Gán quyền Admin Doanh nghiệp cho người dùng này đối với công ty vừa tạo. Trạng thái liên kết là Chờ xác thực.
- **Thông báo**: Gửi yêu cầu phê duyệt đến Quản trị viên MDFoods.

### Trường hợp 2: Người dùng đã tồn tại nhưng Công ty chưa có trong hệ thống

_(Áp dụng cho trường hợp người dùng đã có tài khoản cá nhân hoặc từ công ty khác muốn đăng ký công ty mới)_

- **Khởi tạo Công ty**: Hệ thống thêm mới thông tin công ty với trạng thái Chưa xác thực.
- **Thiết lập liên kết**: Tạo liên kết giữa Người dùng hiện tại và Công ty mới.
- **Phân quyền**: Gán quyền Admin Doanh nghiệp cho người dùng đối với công ty mới. Trạng thái liên kết là Chờ xác thực.
- **Thông báo**: Gửi yêu cầu phê duyệt đến Quản trị viên MDFoods.

### Trường hợp 3: Người dùng mới đăng ký tham gia vào Công ty đã có trong hệ thống

- **Khởi tạo Người dùng**: Hệ thống tạo tài khoản người dùng mới.
- **Thông báo phê duyệt nội bộ**: Hệ thống tự động gửi Email thông báo đến Admin Doanh nghiệp của công ty đó để chờ xác nhận.
- **Xác nhận**: Nếu Admin Doanh nghiệp phê duyệt, hệ thống chính thức tạo liên kết người dùng với công ty với trạng thái Đã xác thực.

## 4. Quy trình Phê duyệt & Trách nhiệm

| Cấp phê duyệt          | Đối tượng chịu trách nhiệm                   | Phạm vi xác thực                                                                                       |
| :--------------------- | :------------------------------------------- | :----------------------------------------------------------------------------------------------------- |
| **Admin MDFoods**      | Chứng thực Doanh nghiệp & Admin Doanh nghiệp | Kiểm tra tính pháp lý của MST, tên công ty và xác thực quyền hạn của người đăng ký làm Admin đầu tiên. |
| **Admin Doanh nghiệp** | Chứng thực Nhân viên/Người dùng mới          | Xác nhận các thành viên mới (kế toán, nhân viên thu mua) thuộc quyền quản lý của doanh nghiệp mình.    |

## 5. Yêu cầu khác

- **Email Automation**:
  - Gửi email thống báo yêu cầu xét duyệt cho Doanh Nghiệp mới tới **Admin MDFoods**
  - Gửi email thông báo kết quả xét duyệt cho người đăng ký
  - Gửi email thông báo yêu cầu xét duyệt cho Nhân viên mới tới **Admin Doanh Nghiệp**
  - Gửi email thông báo kết quả xét duyệt cho người đăng ký
