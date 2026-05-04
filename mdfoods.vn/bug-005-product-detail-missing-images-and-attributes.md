# Báo Cáo Lỗi: Trang Chi Tiết Sản Phẩm Hiển Thị Thiếu Hình Ảnh và Thông Tin Thuộc Tính

## 1. Tổng Quan

Trang chi tiết sản phẩm trên **mdfoods.b-platform.vn** đang gặp hai lỗi hiển thị:

1. **Hình ảnh sản phẩm**: Sản phẩm có 2 hình ảnh trong DongPhatAPI nhưng giao diện chỉ hiển thị được 1 hình. Người dùng không thể xem đầy đủ bộ ảnh sản phẩm.
2. **Thông tin thuộc tính**: Các trường **"Đơn vị tính"** và **"Nhãn hiệu"** hiển thị dấu gạch ngang (`-`) dù DongPhatAPI có trả về dữ liệu tương ứng.

## 2. Thông Tin Lỗi

| Trường          | Nội dung                                                      |
| :-------------- | :------------------------------------------------------------ |
| **Môi trường**  | https://mdfoods.b-platform.vn                                 |
| **Trang lỗi**   | Trang chi tiết sản phẩm (Product Detail Page)                 |
| **Sản phẩm ví dụ** | Cá Lóc Kho Tiêu - Bếp Dì 5 - Hộp Nhôm 200g (SKU: 8938563193223) |
| **Nguồn dữ liệu** | DongPhatAPI                                                  |
| **Mức độ ảnh hưởng** | Trung bình — Ảnh hưởng trực tiếp đến trải nghiệm xem sản phẩm và quyết định mua hàng |

## 3. Mô Tả Chi Tiết

### 3.1. Lỗi Hình Ảnh Sản Phẩm

**Hiện trạng:**
- Giao diện trang chi tiết sản phẩm chỉ hiển thị **1 hình ảnh duy nhất**, không có thanh trượt (image slider/gallery) để xem thêm ảnh.
- Không có chỉ báo nào cho người dùng biết sản phẩm còn có thêm hình ảnh khác.

**Kỳ vọng:**
- DongPhatAPI trả về danh sách nhiều hình ảnh (ví dụ: 2 hình ảnh) cho sản phẩm.
- Trang chi tiết sản phẩm phải hiển thị **đầy đủ tất cả hình ảnh** dưới dạng gallery hoặc image slider, cho phép người dùng chuyển qua lại giữa các hình.

### 3.2. Lỗi Trường "Đơn Vị Tính" (Unit of Measurement)

**Hiện trạng:**
- Trường **"Đơn vị"** trong bảng thông tin sản phẩm hiển thị dấu `-` (không có dữ liệu).

**Kỳ vọng:**
- DongPhatAPI có trả về thông tin đơn vị tính cho sản phẩm.
- Trường này phải hiển thị đúng giá trị nhận được từ API (ví dụ: "Hộp", "Cái", "Kg"...).

### 3.3. Lỗi Trường "Nhãn Hiệu" / "Thương Hiệu" (Brand)

**Hiện trạng:**
- Trường **"Thương hiệu"** trong bảng thông tin sản phẩm hiển thị dấu `-` (không có dữ liệu).

**Kỳ vọng:**
- DongPhatAPI có trả về thông tin nhãn hiệu cho sản phẩm.
- Trường này phải hiển thị đúng tên nhãn hiệu nhận được từ API (ví dụ: "Bếp Dì 5").

## 4. Các Bước Tái Hiện Lỗi

1. Truy cập **https://mdfoods.b-platform.vn**.
2. Điều hướng vào trang **Danh mục**.
3. Chọn sản phẩm **"Cá Lóc Kho Tiêu - Bếp Dì 5 - Hộp Nhôm 200g"** (SKU: `8938563193223`).
4. Quan sát:
   - Khu vực hình ảnh: chỉ thấy 1 ảnh, không có gallery/slider.
   - Bảng thông tin bên phải: trường **"Đơn vị"** và **"Thương hiệu"** đều hiển thị `-`.

## 5. Ảnh Chụp Màn Hình

![Trang chi tiết sản phẩm hiển thị thiếu hình ảnh và thuộc tính](/uploads/workspaces/fdd96b12-3f46-41b5-af1b-f1e7f2a6ba02/019df32c-4bd5-732b-8795-7fb3ef726b6a.png)

> Ảnh cho thấy: chỉ có 1 hình ảnh sản phẩm; trường "Thương hiệu" và "Đơn vị" hiển thị `-`.

## 6. Phân Tích Nguyên Nhân Có Thể

| # | Khả năng nguyên nhân |
|---|----------------------|
| 1 | Frontend chỉ lấy phần tử đầu tiên trong mảng hình ảnh trả về từ API, không render toàn bộ danh sách. |
| 2 | Mapping trường dữ liệu từ response của DongPhatAPI cho "Đơn vị tính" và "Nhãn hiệu" bị sai tên field (key mismatch). |
| 3 | Các trường "Đơn vị tính" và "Nhãn hiệu" chưa được đưa vào component hiển thị hoặc bị bỏ qua trong bước transform dữ liệu. |

## 7. Yêu Cầu Sửa Lỗi

### 7.1. Sửa lỗi hình ảnh

- Đọc toàn bộ mảng hình ảnh trả về từ DongPhatAPI.
- Render gallery/slider với đầy đủ hình ảnh: hình chính (main image) và các hình phụ (thumbnails).
- Hỗ trợ điều hướng qua lại giữa các ảnh (next/prev hoặc click thumbnail).

### 7.2. Sửa lỗi "Đơn Vị Tính"

- Xác định đúng tên trường (key) trong response DongPhatAPI tương ứng với đơn vị tính.
- Map và hiển thị giá trị này vào trường "Đơn vị" trên giao diện.
- Chỉ hiển thị `-` khi giá trị thực sự không tồn tại hoặc rỗng trong response API.

### 7.3. Sửa lỗi "Nhãn Hiệu"

- Xác định đúng tên trường (key) trong response DongPhatAPI tương ứng với nhãn hiệu/thương hiệu.
- Map và hiển thị giá trị này vào trường "Thương hiệu" trên giao diện.
- Chỉ hiển thị `-` khi giá trị thực sự không tồn tại hoặc rỗng trong response API.

## 8. Tiêu Chí Hoàn Thành (Acceptance Criteria)

| # | Tiêu chí |
|---|----------|
| 1 | Trang chi tiết sản phẩm hiển thị đầy đủ tất cả hình ảnh do DongPhatAPI trả về (tối thiểu dạng gallery hoặc slider). |
| 2 | Người dùng có thể điều hướng (next/prev hoặc click) để xem từng hình ảnh trong bộ ảnh sản phẩm. |
| 3 | Trường "Thương hiệu" hiển thị đúng giá trị nhãn hiệu từ API; chỉ hiển thị `-` khi API không trả về dữ liệu. |
| 4 | Trường "Đơn vị" hiển thị đúng giá trị đơn vị tính từ API; chỉ hiển thị `-` khi API không trả về dữ liệu. |
| 5 | Không gây ảnh hưởng đến các thông tin khác đang hiển thị đúng trên trang chi tiết sản phẩm (SKU, Xuất xứ, giá, v.v.). |
