# Yêu cầu Chức năng: Trang "Chính sách Thanh toán" (Payment Policy Page)

## 1. Tổng quan

Chức năng này cung cấp một trang giới thiệu về "Chính sách Thanh toán của Khách hàng" (Customer Payment Policy) nhằm cung cấp thông tin rõ ràng về điều kiện và phương thức thanh toán áp dụng chung tại MDFoods đối với từng phân hạng thành viên khách hàng B2B.

- **Tên link trên Navigation / Footer:** `Chính sách Thanh toán | Payment Policy`
- **Tiêu đề trang (Page Title / Meta Title):** `Chính sách Thanh toán của Khách hàng | Customer Payment Policy`
- **Liên quan:** [FEATURE-004: Hỗ trợ Đa Ngôn Ngữ (EN/VI)](/mdfoods.vn/feature-004-i18n-en-vi.md)

## 2. Bối cảnh / Vấn đề hiện tại

- Cần thống nhất quy định thanh toán trước/sau khi giao nhận đối với khách hàng tự do, thành viên thông thường và khách hàng đặc biệt để tối ưu dòng tiền và giảm thiểu rủi ro công nợ.

## 3. Yêu cầu Chức năng

### 3.1. Hiển thị nội dung đa ngôn ngữ

#### 3.1.1. Nội dung Tiếng Việt (VI)

Ngoài những điều khoản liên quan đến chính sách thanh toán đã ghi chi tiết trong hợp đồng giữa khách hàng và MDFoods thì chính sách thanh toán chung được áp dụng tại MDFoods như sau:
- Khách hàng chưa được phân cấp thành viên (khách hàng đã đăng ký thành viên nhưng chưa được xác lập cấp độ thành viên) thì phải thanh toán 100% giá trị đơn hàng (bao gồm phí dịch vụ giao hàng - nếu có) ngay sau khi chốt đơn hàng và trước khi tiến hành việc giao hàng.
- Khách hàng đã là thành viên nhưng không thuộc dạng khách hàng đặc biệt (được mô tả chi tiết chính sách riêng) thì thanh toán 30% (ba mươi phần trăm) giá trị đơn hàng (bao gồm phí dịch vụ giao hàng - nếu có) ngay sau khi chốt đơn đặt hàng và trước khi tiến hành việc giao hàng, thanh toán tiếp 70% (bảy mươi phần trăm) giá trị của đợt giao hàng ngay sau khi nhận hàng của đợt giao hàng đó.
- Khách hàng có thể thanh toán bằng tiền mặt (trường hợp giá trị thanh toán thấp hơn mức quy định của cơ quan quản lý nhà nước về vấn đề này) hay chuyển khoản. Việc giao hàng được xem là hoàn tất khi bên mua nhận đúng, đủ hàng và hoàn tất nghĩa vụ thanh toán cho bên bán.

Hóa đơn GTGT sẽ được MDFoods phát hành ngay sau khi hai bên hoàn tất việc giao hàng của từng đợt.

*Chúng tôi đã tập hợp những quan tâm, thắc mắc thường thấy của khách hàng và đã xây dựng [bộ những câu hỏi đáp](/[lang]/faq) để cung cấp chi tiết, rõ ràng hơn về những thông tin liên quan, nhằm phục vụ quí vị được tốt hơn. Vui lòng vào xem [Thông tin Hỏi - Đáp](/[lang]/faq) để khám phá thêm về MDFoods chúng tôi.*

[Quay về Trang chủ](/[lang])

#### 3.1.2. Nội dung Tiếng Anh (EN)

In addition to the payment terms detailed in the contract between the customer and MDFoods, the general payment policy applied at MDFoods is as follows:
- Customers who have not yet been assigned a membership level (customers who have registered as members but have not yet established a membership level) must pay 100% of the order value (including delivery service fees - if any) immediately after confirming the order and before delivery.
- Customers who are members but are not classified as special customers (described in a separate policy) must pay 30% (thirty percent) of the order value (including delivery service fees - if any) immediately after confirming the order and before delivery, and pay the remaining 70% (seventy percent) of the order value immediately upon receiving the goods for that delivery.
- Customers can pay by cash (if the payment value is lower than the amount stipulated by the state management agency on this matter) or bank transfer. Delivery is considered complete when the buyer receives the correct and complete goods and fulfills their payment obligations to the seller.

A VAT invoice will be issued by MDFoods immediately after both parties complete the delivery of each batch of goods.

*We have compiled common customer concerns and questions and created a [Q&A section](/[lang]/faq) to provide more detailed and clear information, in order to better serve you. Please visit the [Q&A section](/[lang]/faq) to learn more about MDFoods.*

[Back to Homepage](/[lang])

## 4. Tiêu chí Hoàn thành (Acceptance Criteria)

| # | Tiêu chí |
|---|----------|
| 1 | Trang có thể truy cập qua URL `/vi/payment-policy` và `/en/payment-policy`. |
| 2 | Meta title hiển thị chính xác theo ngôn ngữ lựa chọn. |
| 3 | Chuyển đổi ngôn ngữ hoạt động chính xác và tự động đổi content tương ứng. |
| 4 | Hiển thị đầy đủ các bullet point nội dung chính sách thanh toán bằng cả hai ngôn ngữ. |
