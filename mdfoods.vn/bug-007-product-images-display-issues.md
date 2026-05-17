# Báo Cáo Lỗi: Hiển Thị Hình Ảnh Sản Phẩm — Ảnh Đại Diện Bị Bể và Thiếu Hình Nhỏ Bên Dưới Hình Chính

## 1. Tổng Quan

Trang web **mdfoods.b-platform.vn** đang gặp hai lỗi hiển thị hình ảnh sản phẩm:

1. **Ảnh đại diện sản phẩm bị bể** trên trang danh mục — hình thumbnail hiển thị trong thẻ sản phẩm bị vỡ/mờ/méo.
2. **Thanh hình nhỏ (thumbnail strip) bên dưới ảnh lớn không hiển thị** trên trang chi tiết sản phẩm.

## 2. Thông Tin Lỗi

| Trường               | Nội dung                                                                 |
| :------------------- | :----------------------------------------------------------------------- |
| **Môi trường**       | https://mdfoods.b-platform.vn                                            |
| **Trang lỗi 1**      | Trang Danh Mục (Category Page)                                           |
| **URL ví dụ 1**      | https://mdfoods.b-platform.vn/vi/category?parentId=1517                 |
| **Trang lỗi 2**      | Trang Chi Tiết Sản Phẩm (Product Detail Page)                            |
| **URL ví dụ 2**      | https://mdfoods.b-platform.vn/vi/products?productId=60626               |
| **Mức độ ảnh hưởng** | Trung bình — Ảnh hưởng trực tiếp đến trải nghiệm duyệt và chọn sản phẩm |

## 3. Mô Tả Chi Tiết

### 3.1. Lỗi Ảnh Đại Diện Sản Phẩm Bị Bể Trên Trang Danh Mục

**Hiện trạng:**
- Trên trang danh mục (`/vi/category?parentId=1517`), các thẻ sản phẩm (product cards) hiển thị hình ảnh đại diện bị vỡ (broken), mờ hoặc méo tỉ lệ.
- Nguyên nhân có khả năng cao: hệ thống đang dùng `TenFileThumb` (ảnh thu nhỏ) hiển thị trong container kích thước lớn, dẫn đến ảnh bị kéo dãn hoặc không tải được.

**Kỳ vọng:**
- Hình đại diện sản phẩm trên trang danh mục phải hiển thị rõ nét, đúng tỉ lệ, không bị vỡ.
- Nên sử dụng `TenFile` (ảnh gốc) hoặc đảm bảo `TenFileThumb` có kích thước phù hợp với container hiển thị.

### 3.2. Lỗi Thiếu Thanh Hình Nhỏ Bên Dưới Ảnh Lớn Trên Trang Chi Tiết Sản Phẩm

**Hiện trạng:**
- Trên trang chi tiết sản phẩm (`/vi/products?productId=60626`), không thấy thanh hiển thị hình nhỏ (thumbnail strip / image gallery) bên dưới hình ảnh lớn chính.
- Người dùng không thể xem danh sách ảnh phụ hoặc chuyển đổi giữa nhiều ảnh sản phẩm.

**Kỳ vọng:**
- Khi sản phẩm có nhiều hơn một hình ảnh, thanh thumbnail strip phải hiển thị bên dưới (hoặc bên cạnh) ảnh lớn chính.
- Người dùng có thể click vào thumbnail để chuyển ảnh chính sang ảnh được chọn.

## 4. Các Bước Tái Hiện Lỗi

### Lỗi 1 — Ảnh đại diện bị bể trên trang danh mục

1. Truy cập **https://mdfoods.b-platform.vn/vi/category?parentId=1517**.
2. Quan sát hình ảnh đại diện trong các thẻ sản phẩm.
3. Nhận thấy nhiều hình ảnh bị vỡ/mờ/méo.

### Lỗi 2 — Thiếu thumbnail strip trên trang chi tiết sản phẩm

1. Truy cập **https://mdfoods.b-platform.vn/vi/products?productId=60626**.
2. Quan sát khu vực hình ảnh sản phẩm.
3. Nhận thấy chỉ có ảnh lớn chính; không có hàng hình nhỏ bên dưới dù sản phẩm có nhiều ảnh.

## 5. Liên Kết Với Các Tài Liệu Hiện Có

Lỗi này có liên quan đến các tài liệu đã tồn tại:

- **[FEATURE-006: Cập Nhật Trường Ảnh Sản Phẩm — Sử Dụng Ảnh Gốc (TenFile) Thay Cho TenFileThumb](/mdfoods.vn/feature-006-update-product-image-to-full-size.md)**: Đề xuất dùng `TenFile` thay `TenFileThumb` cho ảnh hiển thị chính. Lỗi ảnh bể trên trang danh mục có khả năng do cùng nguyên nhân.
- **[BUG-005: Trang Chi Tiết Sản Phẩm Hiển Thị Thiếu Hình Ảnh và Thông Tin Thuộc Tính](/mdfoods.vn/bug-005-product-detail-missing-images-and-attributes.md)**: Báo cáo thiếu gallery/slider ảnh trên trang chi tiết sản phẩm. Lỗi 2 trong tài liệu này là trường hợp cụ thể xác nhận bug đó chưa được sửa.

## 6. Phân Tích Nguyên Nhân Có Thể

| # | Khả năng nguyên nhân                                                                                         |
|---|--------------------------------------------------------------------------------------------------------------|
| 1 | Trang danh mục dùng `TenFileThumb` làm nguồn ảnh; thumbnail này không phù hợp kích thước container, gây bể ảnh. |
| 2 | Component gallery/slider trên trang chi tiết sản phẩm chưa được triển khai hoặc chưa render đúng.            |
| 3 | Dữ liệu danh sách ảnh phụ không được đọc/truyền vào component thumbnail strip.                              |
| 4 | CSS của thumbnail strip bị ẩn hoặc thiếu style hiển thị.                                                    |

## 7. Yêu Cầu Sửa Lỗi

### 7.1. Sửa ảnh đại diện bể trên trang danh mục

- Kiểm tra component thẻ sản phẩm (product card) trên trang danh mục.
- Chuyển nguồn ảnh sang sử dụng `TenFile` (ảnh gốc), với fallback về `TenFileThumb` nếu `TenFile` không có.
- Đảm bảo ảnh hiển thị đúng tỉ lệ trong container (dùng `object-fit: cover` hoặc tương đương).

### 7.2. Sửa thanh thumbnail strip bên dưới ảnh lớn

- Đọc đầy đủ danh sách hình ảnh sản phẩm từ API.
- Render thanh thumbnail strip hiển thị tất cả ảnh phụ bên dưới (hoặc bên cạnh) ảnh chính.
- Khi click vào thumbnail, cập nhật ảnh chính thành ảnh được chọn.
- Chỉ hiển thị thumbnail strip khi sản phẩm có nhiều hơn 1 ảnh.

## 8. Tiêu Chí Hoàn Thành (Acceptance Criteria)

| # | Tiêu chí                                                                                                        |
|---|-----------------------------------------------------------------------------------------------------------------|
| 1 | Trang danh mục hiển thị hình đại diện sản phẩm rõ nét, không bị vỡ/mờ/méo tỉ lệ.                             |
| 2 | Ảnh đại diện trên thẻ sản phẩm sử dụng `TenFile` (hoặc fallback về `TenFileThumb` khi không có `TenFile`).    |
| 3 | Trang chi tiết sản phẩm hiển thị thanh thumbnail strip bên dưới ảnh lớn khi sản phẩm có nhiều hơn 1 hình.    |
| 4 | Click vào thumbnail trên trang chi tiết sản phẩm cập nhật ảnh chính tương ứng.                                 |
| 5 | Không ảnh hưởng đến các phần thông tin khác đang hiển thị đúng (tên sản phẩm, giá, SKU, thuộc tính, v.v.).   |
