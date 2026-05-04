# Báo cáo Lỗi: Bộ lọc checkbox chỉ cho phép chọn 1 điều kiện

## 1. Thông tin chung

| Trường            | Nội dung                                                           |
| :---------------- | :----------------------------------------------------------------- |
| **ID**            | BUG-005                                                            |
| **Tiêu đề**       | Bộ lọc dạng checkbox chỉ cho phép chọn 1 điều kiện tại một thời điểm |
| **Môi trường**    | [https://mdfoods.b-platform.vn/](https://mdfoods.b-platform.vn/) |
| **Mức độ ưu tiên**| Trung bình (Medium)                                                |
| **Loại lỗi**      | Lỗi giao diện / Lỗi hành vi tương tác (UI/UX Bug)                 |

---

## 2. Mô tả lỗi

Tại trang danh sách sản phẩm trên **mdfoods.b-platform.vn**, bảng bộ lọc bên trái (sidebar filter) có các nhóm điều kiện trích lọc được hiển thị dưới dạng **checkbox** (ô tích), bao gồm:

- **Thương hiệu** (THƯƠNG HIỆU): ví dụ VY Plant Based, Bếp Dì 5, Cham&Chan, ...
- **Xuất xứ** (XUẤT XỨ): ví dụ Việt Nam, ...
- **Khoảng giá** (KHOANG GIA)

**Vấn đề**: Các ô tích (checkbox) đang hoạt động như nút radio — chỉ cho phép chọn **1 lựa chọn** tại một thời điểm. Khi người dùng chọn một giá trị thứ hai trong cùng một nhóm, giá trị đã chọn trước đó bị **bỏ chọn tự động**.

Đây là hành vi sai so với thiết kế: checkbox phải cho phép **chọn đồng thời nhiều giá trị** trong cùng một nhóm bộ lọc.

---

## 3. Các bước tái hiện lỗi

1. Truy cập [https://mdfoods.b-platform.vn/](https://mdfoods.b-platform.vn/).
2. Mở trang danh sách sản phẩm (hoặc trang có bảng bộ lọc bên trái).
3. Tại nhóm **THƯƠNG HIỆU**, tích chọn **"Bếp Dì 5"** → sản phẩm được lọc theo thương hiệu này.
4. Tiếp tục tích chọn thêm **"VY Plant Based"** trong cùng nhóm **THƯƠNG HIỆU**.
5. **Quan sát**: dấu tích trên "Bếp Dì 5" bị xóa, chỉ còn "VY Plant Based" được chọn.

---

## 4. Kết quả thực tế

- Mỗi lần chọn một giá trị mới, giá trị đã chọn trước đó bị bỏ chọn.
- Hệ thống chỉ áp dụng **1 điều kiện lọc** tại một thời điểm.
- Giao diện hiển thị checkbox (ô vuông) nhưng lại hoạt động như radio button (ô tròn).

## 5. Kết quả mong đợi

- Người dùng có thể **tích chọn đồng thời nhiều giá trị** trong cùng một nhóm bộ lọc.
- Ví dụ: chọn cả "Bếp Dì 5" và "VY Plant Based" → hệ thống lọc và hiển thị sản phẩm thuộc **cả hai** thương hiệu.
- Hành vi phải đúng với giao diện checkbox nhiều lựa chọn (multi-select checkbox).

---

## 6. Ảnh chụp màn hình

![Bộ lọc checkbox chỉ cho phép chọn 1 điều kiện](/uploads/workspaces/fdd96b12-3f46-41b5-af1b-f1e7f2a6ba02/019df331-6f5a-7ee4-9f1f-7a08ba92c36e.png)

*Hình: Bảng bộ lọc bên trái với các nhóm THƯƠNG HIỆU, XUẤT XỨ, KHOANG GIA — checkbox đang hoạt động sai (chỉ cho phép chọn 1 tại một thời điểm).*

---

## 7. Phạm vi ảnh hưởng

| Khu vực        | Ảnh hưởng |
| :------------- | :-------- |
| Trang danh sách sản phẩm | Có — bộ lọc hoạt động sai |
| Nhóm Thương hiệu | Có |
| Nhóm Xuất xứ | Có |
| Nhóm Khoảng giá | Cần kiểm tra thêm |

---

## 8. Phân tích kỹ thuật (Gợi ý)

- Kiểm tra phần logic xử lý sự kiện `onChange` của checkbox trong component bộ lọc.
- Khả năng cao là state quản lý các giá trị đã chọn đang lưu dưới dạng **string đơn** thay vì **mảng (array)**, hoặc logic cập nhật state đang **ghi đè** thay vì **thêm vào** danh sách đã chọn.
- Cần đảm bảo rằng khi người dùng tích/bỏ tích một checkbox, hệ thống thực hiện:
  - **Tích**: thêm giá trị vào mảng điều kiện lọc.
  - **Bỏ tích**: xóa giá trị khỏi mảng điều kiện lọc.
  - Gọi API/cập nhật kết quả với **tất cả** các giá trị trong mảng.

---

## 9. Tiêu chí Hoàn thành (Acceptance Criteria)

| # | Tiêu chí |
|---|----------|
| 1 | Người dùng có thể tích chọn nhiều thương hiệu cùng lúc, danh sách sản phẩm hiển thị đúng kết quả tổng hợp. |
| 2 | Người dùng có thể tích chọn nhiều xuất xứ cùng lúc, danh sách sản phẩm hiển thị đúng kết quả tổng hợp. |
| 3 | Bỏ tích một giá trị chỉ xóa giá trị đó khỏi điều kiện lọc, không ảnh hưởng đến các giá trị đang chọn khác. |
| 4 | Giao diện checkbox hiển thị đúng trạng thái (tích/bỏ tích) cho tất cả các lựa chọn đang active. |
| 5 | Khi không có giá trị nào được chọn trong một nhóm, hệ thống không áp dụng bộ lọc cho nhóm đó. |
