# Yêu cầu Chức năng: Trang "Khách hàng Thân thiết" (Loyalty Page)

## 1. Tổng quan

Chức năng này cung cấp một trang chuyên biệt giới thiệu về "Chính sách Khách hàng Thân thiết" của MDFoods (MDFoods' Loyalty Policy) nhằm cung cấp thông tin chi tiết về chương trình tích điểm, phân hạng thành viên, đặc quyền ưu đãi và điều khoản sử dụng cho các khách hàng B2B.

- **Tên link trên Navigation / Footer:** `Khách hàng Thân thiết | MDFoods' Loyalty`
- **Tiêu đề trang (Page Title / Meta Title):** `Chính sách Khách hàng Thân thiết của MDFoods | MDFoods' Loyalty Policy`
- **Liên quan:** 
  - [FEATURE-004: Hỗ trợ Đa Ngôn Ngữ (EN/VI)](/mdfoods.vn/feature-004-i18n-en-vi.md)
  - [FEATURE-008: Cải Tiến Giao Diện Chuyển Đổi Ngôn Ngữ (VI | EN với Highlight)](/mdfoods.vn/feature-008-language-switcher-ui.md)

## 2. Vấn đề hiện tại / Bối cảnh

- Hiện tại hệ thống MDFoods chưa có trang thông tin chính thức giới thiệu về chính sách khách hàng thân thiết.
- Khách hàng B2B không nắm rõ cơ chế tích lũy điểm M-Point, các hạng thành viên cũng như các đặc quyền ưu đãi tương ứng khi mua hàng.
- Thiếu trang thông tin chi tiết để tối ưu SEO cho từ khóa "Khách hàng thân thiết MDFoods" hoặc "Chính sách ưu đãi MDFoods".

## 3. Yêu cầu Chức năng

### 3.1. Vị trí hiển thị đường dẫn (Link Placement)

- Link truy cập trang phải được hiển thị rõ ràng tại:
  - Menu Navigation chính (hoặc phần menu thả xuống của tài khoản khách hàng).
  - Footer trang web dưới nhóm liên kết "Về MDFoods" hoặc "Hỗ trợ & Chính sách".
- Anchor text hiển thị tương ứng theo ngôn ngữ hiện tại:
  - Tiếng Việt: `Khách hàng Thân thiết`
  - Tiếng Anh: `MDFoods' Loyalty`

### 3.2. Hiển thị nội dung đa ngôn ngữ

Trang web phải hiển thị đầy đủ và chính xác nội dung chính sách theo ngôn ngữ hiện tại mà người dùng lựa chọn (VI/EN). Nội dung chi tiết như sau:

#### 3.2.1. Nội dung Tiếng Việt (VI)

##### CHÍNH SÁCH KHÁCH HÀNG THÂN THIẾT CỦA MDFOODS

MDFoods áp dụng chính sách ưu đãi thường xuyên cho những khách hàng đã đồng hành và có những đóng góp tích cực cho sự phát triển bền vững của mình.

Tuỳ vào từng đặc điểm mua hàng của khách hàng và mức độ đóng góp trong từng giai đoạn, chúng tôi sẽ thiết lập phân hạng khách hàng theo các qui tắc được định sẵn trong hệ thống phân hạng khách hàng thân thiết của MDFoods.

Hệ thống phân hạng này được mô tả rõ ràng, chi tiết và cập nhật. Các chỉ số liên quan được thiết lập trong hệ thống và hệ thống tự động phân hạng khách hàng theo các tiêu chí tương ứng dựa vào các giao dịch mua hàng thực tế của khách hàng với MDFoods. 

Vì vậy, mức phân hạng khách hàng thân thiết cho từng khách hàng được cập nhật liên tục theo một chu kỳ nhất định - hệ thống sẽ tái đánh giá và phân hạng lại cấp độ phù hợp cho khách hàng trong từng giai đoạn, tuỳ cấp độ phân hạng khách hàng đang có mà chu kỳ đánh giá cũng khác nhau.

Các yếu tố liên quan đến việc đánh giá và thiết lập phân hạng khách hàng thân thiết của MDFoods:
- Phân khúc khách hàng.
- Giá trị mua hàng.
- Tần suất mua hàng.

Có thể hiểu, MDFoods là cộng sự đắc lực của bạn khi chúng tôi đóng vai trò cung ứng hàng hoá, nguyên liệu phục vụ cho hoạt động sản xuất, kinh doanh của bạn. Bên cạnh đó, MDFoods còn là một hạng mục đầu tư hiệu quả của bạn khi chúng tôi chia sẻ lợi ích với bạn từ những đóng góp thiết thực của bạn vào MDFoods.

*Chúng tôi đã tập hợp những quan tâm, thắc mắc thường thấy của khách hàng và đã xây dựng [bộ những câu hỏi đáp](/[lang]/faq) để cung cấp chi tiết, rõ ràng hơn về những thông tin liên quan, nhằm phục vụ quí vị được tốt hơn. Vui lòng vào xem [Thông tin Hỏi - Đáp](/[lang]/faq) để khám phá thêm về MDFoods chúng tôi.*

[Quay về Trang chủ](/[lang])

---

#### 3.2.2. Nội dung Tiếng Anh (EN)

##### MDFOODS LOYALTY POLICY

MDFoods applies a regular reward policy for customers who have been loyal and made positive contributions to its sustainable development.

Depending on each customer's purchasing characteristics and level of contribution at each stage, we will establish customer rankings according to predefined rules in MDFoods' loyalty program.

This ranking system is clearly described, detailed, and up-to-date. Relevant indicators are established in the system, and the system automatically ranks customers according to corresponding criteria based on their actual purchase transactions with MDFoods.

Therefore, the loyalty level for each customer is continuously updated at a certain cycle – the system will re-evaluate and re-rank the customer to the appropriate level at each stage, with the evaluation cycle varying depending on the current customer ranking.

Factors involved in evaluating and establishing loyalty programs for MDFoods customers include:
- Customer segment.
- Purchase value.
- Purchase frequency.

In essence, MDFoods is your valuable partner, supplying goods and raw materials for your production and business operations. Furthermore, MDFoods is a profitable investment, as we share the benefits with you from your practical contributions to MDFoods.

*We have compiled common customer concerns and questions and created a [Q&A section](/[lang]/faq) to provide more detailed and clear information, in order to better serve you. Please visit the [Q&A section](/[lang]/faq) to learn more about MDFoods.*

[Back to Homepage](/[lang])

### 3.3. Yêu cầu về Giao diện & Trải nghiệm (UI/UX)

- Giao diện phải tương thích tốt trên cả Desktop và Mobile (Responsive Design).
- Thiết kế đẹp mắt, thoáng, dễ đọc, phù hợp với phong cách chung của các trang chính sách khác.
- Các liên kết (Hyperlinks) phải được hiển thị rõ ràng và có hiệu ứng hover tương tác tốt.

## 4. Tiêu chí Hoàn thành (Acceptance Criteria)

| # | Tiêu chí |
|---|----------|
| 1 | Trang "Khách hàng Thân thiết" có thể truy cập được từ URL dạng `/vi/loyalty-program` hoặc `/en/loyalty-program`. |
| 2 | Tiêu đề trang (Meta Title) hiển thị chính xác theo ngôn ngữ lựa chọn. |
| 3 | Link chuyển đổi ngôn ngữ hoạt động chính xác trên trang này, khi chuyển đổi ngôn ngữ thì nội dung bài viết tự động chuyển đổi tương ứng (VI ↔ EN). |
| 4 | Hiển thị đầy đủ các đoạn văn và danh sách thuộc tính (Phân khúc, Giá trị, Tần suất mua hàng) theo đúng văn bản yêu cầu. |
| 5 | Nút "bộ những câu hỏi đáp" / "Thông tin Hỏi - Đáp" dẫn chính xác đến trang FAQ tương ứng theo ngôn ngữ (`/[lang]/faq`). |
| 6 | Nút "Quay về Trang chủ" dẫn chính xác đến trang chủ của nền tảng (`/[lang]`). |
| 7 | Giao diện responsive tốt trên mobile và desktop, căn lề và giãn dòng hợp lý. |
| 8 | Đầy đủ anchor link trên Footer/Header dẫn đến đúng trang. |
