# Yêu cầu Chức năng: Quản lý thành viên công ty

## 1. Tổng quan

Chức năng này cho phép Quản trị viên (Admin) của doanh nghiệp (B2B) quản lý danh sách các thành viên, phân quyền và duyệt yêu cầu tham gia doanh nghiệp từ các nhân viên mới.

## 2. Quyền truy cập

Chỉ những người dùng có vai trò **Admin Doanh nghiệp** mới có quyền thêm, phân quyền, duyệt hoặc xóa thành viên trong phạm vi doanh nghiệp của mình.

## 3. Quản lý danh sách thành viên

### 3.1. Xem danh sách thành viên

- Admin có thể xem danh sách tất cả các liên kết thành viên với doanh nghiệp.
- Thông tin hiển thị bao gồm: Họ Tên, Email, Số điện thoại, Chức danh, Vai trò (Admin/Nhân viên), và Trạng thái (Đang chờ duyệt, Đã xác thực, Bị khóa/Vô hiệu hóa).

### 3.2. Chức năng tìm kiếm và lọc

- Tìm kiếm thành viên theo: Họ Tên, Email, hoặc Số điện thoại.
- Lọc thành viên theo: Trạng thái (Chờ duyệt, Hoạt động, Vô hiệu hóa) hoặc Vai trò.

## 4. Các thao tác quản lý (Logic nghiệp vụ)

### 4.1. Thêm thành viên mới (Mời vào công ty)

1. Admin nhập địa chỉ Email của nhân viên mới để gửi lời mời tham gia, kèm theo lựa chọn **Vai trò**:
   - **Admin Doanh nghiệp**: Có toàn quyền (thêm/xóa thành viên, cài đặt thông tin công ty).
   - **Nhân viên (Thu mua, Kế toán,...)**: Có thể tìm kiếm sản phẩm và đặt hàng, theo dõi đơn hàng của công ty tùy theo phân quyền chi tiết.
2. Hệ thống tạo một bản ghi lời mời (nếu người đó chưa có tài khoản) hoặc tạo yêu cầu chờ tài khoản đó xác nhận.
3. Hệ thống gửi 1 email mời tham gia đính kèm liên kết xác nhận.
4. Khi nhân viên nhấp vào liên kết, họ sẽ được điều hướng đến trang Đăng xuất (nếu đang đăng nhập sai tài khoản) hoặc Đăng ký/Đăng nhập để chấp nhận gắn kết với công ty.

### 4.2. Phê duyệt yêu cầu gửi đến (áp dụng cho người dùng tự đăng ký)

1. Khi một người dùng đăng ký tài khoản (UC-001) và tìm kiếm/chọn doanh nghiệp đã có trên hệ thống để xin gắn kết.
2. Admin Doanh nghiệp sẽ nhận được thông báo/email và yêu cầu hiển thị ở tab **Đang chờ duyệt** trong danh sách.
3. Admin có thể thực hiện **Phê duyệt** hoặc **Từ chối**:
   - **Phê duyệt**: Thành viên chính thức liên kết với công ty (Trạng thái: Đã xác thực).
   - **Từ chối**: Xóa bỏ yêu cầu liên kết. Người dùng sẽ nhận được email thông báo từ chối.

### 4.3. Cập nhật thông tin và vai trò

- Admin có thể thay đổi Vai trò (Ví dụ: Nâng quyền một nhân viên lên Admin Doanh nghiệp) hoặc cập nhật Chức danh cho thành viên.

### 4.4. Vô hiệu hóa (Deactivate) / Hủy liên kết

- Khi một nhân viên nghỉ việc hoặc chuyển bộ phận, Admin có thể **Vô hiệu hóa** hoặc **Hủy liên kết** của nhân viên đó với doanh nghiệp:
  - Nhân viên này sẽ không còn quyền đăng nhập hoặc thao tác dưới danh nghĩa doanh nghiệp nữa.
  - Các dữ liệu lịch sử như Đơn hàng mà nhân viên này đã tạo vẫn được giữ lại nguyên vẹn và hiển thị thông tin người tạo cũ.

## 5. Các quy tắc quan trọng khác

- **Điều kiện Admin tối thiểu**: Doanh nghiệp phải luôn có ít nhất một tài khoản **Admin Doanh nghiệp** ở trạng thái Hoạt động. Hệ thống sẽ chặn hành động vô hiệu hóa/hủy liên kết đối với Admin cuối cùng.
- **Email Thông báo**: Hệ thống phải tự động gửi email cho người dùng (nhân viên) khi:
  - Được mời vào doanh nghiệp.
  - Yêu cầu tham gia doanh nghiệp được Admin phê duyệt hoặc từ chối.
  - Bị hủy liên kết hoặc vô hiệu hóa.
