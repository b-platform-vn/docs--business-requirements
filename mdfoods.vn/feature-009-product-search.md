# Tính Năng: Tìm Kiếm Sản Phẩm

## 1. Thông tin chung

| Trường             | Nội dung                                                    |
| :----------------- | :---------------------------------------------------------- |
| **ID**             | FEATURE-009                                                 |
| **Tiêu đề**        | Tìm kiếm sản phẩm toàn trang                                |
| **Ứng dụng**       | mdfoods.b-platform.vn                                       |
| **Loại**           | Tính năng mới (New Feature)                                 |
| **Mức độ ưu tiên** | Cao (High)                                                  |

---

## 2. Mô tả tính năng

Người dùng có thể tìm kiếm sản phẩm từ **bất kỳ trang nào** trong ứng dụng thông qua ô tìm kiếm trên thanh điều hướng (header/navbar). Sau khi nhập từ khóa và nhấn **Enter**, người dùng được chuyển đến **trang kết quả tìm kiếm** — có cùng layout với trang Danh Mục Sản Phẩm.

---

## 3. Yêu cầu chức năng

### 3.1. Ô tìm kiếm (Search Input)

- Ô tìm kiếm được hiển thị cố định trên thanh điều hướng, **có mặt trên tất cả các trang**.
- Người dùng có thể click vào ô tìm kiếm và nhập từ khóa bất kỳ.
- Hỗ trợ nhập tiếng Việt (có dấu và không dấu).
- Nhấn **Enter** để submit từ khóa và chuyển đến trang kết quả.

### 3.2. Trang kết quả tìm kiếm (Search Result Page)

- URL đề xuất: `/vi/search?q=<từ khóa>`
- Layout giống trang **Danh Mục Sản Phẩm** (product listing/category page): hiển thị grid sản phẩm, hỗ trợ bộ lọc, phân trang.
- Hiển thị số lượng kết quả tìm được, ví dụ: *"Tìm thấy 24 sản phẩm cho 'cá lóc'"*.
- Nếu không có kết quả: hiển thị thông báo rõ ràng, ví dụ: *"Không tìm thấy sản phẩm nào phù hợp với 'xyz'."* và gợi ý tìm kiếm khác hoặc xem tất cả sản phẩm.

### 3.3. Tiêu chí tìm kiếm (Search Scope)

Sản phẩm được đưa vào kết quả tìm kiếm khi từ khóa **giống hoặc gần giống** với ít nhất một trong các trường sau:

| Trường dữ liệu | Ghi chú |
|---|---|
| **Tên sản phẩm** | Ưu tiên cao nhất |
| **Mô tả sản phẩm** | Full-text search |
| **SKU** | Tìm kiếm chính xác (exact match) hoặc contains |
| **Xuất xứ** | Ví dụ: "Việt Nam", "Nhật Bản" |
| **Nguồn gốc** | Thông tin nguồn gốc sản phẩm |

### 3.4. Thuật toán tìm kiếm

- Tìm kiếm **không phân biệt hoa thường** (case-insensitive).
- Hỗ trợ tìm kiếm **không dấu** — ví dụ: nhập "ca loc" vẫn tìm được "Cá Lóc".
- Kết quả được **sắp xếp theo độ liên quan** (relevance): tên sản phẩm khớp được ưu tiên hơn mô tả.

---

## 4. Luồng người dùng (User Flow)

```
[Bất kỳ trang nào]
        ↓ Click vào ô tìm kiếm
[Nhập từ khóa]
        ↓ Nhấn Enter
[Trang kết quả tìm kiếm /vi/search?q=<từ khóa>]
        ↓ Hiển thị danh sách sản phẩm phù hợp (layout giống trang Danh Mục)
        ↓ Click vào sản phẩm
[Trang Chi Tiết Sản Phẩm]
```

---

## 5. Yêu cầu kỹ thuật

- **Backend API:** Endpoint tìm kiếm sản phẩm, ví dụ: `GET /api/products/search?q=<từ khóa>&page=<n>&limit=<n>`.
- **Full-text search:** Sử dụng cơ chế tìm kiếm phù hợp (PostgreSQL `ILIKE` / `tsvector`, Elasticsearch, hoặc tương đương) trên các trường: tên, mô tả, SKU, xuất xứ, nguồn gốc.
- **Frontend:** Route `/vi/search` nhận query param `q`, gọi API tìm kiếm và render kết quả dùng lại component danh sách sản phẩm hiện có.
- **SEO:** Trang kết quả tìm kiếm có `<title>` và `<meta>` phù hợp.
- **Performance:** Kết quả tìm kiếm trả về trong vòng ≤ 1 giây cho từ khóa thông thường.

---

## 6. Tiêu chí hoàn thành (Acceptance Criteria)

| # | Tiêu chí |
|---|----------|
| 1 | Ô tìm kiếm hiển thị trên tất cả các trang của ứng dụng. |
| 2 | Nhấn Enter sau khi nhập từ khóa điều hướng đến `/vi/search?q=<từ khóa>`. |
| 3 | Trang kết quả hiển thị đúng layout giống trang Danh Mục Sản Phẩm. |
| 4 | Kết quả tìm kiếm bao gồm sản phẩm khớp theo: Tên, Mô tả, SKU, Xuất xứ, Nguồn gốc. |
| 5 | Tìm kiếm không phân biệt hoa thường và hỗ trợ không dấu. |
| 6 | Hiển thị số lượng kết quả tìm được phía trên danh sách. |
| 7 | Hiển thị thông báo phù hợp khi không có kết quả. |
| 8 | Click vào sản phẩm trong trang kết quả điều hướng đúng đến trang Chi Tiết Sản Phẩm. |
| 9 | Thời gian phản hồi tìm kiếm ≤ 1 giây. |
