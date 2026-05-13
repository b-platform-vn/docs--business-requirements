# Yêu cầu Chức năng: Cập Nhật Nội Dung Mô Tả Footer

## 1. Tổng quan

Cập nhật đoạn văn mô tả thương hiệu trong khu vực footer của website MDFoods nhằm phản ánh đúng hơn sứ mệnh và định vị thương hiệu, đồng thời đảm bảo nội dung nhất quán giữa hai phiên bản ngôn ngữ Tiếng Việt và Tiếng Anh.

## 2. Yêu cầu Thay Đổi

### 2.1. Nội dung hiện tại

```
MDFoods là lựa chọn hàng đầu cho nhu cầu cung ứng thực phẩm B2B số lượng lớn trong hệ sinh thái Di5 Kitchen. Chúng tôi cung cấp sản phẩm chuẩn cao với độ ổn định vận hành cấp công nghiệp.
```

### 2.2. Nội dung mong đợi

**Tiếng Việt (VI):**

> MDFoods đóng vai trò làm cầu nối giữa Nhà sản xuất và các Kênh phân phối trực tiếp Thực Phẩm đến tay người tiêu dùng. Với sứ mệnh "Tối ưu và Minh bạch hóa Chuỗi cung ứng Thực phẩm"

**Tiếng Anh (EN):**

> MDFoods acts as a bridge between Manufacturers and Food Distribution Channels direct to consumers. With the mission of "Optimizing and Transparency of the Food Supply Chain"

## 3. Phạm vi Thay Đổi

- **Vị trí:** Khu vực footer trên toàn bộ các trang của website MDFoods.
- **Thành phần:** Đoạn mô tả thương hiệu (brand description) trong footer.
- **Ngôn ngữ:** Cần cập nhật cho cả hai phiên bản: Tiếng Việt (`/vi`) và Tiếng Anh (`/en`).

## 4. Yêu cầu Kỹ thuật

- Cập nhật nội dung trong file ngôn ngữ tương ứng:
  - `public/locales/vi/*.json` — cập nhật key mô tả footer bằng Tiếng Việt.
  - `public/locales/en/*.json` — cập nhật key mô tả footer bằng Tiếng Anh.
- Đảm bảo không ảnh hưởng đến layout/style của footer hiện tại.

## 5. Tiêu chí Hoàn thành (Acceptance Criteria)

| # | Tiêu chí |
|---|----------|
| 1 | Truy cập website phiên bản `/vi`, footer hiển thị đúng nội dung Tiếng Việt mới. |
| 2 | Truy cập website phiên bản `/en`, footer hiển thị đúng nội dung Tiếng Anh mới. |
| 3 | Nội dung cũ không còn xuất hiện ở cả hai phiên bản ngôn ngữ. |
| 4 | Layout và style của footer không thay đổi so với hiện tại. |
