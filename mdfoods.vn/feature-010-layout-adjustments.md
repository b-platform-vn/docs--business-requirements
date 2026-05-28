# Yêu cầu Chức năng: Điều chỉnh Một Số Tiểu Tiết Trình Bày Layout

## 1. Tổng quan

Tài liệu này đặc tả chi tiết các yêu cầu điều chỉnh nhỏ (tiểu tiết) về mặt giao diện, văn bản và logo trên hệ thống **MDFoods.vn** nhằm cải thiện trải nghiệm người dùng, tăng tính thẩm mỹ và đảm bảo tính nhất quán của thương hiệu.

## 2. Các điểm cần điều chỉnh và Yêu cầu chi tiết

### 2.1. Điều chỉnh khoảng cách dòng văn bản sứ mệnh
* **Vấn đề hiện tại:** Tại khu vực trình bày sứ mệnh (Footer hoặc phần giới thiệu), khoảng cách giữa các dòng văn bản quá rộng, tạo cảm giác rời rạc (do sử dụng double line-break hoặc margin/padding quá lớn).
* **Yêu cầu sửa đổi:**
  * Thu hẹp khoảng cách dòng để văn bản liền mạch hơn.
  * Chỉ cần sử dụng xuống dòng đơn thuần (`line-break` hoặc `<br />`), giảm bớt khoảng trống dư thừa giữa các câu.
  * Đảm bảo bố cục hiển thị gọn gàng, trực quan trên cả Desktop và Mobile.

### 2.2. Chuẩn hóa câu dịch tiếng Anh của Sứ mệnh (Mission Statement)
* **Vấn đề hiện tại:** Câu tiếng Anh mô tả sứ mệnh hiện tại chưa được tự nhiên:
  * *“With the mission of "Optimizing and Making the Agricultural Products and Food Supply Chain More Transparent".”*
* **Yêu cầu sửa đổi:**
  * Cập nhật câu tiếng Anh thành dạng câu khẳng định trực tiếp, chuyên nghiệp hơn:
  * **“Our mission is "Optimizing and Making the Agricultural Products and Food Supply Chain More Transparent".”**

### 2.3. Thay thế Logo không viền trắng
* **Vấn đề hiện tại:** Logo ở khu vực footer hoặc một số trang con vẫn đang sử dụng phiên bản có viền khung màu trắng bên ngoài (nền đục), trông không đồng bộ và mất thẩm mỹ khi đặt trên nền màu hoặc nền tối.
* **Yêu cầu sửa đổi:**
  * Thay thế bằng phiên bản logo có nền trong suốt (transparent background), không có viền khung màu trắng bao quanh (giống như logo chính thống được hiển thị ở thanh điều hướng Top bar).
  * *Lưu ý:* Nếu nhiệm vụ này đã được đề cập hoặc thực hiện một phần ở các task trước đó, cần kiểm tra kỹ và hoàn thành triệt để trong phạm vi task này.

### 2.4. Tinh chỉnh Bộ chọn Ngôn ngữ (Language Switcher)
* **Vấn đề hiện tại:** Bộ chọn ngôn ngữ hiển thị icon quả địa cầu kế bên, chiếm dụng diện tích và không thực sự cần thiết đối với một giao diện tối giản. Kích thước font chữ của hai tùy chọn `VI | EN` có thể chưa đủ nổi bật.
* **Yêu cầu sửa đổi:**
  * **Loại bỏ biểu tượng quả địa cầu** ở khu vực bộ chọn ngôn ngữ.
  * **Kiểm thử kích thước chữ (Font Size):**
    * Thử nghiệm tăng font size của cụm `VI | EN` lên một chút để xem độ nổi bật và tính hài hòa của giao diện.
    * Đánh giá trực quan: Nếu kích thước chữ lớn hơn giúp bộ chọn dễ nhìn và nổi bật hơn thì giữ nguyên font size mới; ngược lại, nếu trông mất cân đối thì quay về font size cũ.

## 3. Tiêu chí Hoàn thành (Acceptance Criteria)

| # | Tiêu chí hoàn thành |
|---|--------------------|
| 1 | Khoảng cách dòng của đoạn văn sứ mệnh được thu nhỏ lại hợp lý, văn bản trình bày liền mạch, không còn khoảng trống thừa lớn. |
| 2 | Nội dung Tiếng Anh của sứ mệnh được cập nhật chính xác thành: *“Our mission is "Optimizing and Making the Agricultural Products and Food Supply Chain More Transparent".”* |
| 3 | Logo có viền trắng ở các khu vực liên quan được thay thế hoàn toàn bằng phiên bản logo không viền trắng (nền trong suốt). |
| 4 | Biểu tượng quả địa cầu bên cạnh bộ chọn ngôn ngữ (`VI | EN`) được loại bỏ hoàn toàn. |
| 5 | Đã thực hiện thử nghiệm tăng font size cho bộ chọn ngôn ngữ, chọn ra phương án hiển thị tối ưu nhất (giữ font size lớn hơn nếu đẹp hơn, hoặc giữ font size cũ nếu không phù hợp). |
| 6 | Tất cả các thay đổi giao diện hiển thị chính xác, không bị lỗi layout trên cả giao diện Desktop và Mobile. |
