# Yêu Cầu Chức Năng: Cập Nhật Trường Ảnh Sản Phẩm — Sử Dụng Ảnh Gốc (TenFile) Thay Cho Ảnh Thu Nhỏ (TenFileThumb)

## 1. Tổng Quan

Trang chi tiết sản phẩm trên **mdfoods.b-platform.vn** hiện đang sử dụng trường `TenFileThumb` (ảnh thu nhỏ) làm ảnh hiển thị chính. Trường này được thiết kế cho cơ chế tải ảnh nhỏ trước — khi người dùng click vào mới hiển thị ảnh lớn (`TenFile`). Tuy nhiên, phiên bản hiện tại **không triển khai chức năng click-to-zoom / lightbox** này, dẫn đến việc ảnh hiển thị bị vỡ hoặc có chất lượng thấp.

Yêu cầu này đề xuất chuyển sang sử dụng trực tiếp trường `TenFile` (ảnh gốc, kích thước đầy đủ) để đảm bảo hình ảnh sản phẩm hiển thị đúng và rõ nét cho người dùng.

## 2. Bối Cảnh

DongPhatAPI trả về 2 trường liên quan đến hình ảnh sản phẩm:

| Trường         | Mô tả                                                                 |
| :------------- | :-------------------------------------------------------------------- |
| `TenFileThumb` | Đường dẫn ảnh thu nhỏ (thumbnail), dùng để tải nhanh khi preview     |
| `TenFile`      | Đường dẫn ảnh gốc (full-size), chất lượng cao, dùng để hiển thị chính |

**Cơ chế hoạt động ở phiên bản cũ:**
- Giao diện tải `TenFileThumb` để hiển thị ban đầu (load nhanh).
- Khi người dùng click vào ảnh, hệ thống tải và hiển thị `TenFile` (ảnh chất lượng cao).

**Vấn đề ở phiên bản hiện tại:**
- Chức năng click-to-zoom / lightbox **chưa được triển khai**.
- Giao diện vẫn dùng `TenFileThumb` làm ảnh hiển thị duy nhất.
- `TenFileThumb` khi hiển thị ở kích thước lớn (full-width container) bị **vỡ ảnh, mờ hoặc không tải được**, ảnh hưởng tiêu cực đến trải nghiệm người dùng và tính thẩm mỹ của trang sản phẩm.

## 3. Yêu Cầu Chức Năng

### 3.1. Thay Đổi Nguồn Ảnh Hiển Thị

- **Hiện tại**: Trang chi tiết sản phẩm sử dụng `TenFileThumb` làm ảnh hiển thị chính.
- **Yêu cầu**: Thay thế bằng `TenFile` (ảnh gốc, kích thước đầy đủ) làm ảnh hiển thị chính trên trang chi tiết sản phẩm.

### 3.2. Xử Lý Fallback

- Nếu `TenFile` không có giá trị (null hoặc rỗng), hệ thống fallback về `TenFileThumb`.
- Nếu cả hai trường đều không có giá trị, hiển thị ảnh placeholder mặc định của hệ thống.

### 3.3. Phạm Vi Ảnh Hưởng

Thay đổi áp dụng cho:
- Ảnh hiển thị chính (main product image) trên **trang chi tiết sản phẩm**.
- Nếu trang có hiển thị danh sách ảnh phụ (thumbnail list/gallery), cần đánh giá riêng — **nằm ngoài phạm vi của yêu cầu này**.

## 4. Luồng Xử Lý

```
Gọi DongPhatAPI lấy chi tiết sản phẩm
  → Đọc giá trị TenFile từ response
    → Nếu TenFile có giá trị: hiển thị TenFile làm ảnh chính
    → Nếu TenFile rỗng/null: fallback về TenFileThumb
      → Nếu TenFileThumb cũng rỗng/null: hiển thị ảnh placeholder
```

## 5. Ảnh Chụp Màn Hình Minh Họa

![Trang chi tiết sản phẩm — ảnh hiện đang hiển thị từ TenFileThumb](/uploads/workspaces/fdd96b12-3f46-41b5-af1b-f1e7f2a6ba02/019dfb9c-bf5e-7195-bd9f-9a1092ca4c01.png)

> Ảnh minh họa trang chi tiết sản phẩm. Khu vực ảnh chính cần được cập nhật để sử dụng `TenFile` thay cho `TenFileThumb`.

## 6. Tiêu Chí Hoàn Thành (Acceptance Criteria)

| # | Tiêu chí |
|---|----------|
| 1 | Trang chi tiết sản phẩm hiển thị ảnh từ trường `TenFile` (ảnh gốc, kích thước đầy đủ) thay vì `TenFileThumb`. |
| 2 | Ảnh hiển thị rõ nét, không bị vỡ hoặc mờ khi render ở kích thước đầy đủ trong container. |
| 3 | Khi `TenFile` rỗng hoặc null, hệ thống tự động fallback về `TenFileThumb`. |
| 4 | Khi cả `TenFile` và `TenFileThumb` đều rỗng, hệ thống hiển thị ảnh placeholder mặc định. |
| 5 | Không ảnh hưởng đến các thông tin khác trên trang chi tiết sản phẩm (tên, giá, SKU, thuộc tính, v.v.). |
| 6 | Hiệu năng tải trang không giảm đáng kể (ảnh gốc có thể lớn hơn — cần kiểm tra lazy load và tối ưu kích thước nếu cần). |
