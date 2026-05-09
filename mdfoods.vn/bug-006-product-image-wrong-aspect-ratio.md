# Báo Cáo Lỗi: Ảnh Sản Phẩm Sai Tỉ Lệ Trên Trang Chi Tiết

## 1. Thông tin chung

| Trường               | Nội dung                                                                   |
| :------------------- | :------------------------------------------------------------------------- |
| **ID**               | BUG-006                                                                    |
| **Tiêu đề**          | Ảnh sản phẩm hiển thị sai tỉ lệ — bị crop khi thay đổi kích thước màn hình |
| **Phiên bản**        | 1.2.1 (PRD)                                                                |
| **Môi trường**       | https://mdfoods.b-platform.vn                                              |
| **Trang lỗi**        | Trang chi tiết sản phẩm (Product Detail Page)                              |
| **Ví dụ sản phẩm**   | https://mdfoods.b-platform.vn/vi/products?productId=60761                  |
| **Mức độ ưu tiên**   | Trung bình (Medium)                                                        |
| **Loại lỗi**         | Lỗi giao diện / Hiển thị (UI Bug)                                          |
| **Ảnh chụp màn hình**| Sẽ cung cấp sau                                                            |

---

## 2. Mô tả lỗi

Trên trang chi tiết sản phẩm, ảnh sản phẩm được hiển thị với **tỉ lệ không cố định** (không phải tỉ lệ 1:1). Trong khi đó, ảnh gốc từ hệ thống có tỉ lệ **vuông 600×600 px**.

Hệ quả:
- Khi người dùng thay đổi kích thước cửa sổ trình duyệt hoặc truy cập từ các thiết bị có màn hình khác nhau (mobile, tablet, desktop), ảnh sản phẩm bị **crop (cắt xén)** theo chiều ngang hoặc chiều dọc.
- Nội dung quan trọng trong ảnh (nhãn hiệu, thông tin sản phẩm in trên bao bì) có thể bị mất khỏi tầm nhìn.
- Trải nghiệm người dùng không nhất quán giữa các kích thước màn hình.

---

## 3. Hành vi kỳ vọng

- Ảnh sản phẩm phải hiển thị theo đúng tỉ lệ gốc **1:1 (vuông)**, không bị kéo dãn hoặc cắt xén.
- Khi container thay đổi kích thước, ảnh thu nhỏ/phóng to tương ứng nhưng **giữ nguyên tỉ lệ khung hình**.
- Sử dụng CSS `object-fit: contain` hoặc `aspect-ratio: 1 / 1` để đảm bảo hiển thị đúng trên mọi màn hình.

---

## 4. Các bước tái hiện lỗi

1. Truy cập https://mdfoods.b-platform.vn/vi/products?productId=60761.
2. Quan sát khu vực ảnh sản phẩm ở trang chi tiết.
3. Thay đổi kích thước cửa sổ trình duyệt (kéo rộng / thu hẹp).
4. Quan sát: ảnh sản phẩm bị crop theo chiều ngang hoặc chiều dọc, không giữ tỉ lệ vuông.

---

## 5. Phân tích nguyên nhân có thể

| # | Khả năng nguyên nhân |
|---|----------------------|
| 1 | Container ảnh sử dụng `object-fit: cover` hoặc `object-fit: fill` thay vì `object-fit: contain`. |
| 2 | Thiếu khai báo `aspect-ratio: 1 / 1` hoặc kích thước cố định cho container ảnh. |
| 3 | CSS responsive overrides làm thay đổi chiều cao container ảnh không theo tỉ lệ. |

---

## 6. Yêu cầu sửa lỗi

- Đặt `aspect-ratio: 1 / 1` cho container ảnh sản phẩm trên trang chi tiết.
- Sử dụng `object-fit: contain` để ảnh luôn hiển thị đầy đủ trong container mà không bị crop.
- Kiểm tra và đảm bảo hiển thị nhất quán trên mobile, tablet và desktop.
- Không ảnh hưởng đến các thành phần khác trên trang chi tiết sản phẩm.

---

## 7. Tiêu chí hoàn thành (Acceptance Criteria)

| # | Tiêu chí |
|---|----------|
| 1 | Ảnh sản phẩm hiển thị đúng tỉ lệ 1:1 trên trang chi tiết sản phẩm. |
| 2 | Ảnh không bị crop khi thay đổi kích thước cửa sổ trình duyệt. |
| 3 | Hiển thị nhất quán trên các thiết bị: desktop (≥1280px), tablet (768–1279px), mobile (≤767px). |
| 4 | Không ảnh hưởng đến gallery ảnh hoặc các thành phần khác trên trang chi tiết. |
