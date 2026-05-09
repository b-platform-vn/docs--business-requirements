# Báo Cáo Lỗi: Hiển Thị Sai Thông Tin SKU Giữa Trang Chủ và Trang Chi Tiết Sản Phẩm

## 1. Thông tin chung

| Trường               | Nội dung                                                                             |
| :------------------- | :----------------------------------------------------------------------------------- |
| **ID**               | BUG-007                                                                              |
| **Tiêu đề**          | Thông tin SKU không nhất quán giữa màn hình trang chủ và màn hình chi tiết sản phẩm |
| **Phiên bản**        | 1.2.1 (PRD)                                                                          |
| **Môi trường**       | PRD: https://mdfoods.b-platform.vn / STG: https://mdfoods-beta.b-platform.vn        |
| **Mức độ ưu tiên**   | Cao (High)                                                                           |
| **Loại lỗi**         | Lỗi hiển thị dữ liệu (Data Display Bug)                                              |
| **Liên quan đến**    | BPL-12 (lỗi SKU tương tự đã từng được fix, nay tái hiện trên v1.2.1)                |

---

## 2. Mô tả lỗi

Thông tin SKU sản phẩm được hiển thị **không nhất quán** giữa màn hình trang chủ và màn hình chi tiết sản phẩm, cụ thể:

### 2.1. Môi trường STG

| Màn hình          | Giá trị SKU hiển thị | Link                                                                           |
| :---------------- | :------------------- | :----------------------------------------------------------------------------- |
| Trang chủ         | `SKU: N/A`           | https://mdfoods-beta.b-platform.vn/vi                                          |
| Chi tiết sản phẩm | `SKU: 000000014651`  | https://mdfoods-beta.b-platform.vn/vi/products?productId=59404                 |

### 2.2. Môi trường PRD

| Màn hình          | Giá trị SKU hiển thị | Link                                                                        |
| :---------------- | :------------------- | :-------------------------------------------------------------------------- |
| Trang chủ         | `SKU: N/A`           | https://mdfoods.b-platform.vn/vi                                            |
| Chi tiết sản phẩm | `SKU: 60761`         | https://mdfoods.b-platform.vn/vi/products?productId=60761                   |

**Vấn đề**: Cùng một sản phẩm nhưng SKU hiển thị `N/A` ở trang chủ trong khi trang chi tiết lại hiển thị đúng mã SKU. Điều này gây nhầm lẫn cho người dùng và ảnh hưởng đến khả năng tra cứu, đặt hàng theo SKU.

---

## 3. Hành vi kỳ vọng

- SKU sản phẩm phải hiển thị **nhất quán** trên tất cả các màn hình: trang chủ, trang danh mục và trang chi tiết sản phẩm.
- Nếu SKU tồn tại trong dữ liệu, phải hiển thị đúng giá trị đó ở mọi nơi.
- Chỉ hiển thị `N/A` khi SKU thực sự không có trong dữ liệu từ API.

---

## 4. Các bước tái hiện lỗi

1. Truy cập trang chủ https://mdfoods.b-platform.vn/vi.
2. Quan sát thông tin SKU hiển thị trên card sản phẩm — thấy `SKU: N/A`.
3. Click vào sản phẩm để vào trang chi tiết (ví dụ: https://mdfoods.b-platform.vn/vi/products?productId=60761).
4. Quan sát thông tin SKU trên trang chi tiết — thấy `SKU: 60761`.
5. So sánh: hai màn hình hiển thị SKU khác nhau cho cùng một sản phẩm.

---

## 5. Phân tích nguyên nhân có thể

| # | Khả năng nguyên nhân |
|---|----------------------|
| 1 | API trả về danh sách sản phẩm ở trang chủ dùng trường SKU khác (hoặc không trả về) so với API trang chi tiết. |
| 2 | Component card sản phẩm ở trang chủ mapping sai tên trường SKU từ response API (key mismatch). |
| 3 | Logic fallback về `N/A` quá rộng — áp dụng cho cả trường hợp trường SKU tồn tại nhưng có format khác. |
| 4 | Đây là regression từ BPL-12 đã được fix trước đây, có thể bị tái hiện do merge conflict hoặc refactor. |

---

## 6. Yêu cầu sửa lỗi

- Kiểm tra và đồng bộ hóa cách lấy/hiển thị trường SKU giữa component card sản phẩm (trang chủ) và component trang chi tiết.
- Xác định đúng tên field SKU trong response API được dùng cho danh sách sản phẩm.
- Cập nhật mapping hoặc transformer để SKU hiển thị nhất quán.
- Viết test case kiểm tra tính nhất quán dữ liệu SKU giữa các màn hình.

---

## 7. Tiêu chí hoàn thành (Acceptance Criteria)

| # | Tiêu chí |
|---|----------|
| 1 | SKU sản phẩm hiển thị đúng và nhất quán trên trang chủ, trang danh mục và trang chi tiết sản phẩm. |
| 2 | Không có sản phẩm nào hiển thị `SKU: N/A` ở trang chủ trong khi trang chi tiết có SKU hợp lệ. |
| 3 | Kiểm tra thành công trên cả môi trường STG và PRD. |
| 4 | Regression test được bổ sung để ngăn lỗi tái hiện trong các phiên bản tiếp theo. |
