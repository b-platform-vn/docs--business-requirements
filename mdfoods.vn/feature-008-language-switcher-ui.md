# Yêu cầu Chức năng: Cải Tiến Giao Diện Chuyển Đổi Ngôn Ngữ

## 1. Tổng quan

Cải tiến bộ chọn ngôn ngữ (language switcher) hiện tại để hiển thị rõ ràng hai tùy chọn **VI** và **EN** cùng một lúc, đồng thời làm nổi bật (highlight) ngôn ngữ đang được sử dụng.

**Liên quan:** [FEATURE-004: Hỗ trợ Đa Ngôn Ngữ (EN/VI)](/mdfoods.vn/feature-004-i18n-en-vi.md)

## 2. Vấn đề hiện tại

- Bộ chọn ngôn ngữ hiện chỉ hiển thị một nút **VI** duy nhất.
- Khi nhấn vào **VI**, hệ thống chuyển sang Tiếng Anh — hành vi này gây nhầm lẫn cho người dùng vì không rõ đây là nút toggle hay nút chọn ngôn ngữ hiện tại.
- Người dùng không biết ngôn ngữ nào đang được kích hoạt.

## 3. Yêu cầu Chức năng

### 3.1. Hiển thị hai tùy chọn ngôn ngữ

- Bộ chọn ngôn ngữ phải hiển thị **đồng thời cả hai** tùy chọn: **VI** và **EN**.
- Giao diện gợi ý: `VI | EN` hoặc dạng hai nút cạnh nhau.

### 3.2. Highlight ngôn ngữ hiện tại

- Ngôn ngữ đang được kích hoạt phải được làm nổi bật rõ ràng (ví dụ: in đậm, màu sắc khác biệt, gạch chân hoặc nền highlight).
- Ngôn ngữ không được kích hoạt hiển thị ở trạng thái thụ động (mờ hơn hoặc không highlight).

### 3.3. Hành vi khi chọn ngôn ngữ

- Nhấn vào **VI**: chuyển sang Tiếng Việt, highlight **VI**.
- Nhấn vào **EN**: chuyển sang Tiếng Anh, highlight **EN**.
- Hành vi chuyển đổi URL, lưu cookies và gọi API giữ nguyên theo đặc tả trong [FEATURE-004](/mdfoods.vn/feature-004-i18n-en-vi.md).

## 4. Giao diện Mẫu

```
Trạng thái ngôn ngữ Tiếng Việt:  [VI] | EN
Trạng thái ngôn ngữ Tiếng Anh:    VI | [EN]
```

Trong đó `[X]` biểu thị ngôn ngữ đang được highlight/kích hoạt.

## 5. Tiêu chí Hoàn thành (Acceptance Criteria)

| # | Tiêu chí |
|---|----------|
| 1 | Bộ chọn ngôn ngữ hiển thị đồng thời cả **VI** và **EN**. |
| 2 | Ngôn ngữ đang được kích hoạt được highlight rõ ràng, phân biệt với ngôn ngữ còn lại. |
| 3 | Nhấn **VI** khi đang ở EN → chuyển sang Tiếng Việt, highlight **VI**. |
| 4 | Nhấn **EN** khi đang ở VI → chuyển sang Tiếng Anh, highlight **EN**. |
| 5 | Nhấn vào ngôn ngữ đang được kích hoạt → không có hành động gì (hoặc reload trang giữ nguyên ngôn ngữ). |
| 6 | Giao diện hiển thị đúng trên cả desktop và mobile. |
