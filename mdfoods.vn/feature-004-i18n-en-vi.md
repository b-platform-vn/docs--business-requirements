# Yêu cầu Chức năng: Hỗ trợ Đa Ngôn Ngữ (EN/VI)

## 1. Tổng quan

Chức năng này cho phép người dùng truy cập và sử dụng nền tảng MDFoods bằng hai ngôn ngữ: **Tiếng Việt (VI)** và **Tiếng Anh (EN)**. Ngôn ngữ hiển thị được xác định theo thứ tự ưu tiên từ URL, sau đó là cookies đã lưu.

## 2. Yêu cầu Chức năng

### 2.1. Chuyển đổi Ngôn ngữ

- Giao diện web cung cấp bộ chọn ngôn ngữ (language switcher) để người dùng có thể chuyển đổi giữa **Tiếng Việt** và **Tiếng Anh**.
- Khi người dùng chọn ngôn ngữ, lựa chọn được **lưu vào cookies** để duy trì trong các lần truy cập tiếp theo.

### 2.2. Xác định Ngôn ngữ theo URL

- Website hỗ trợ hai tiền tố ngôn ngữ trên URL:
  - `/vi/...` — hiển thị nội dung bằng **Tiếng Việt**
  - `/en/...` — hiển thị nội dung bằng **Tiếng Anh**
- Khi người dùng truy cập trực tiếp qua URL có tiền tố ngôn ngữ (`/vi` hoặc `/en`), hệ thống **xác nhận và ghi đè (overwrite)** giá trị ngôn ngữ đang lưu trong cookies.

### 2.3. Thứ tự Ưu tiên Xác định Ngôn ngữ

| Thứ tự | Nguồn              | Mô tả                                                               |
| :----: | :----------------- | :------------------------------------------------------------------ |
| 1      | URL path prefix    | `/vi` hoặc `/en` trong URL — ưu tiên cao nhất, ghi đè cookies       |
| 2      | Cookies            | Ngôn ngữ đã được lưu từ lần chọn hoặc truy cập URL trước đó         |
| 3      | Mặc định hệ thống  | Tiếng Việt (`vi`) nếu không có thông tin từ URL hoặc cookies         |

### 2.4. Tích hợp DongPhatAPI

- Tất cả các yêu cầu gửi tới **DongPhatAPI** phải kèm theo query parameter `lang` để API trả về nội dung đúng ngôn ngữ:
  - `lang=1` — Tiếng Anh (EN)
  - `lang=0` — Tiếng Việt (VN)
- Giá trị `lang` được xác định dựa trên ngôn ngữ hiện tại của phiên làm việc (theo thứ tự ưu tiên ở mục 2.3).

## 3. Luồng Xử lý

### 3.1. Người dùng truy cập qua URL có tiền tố ngôn ngữ

```
Người dùng truy cập /en/... 
  → Hệ thống đọc tiền tố "en"
  → Ghi đè cookies: lang = "en"
  → Gọi DongPhatAPI với lang=1
  → Hiển thị toàn bộ giao diện bằng Tiếng Anh
```

### 3.2. Người dùng chọn ngôn ngữ qua bộ chọn

```
Người dùng nhấn "Tiếng Anh" trên language switcher
  → Cập nhật cookies: lang = "en"
  → Redirect sang URL tương ứng: /en/...
  → Gọi DongPhatAPI với lang=1
  → Hiển thị toàn bộ giao diện bằng Tiếng Anh
```

### 3.3. Người dùng truy cập không có tiền tố ngôn ngữ

```
Người dùng truy cập /products
  → Hệ thống kiểm tra cookies
    → Nếu có: sử dụng ngôn ngữ từ cookies
    → Nếu không có: mặc định Tiếng Việt
  → Gọi DongPhatAPI với lang tương ứng
  → Hiển thị giao diện theo ngôn ngữ đã xác định
```

## 4. Phạm vi Dịch thuật

Các thành phần cần hỗ trợ đa ngôn ngữ bao gồm:

- **Giao diện tĩnh**: nhãn nút, tiêu đề trang, thông báo lỗi, placeholder, tooltip.
- **Nội dung động từ API**: tên sản phẩm, mô tả, danh mục, thông báo hệ thống (trả về từ DongPhatAPI theo `lang`).
- **Trang chính sách & thông tin**: Business Policy, Website Use Policy, Q&A, v.v.
- **Email tự động**: nội dung email gửi cho người dùng theo ngôn ngữ ưu tiên của họ.

## 5. Yêu cầu Kỹ thuật

- Sử dụng thư viện i18n chuẩn phù hợp với framework frontend đang dùng (ví dụ: `next-intl`, `react-i18next`).
- File ngôn ngữ được tổ chức theo chuẩn JSON, tách riêng cho từng ngôn ngữ:
  - `public/locales/vi/*.json`
  - `public/locales/en/*.json`
- Cookies lưu trữ ngôn ngữ sử dụng key `NEXT_LOCALE` (hoặc tương đương theo framework), thời hạn **1 năm**.
- Khi chuyển đổi ngôn ngữ, URL phải được cập nhật tương ứng (`/vi/...` ↔ `/en/...`) để đảm bảo tính nhất quán và SEO.
- Hỗ trợ **SEO đa ngôn ngữ**: thẻ `hreflang` phải được khai báo đúng cho mỗi phiên bản ngôn ngữ của trang.

## 6. Tiêu chí Hoàn thành (Acceptance Criteria)

| # | Tiêu chí |
|---|----------|
| 1 | Người dùng truy cập `/en/...` thấy toàn bộ giao diện bằng Tiếng Anh và cookies được ghi `en`. |
| 2 | Người dùng truy cập `/vi/...` thấy toàn bộ giao diện bằng Tiếng Việt và cookies được ghi `vi`. |
| 3 | Người dùng chọn ngôn ngữ qua language switcher, reload lại trang vẫn giữ nguyên ngôn ngữ đã chọn. |
| 4 | Mọi yêu cầu API đến DongPhatAPI đều kèm đúng giá trị `lang` (0 hoặc 1) theo ngôn ngữ hiện tại. |
| 5 | Truy cập URL không có tiền tố ngôn ngữ, hệ thống dùng ngôn ngữ từ cookies; nếu không có cookies thì mặc định là Tiếng Việt. |
| 6 | URL `/vi/...` ghi đè cookies dù trước đó người dùng đã chọn `en`, và ngược lại. |
| 7 | Thẻ `hreflang` được khai báo đúng trên tất cả các trang. |
