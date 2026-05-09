# Báo Cáo Lỗi: Click Vào Sản Phẩm Tương Tự Dẫn Đến Trang Danh Mục Thay Vì Trang Chi Tiết

## 1. Thông tin chung

| Trường               | Nội dung                                                                              |
| :------------------- | :------------------------------------------------------------------------------------ |
| **ID**               | BUG-008                                                                               |
| **Tiêu đề**          | Click vào sản phẩm tương tự dẫn đến trang Danh Mục thay vì trang Chi Tiết Sản Phẩm  |
| **Phiên bản**        | 1.2.1 (PRD)                                                                           |
| **Môi trường**       | PRD: https://mdfoods.b-platform.vn / STG: https://mdfoods-beta.b-platform.vn         |
| **Trang lỗi**        | Trang chi tiết sản phẩm — phần "Sản phẩm tương tự"                                   |
| **Mức độ ưu tiên**   | Cao (High)                                                                            |
| **Loại lỗi**         | Lỗi điều hướng (Navigation Bug)                                                       |

**Links tái hiện:**
- PRD: https://mdfoods.b-platform.vn/vi/products?productId=60761
- STG: https://mdfoods-beta.b-platform.vn/vi/products?productId=59404

---

## 2. Mô tả lỗi

Khi người dùng đang xem trang chi tiết sản phẩm và click vào một sản phẩm trong danh sách **"Sản phẩm tương tự"** ở phía dưới, thay vì được điều hướng đến **trang chi tiết của sản phẩm đó**, người dùng bị chuyển đến **trang Danh Mục Sản Phẩm** (category/listing page).

Hành vi này sai so với kỳ vọng và phá vỡ luồng duyệt sản phẩm của người dùng.

---

## 3. Hành vi hiện tại vs. Kỳ vọng

| | Hành vi hiện tại | Hành vi kỳ vọng |
|---|---|---|
| Click sản phẩm tương tự | Chuyển đến trang **Danh Mục** (`/vi/category` hoặc tương đương) | Chuyển đến trang **Chi Tiết Sản Phẩm** (`/vi/products?productId=<id>`) |
| URL sau khi click | Không chứa `productId` của sản phẩm được click | Chứa đúng `productId` của sản phẩm được click |

---

## 4. Các bước tái hiện lỗi

1. Truy cập trang chi tiết sản phẩm:
   - PRD: https://mdfoods.b-platform.vn/vi/products?productId=60761
   - STG: https://mdfoods-beta.b-platform.vn/vi/products?productId=59404
2. Cuộn xuống phần **"Sản phẩm tương tự"** ở phía dưới trang.
3. Click vào bất kỳ sản phẩm nào trong danh sách đó.
4. Quan sát: người dùng bị chuyển đến trang **Danh Mục sản phẩm** thay vì trang **Chi Tiết** của sản phẩm vừa click.

---

## 5. Phân tích nguyên nhân có thể

| # | Khả năng nguyên nhân |
|---|----------------------|
| 1 | Link của card sản phẩm trong component "Sản phẩm tương tự" trỏ sai URL — thiếu `productId` hoặc dùng sai route. |
| 2 | onClick handler của card sản phẩm điều hướng đến `/category` thay vì `/products?productId=<id>`. |
| 3 | Component card sản phẩm dùng chung giữa trang danh mục và trang chi tiết, nhưng props `href` không được truyền đúng khi dùng trong context "Sản phẩm tương tự". |
| 4 | `productId` của sản phẩm tương tự không được truyền vào link do thiếu mapping dữ liệu từ API response. |

---

## 6. Yêu cầu sửa lỗi

- Kiểm tra component hiển thị "Sản phẩm tương tự" và đảm bảo mỗi card sản phẩm có link đúng định dạng `/vi/products?productId=<id>`.
- Xác nhận `productId` của từng sản phẩm tương tự được truyền đúng vào `href` hoặc `onClick` handler.
- Kiểm tra nhất quán trên cả STG và PRD.

---

## 7. Tiêu chí hoàn thành (Acceptance Criteria)

| # | Tiêu chí |
|---|----------|
| 1 | Click vào sản phẩm tương tự điều hướng đúng đến trang chi tiết của sản phẩm đó (`/vi/products?productId=<id>`). |
| 2 | URL sau khi điều hướng chứa đúng `productId` của sản phẩm được click. |
| 3 | Hành vi đúng trên cả môi trường PRD và STG. |
| 4 | Không ảnh hưởng đến điều hướng ở các khu vực khác (trang danh mục, trang chủ, v.v.). |
