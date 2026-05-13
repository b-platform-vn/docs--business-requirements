# FEATURE-007: Cập Nhật Nội Dung Mô Tả Footer

## Thông tin chung

| Trường        | Nội dung                                    |
|---------------|---------------------------------------------|
| **ID**        | FEATURE-007                                 |
| **Loại**      | Feature                                     |
| **Ứng dụng**  | mdfoods.vn                                  |
| **Ưu tiên**   | Medium                                      |
| **Trạng thái**| Todo                                        |

---

## Mô tả

Cập nhật nội dung đoạn mô tả trong khu vực **Footer** của website MDFoods để phản ánh chính xác hơn sứ mệnh và vai trò của nền tảng.

---

## Hiện trạng

Nội dung hiện tại trong footer:

> MDFoods là lựa chọn hàng đầu cho nhu cầu cung ứng thực phẩm B2B số lượng lớn trong hệ sinh thái Di5 Kitchen. Chúng tôi cung cấp sản phẩm chuẩn cao với độ ổn định vận hành cấp công nghiệp.

---

## Yêu cầu thay đổi

### Tiếng Việt (`vi`)

> MDFoods đóng vai trò làm cầu nối giữa Nhà sản xuất và các Kênh phân phối trực tiếp Thực Phẩm đến tay người tiêu dùng. Với sứ mệnh "Tối ưu và Minh bạch hóa Chuỗi cung ứng Thực phẩm"

### Tiếng Anh (`en`)

> MDFoods acts as a bridge between Manufacturers and Food Distribution Channels direct to consumers. With the mission of "Optimizing and Transparency of the Food Supply Chain"

---

## Phạm vi thay đổi

- **Repository:** `web-b2b-mdfoods`
- **Khu vực:** Component Footer — trường mô tả / tagline
- **Ngôn ngữ:** Cập nhật cả bản `vi` và `en` trong file i18n/translation tương ứng

---

## Tiêu chí hoàn thành (Definition of Done)

- [ ] Nội dung tiếng Việt hiển thị đúng theo yêu cầu trên môi trường staging.
- [ ] Nội dung tiếng Anh hiển thị đúng khi chuyển sang ngôn ngữ `en`.
- [ ] Không ảnh hưởng đến layout/UI của footer (kiểm tra responsive trên mobile & desktop).
- [ ] Đã review và được product owner xác nhận.
