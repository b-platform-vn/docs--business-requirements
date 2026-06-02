# Yêu cầu Chức năng: Đồng bộ hóa Layout cho các Trang Thông tin (Liên hệ, Con người MDFoods, Hỏi & Đáp, Chính sách Giao hàng)

## 1. Tổng quan

Yêu cầu này đề xuất đồng bộ hóa giao diện và cấu trúc layout của 4 trang thông tin chính trên hệ thống **mdfoods.vn** để khớp với layout tiêu chuẩn của trang giới thiệu MDFoods (About Page `/mdfoods`). Việc này giúp tăng tính đồng nhất trong trải nghiệm người dùng (UI/UX) và tạo cảm giác chuyên nghiệp, nhất quán trên toàn bộ nền tảng.

Các trang cần được cập nhật bao gồm:
- **Liên hệ** (`/contact`)
- **Con người MDFoods** (`/hrbp`)
- **Hỏi & Đáp** (`/faq`)
- **Chính sách Giao hàng** (`/delivery-policy`)

**Liên quan:**
- [FEATURE-004: Hỗ trợ Đa Ngôn Ngữ (EN/VI)](/mdfoods.vn/feature-004-i18n-en-vi.md)
- [FEATURE-010: Trang "Khách hàng Thân thiết" (Loyalty Page)](/mdfoods.vn/feature-010-loyalty-page.md)

## 2. Bối cảnh / Vấn đề hiện tại

Hiện tại, các trang thông tin phụ trợ trên hệ thống đang sử dụng các kiểu layout, container và màu nền khác nhau, thiếu sự nhất quán:
- **Trang giới thiệu MDFoods (`/mdfoods`)** sử dụng layout chuẩn rất đẹp mắt: màu nền trang nhẹ nhàng (`catalog-page-bg`), nội dung đặt trong một thẻ bài viết (`article`) bo góc lớn (`rounded-[28px]`), màu nền nổi bật (`bg-(--catalog-surface)`), có đổ bóng mịn màng và căn lề rộng rãi.
- **Trang Liên hệ (`/contact`)** và **Chính sách Giao hàng (`/delivery-policy`)** có chiều rộng container tối đa lên đến `1400px` (quá rộng so với trang giới thiệu `max-w-350`), và trang Giao hàng sử dụng ảnh nền che phủ lớn dễ gây mất tập trung.
- **Trang Hỏi & Đáp (`/faq`)** sử dụng bố cục chia hai cột nhưng chưa có container nền bo góc rộng tương tự như trang giới thiệu.
- **Trang Con người MDFoods (`/hrbp`)** sử dụng nền trắng phẳng (`bg-white`), chiều rộng giới hạn ở mức `max-w-4xl` và không có bóng đổ hay bo góc của card bài viết.

## 3. Yêu cầu Chi tiết

### 3.1. Layout Tiêu chuẩn (Layout gốc làm mẫu từ `/mdfoods`)

Tất cả 4 trang trên cần được chuyển đổi sang cấu trúc CSS/HTML tương tự như sau:
1. **Container chính (Main Wrapper):**
   - Class nền: `catalog-page-bg min-h-screen`
   - Class bọc nội dung: `mx-auto w-full max-w-350 px-4 pb-10 pt-6 sm:px-6 sm:pb-12 sm:pt-8 md:px-8 md:pt-10` (Giới hạn chiều rộng tối đa ở mức `max-w-350` tương đương khoảng `1400px` nhưng được padding và thiết kế đồng bộ).
2. **Thẻ Bài viết (Article Card Container):**
   - Class: `mt-4 rounded-[28px] bg-(--catalog-surface) p-5 shadow-[0_10px_30px_rgba(15,23,42,0.08)] sm:mt-5 sm:p-8 md:p-10 lg:p-12`
3. **Thanh điều hướng (Breadcrumb Navigation):**
   - Hiển thị đầu trang bài viết, sử dụng component `<Breadcrumb />` hoặc định dạng điều hướng nhất quán với liên kết quay về trang chủ.
4. **Tiêu đề trang (Page Header):**
   - Dòng chữ nhỏ (Kicker): `text-xs font-semibold uppercase tracking-[0.4em] text-primary-1/80` (Ví dụ: `MDFOODS`, `HỎI & ĐÁP`, v.v.)
   - Tiêu đề chính (`h1`): `mt-3 text-2xl font-black text-(--catalog-text) sm:text-3xl lg:text-4xl`
5. **Nút quay lại (Back Button):**
   - Cuối trang cần có nút quay lại trang chủ dạng: `inline-flex h-11 w-full items-center justify-center rounded-xl bg-primary-1 px-6 text-sm font-bold uppercase tracking-[0.08em] text-white transition-opacity hover:opacity-90 sm:w-auto`

---

### 3.2. Yêu cầu cho từng trang cụ thể

#### 3.2.1. Trang Liên hệ (`/contact`)
- **Thay đổi:** Bọc toàn bộ phần tiêu đề, banner thông báo và lưới 2 cột (Form liên hệ + Thông tin liên hệ) vào bên trong Thẻ Bài viết (`Article Card Container`) tiêu chuẩn.
- **Bố cục:** Giữ nguyên lưới 2 cột trên Desktop (Form bên trái, Info bên phải) nhưng điều chỉnh padding và khoảng cách cho phù hợp với diện tích hiển thị mới bên trong card bài viết.
- **Nút:** Cập nhật nút "Quay lại trang chủ" ở dưới cùng khớp với CSS nút tiêu chuẩn.

#### 3.2.2. Trang Con người MDFoods (`/hrbp`)
- **Thay đổi:** Thay thế thẻ `<main className="min-h-screen bg-white">` bằng layout tiêu chuẩn với lớp nền `catalog-page-bg`.
- **Cấu trúc:** Bọc phần nội dung bài viết và thông tin tuyển dụng vào trong Thẻ Bài viết (`Article Card Container`) bo góc `rounded-[28px]` có đổ bóng.
- **Khối nội dung:** Các khối thông tin "Mô hình HRBP", "Tầm nhìn tuyển dụng", "Cơ hội nghề nghiệp" nên được trình bày trong các box màu xám nhạt (`bg-[#f8fafc] rounded-2xl px-4 py-4 sm:px-5 sm:py-5`) giống như các section trên trang giới thiệu.

#### 3.2.3. Trang Hỏi & Đáp (`/faq`)
- **Thay đổi:** Toàn bộ khu vực chứa thanh bên (Aside chứa danh mục câu hỏi) và khu vực hiển thị câu hỏi (FaqPage) cần được đặt gọn gàng trong Thẻ Bài viết (`Article Card Container`) hoặc đồng bộ hóa màu nền và độ bo góc của các cột.
- **Giao diện:** Đảm bảo màu nền tổng thể của trang là `catalog-page-bg` và các khối điều hướng bên trái cùng với danh sách câu hỏi bên phải có đổ bóng và bo góc đồng đều, ăn khớp với thiết kế chung của hệ thống.

#### 3.2.4. Trang Chính sách Giao hàng (`/delivery-policy`)
- **Thay đổi:** Loại bỏ ảnh nền lớn `/delivery-back1.png` và lớp phủ tối màu để trang sáng sủa, dễ đọc hơn. Thay thế bằng lớp nền trang tiêu chuẩn `catalog-page-bg`.
- **Cấu trúc:** Đưa toàn bộ nội dung chính sách giao hàng vào trong Thẻ Bài viết (`Article Card Container`).
- **Nội dung:** Giữ nguyên tính năng thu gọn/mở rộng chính sách (Collapsible parts) và nút toggle, nhưng căn chỉnh và phối hợp font chữ cho đồng bộ với trang giới thiệu.

## 4. Tiêu chí Hoàn thành (Acceptance Criteria)

| # | Tiêu chí |
|---|----------|
| 1 | Cả 4 trang (Liên hệ, Con người MDFoods, Hỏi & Đáp, Chính sách Giao hàng) đều sử dụng màu nền trang nhất quán (`catalog-page-bg`). |
| 2 | Nội dung của các trang được hiển thị bên trong các thẻ bài viết (`article`) bo góc `rounded-[28px]` với bóng đổ `shadow-[0_10px_30px_rgba(15,23,42,0.08)]`. |
| 3 | Loại bỏ hoàn toàn ảnh nền `/delivery-back1.png` trên trang Chính sách Giao hàng. |
| 4 | Kích thước container và khoảng cách lề (padding) trên desktop và mobile đồng bộ hoàn toàn với cấu trúc trang giới thiệu (`/mdfoods`). |
| 5 | Các tiêu đề trang hiển thị đúng cỡ chữ, khoảng cách và kiểu chữ (font-weight) theo chuẩn chung. |
| 6 | Nút "Quay về Trang chủ" hoặc liên kết quay lại ở cuối mỗi trang hoạt động chính xác và có style đồng nhất. |
| 7 | Giao diện hiển thị tốt, responsive trên mọi kích thước màn hình (Mobile, Tablet, Desktop) mà không bị vỡ khung hình. |
| 8 | Hỗ trợ hiển thị và chuyển đổi ngôn ngữ (VI/EN) bình thường trên tất cả các trang sau khi cập nhật layout. |
