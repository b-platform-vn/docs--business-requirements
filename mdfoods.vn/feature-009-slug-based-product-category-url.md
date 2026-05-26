# Yêu Cầu Chức Năng: Tạo Lại Đường Dẫn Sản Phẩm và Danh Mục Theo Tên (Slug-based URL)

## 1. Tổng Quan

Hiện tại, đường dẫn sản phẩm và danh mục trên **mdfoods.b-platform.vn** sử dụng trực tiếp mã số (ID thập phân) của sản phẩm/danh mục. Điều này không tối ưu cho SEO và tiềm ẩn rủi ro bảo mật (lộ cấu trúc dữ liệu nội bộ).

Yêu cầu này đề xuất chuyển sang dạng đường dẫn thân thiện, kết hợp tên sản phẩm/danh mục đã được chuẩn hoá (normalized slug) và mã định danh được mã hoá sang hệ thập lục phân (hexadecimal).

**Trước:**
```
/vi/products/62165
/vi/category/1545/1351?sort=relevance
```

**Sau:**
```
/vi/products/thit-bo-my-62165-f31d
/vi/category/gia-vi-nuoc-cham-605-f31d/muoi-tieu-chanh-547-223
```

## 2. Giải Pháp Kỹ Thuật

### 2.1. Quy tắc tạo slug

**Đối với sản phẩm:**
- Chuẩn hoá tên sản phẩm: chuyển về chữ thường, bỏ dấu tiếng Việt, thay ký tự đặc biệt/khoảng trắng bằng dấu `-`.
- Chuyển mã sản phẩm từ hệ thập phân sang hệ thập lục phân (hex).
- Ghép theo dạng: `{normalized-name}-{hex-id}`.

Ví dụ: Sản phẩm ID `62165`, tên `Thịt Bò Mỹ` → hex `f31d` → slug `thit-bo-my-f31d`

Đường dẫn: `/vi/products/thit-bo-my-f31d`

**Đối với danh mục:**
- Áp dụng quy tắc tương tự cho từng cấp danh mục trong đường dẫn.
- Mỗi segment danh mục có dạng: `{normalized-name}-{hex-id}`.

Ví dụ: Danh mục cha ID `1545`, tên `Gia vị nước chấm` → hex `609` → `gia-vi-nuoc-cham-609`
Danh mục con ID `1351`, tên `Muối tiêu chanh` → hex `547` → `muoi-tieu-chanh-547`

Đường dẫn: `/vi/category/gia-vi-nuoc-cham-609/muoi-tieu-chanh-547`

### 2.2. Quy tắc phân tích (parse) slug

Khi nhận request từ phía client:
1. Tách phần cuối của segment sau dấu `-` cuối cùng → lấy hex ID.
2. Chuyển hex ID sang thập phân → thu được mã sản phẩm/danh mục gốc.
3. Dùng mã gốc để truy vấn dữ liệu bình thường.

> Phần tên trong slug không cần khớp chính xác — hệ thống chỉ dùng hex ID để tra cứu. Điều này cho phép URL vẫn hoạt động dù tên sản phẩm thay đổi, miễn là ID giữ nguyên.

### 2.3. Redirect và tương thích ngược

- Các đường dẫn dạng cũ (`/vi/products/62165`) cần được redirect 301 sang đường dẫn slug tương ứng.
- Tránh để hai URL cùng trỏ vào một nội dung (duplicate content — ảnh hưởng SEO).

## 3. Phạm Vi Ảnh Hưởng

| Thành phần | Mô tả thay đổi |
|:-----------|:---------------|
| **Web (Frontend)** | Tạo slug khi render link, parse slug khi nhận route, xử lý redirect 301 từ URL cũ |
| **API (Backend)** | Cung cấp thông tin cần thiết để web tự tạo slug (tên + ID), hoặc cung cấp API sinh slug sẵn nếu cần |

## 4. Luồng Xử Lý

### Tạo slug (Web render)
```
Lấy thông tin sản phẩm/danh mục (ID + tên)
  → Chuẩn hoá tên: bỏ dấu, chữ thường, thay khoảng trắng bằng "-"
  → Chuyển ID sang hex
  → Ghép: {normalized-name}-{hex-id}
  → Dùng làm segment trong URL
```

### Phân tích slug (Web routing)
```
Nhận URL request: /vi/products/{slug}
  → Tách hex ID từ phần cuối của slug (sau dấu "-" cuối)
  → Chuyển hex → decimal
  → Gọi API với ID thập phân như bình thường
```

### Redirect từ URL cũ
```
Request đến /vi/products/{decimal-id}
  → Phát hiện segment chỉ là số nguyên thuần
  → Lấy thông tin sản phẩm, tạo slug
  → Redirect 301 sang /vi/products/{slug}
```

## 5. Tiêu Chí Hoàn Thành (Acceptance Criteria)

| # | Tiêu chí |
|---|----------|
| 1 | Đường dẫn sản phẩm hiển thị dạng `{normalized-name}-{hex-id}` thay vì ID thập phân thuần. |
| 2 | Đường dẫn danh mục (từng cấp) hiển thị dạng `{normalized-name}-{hex-id}`. |
| 3 | Hệ thống parse đúng hex ID từ slug và trả về đúng trang sản phẩm/danh mục. |
| 4 | URL cũ dạng `/products/{decimal-id}` được redirect 301 sang URL slug tương ứng. |
| 5 | Tên sản phẩm thay đổi không làm hỏng URL (ID là nguồn tra cứu duy nhất). |
| 6 | Slug không chứa ký tự đặc biệt, dấu tiếng Việt, hoặc khoảng trắng. |
| 7 | Không ảnh hưởng đến chức năng tìm kiếm, lọc, phân trang trên trang danh mục. |

## 6. Phân Chia Task

Tính năng này được chia thành 2 task độc lập:

- **Web (Frontend)**: Cập nhật router, tạo slug utility, xử lý redirect 301.
- **API (Backend)**: Đảm bảo response trả về đầy đủ tên + ID cho web tạo slug; review nếu cần endpoint hỗ trợ thêm.
