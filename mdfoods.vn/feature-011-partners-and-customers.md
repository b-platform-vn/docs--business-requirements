# Yêu cầu Chức năng: Trang "Nguồn cung và Khách hàng" (Partners and Customers Page)

## 1. Tổng quan

Chức năng này cung cấp một trang chuyên biệt giới thiệu về "Nguồn cung và Khách hàng" của MDFoods (MDFoods' Partners and Customers) nhằm làm rõ vai trò của MDFoods trong chuỗi cung ứng thực phẩm, đồng thời liệt kê các đối tác nguồn cung và nhóm khách hàng mục tiêu của công ty.

- **Tên link trên Navigation / Footer:** `Nguồn cung, Khách hàng | Partners and Customers` (nằm ngay dưới link "Giới thiệu MDFoods" và ngay trên "Đối tác Tin cậy")
- **Tiêu đề trang (Page Title / Meta Title):** `Nguồn cung và Khách hàng của MDFoods | MDFoods’ Partners and Customers`
- **Tên path link:** `Đối tác tin cậy | Trusted Partner` đổi thành `Đối tác Tin cậy | Trusted Partner` để tạo điểm nhấn.
- **Liên quan:** [FEATURE-004: Hỗ trợ Đa Ngôn Ngữ (EN/VI)](/mdfoods.vn/feature-004-i18n-en-vi.md)

## 2. Bối cảnh / Vấn đề hiện tại

- Cần làm nổi bật vị trí và vai trò kiểm soát chất lượng, điều phối chuỗi cung ứng nông sản của MDFoods.
- Cần cung cấp thông tin minh bạch về nguồn hàng và đối tượng phục vụ của MDFoods cho khách hàng và đối tác B2B.

## 3. Yêu cầu Chức năng

### 3.1. Vị trí hiển thị đường dẫn (Link Placement)

- Hiển thị trên cả Menu Navigation (chân trang và top bar).
- Menu trên top bar phải tự động đóng khi người dùng click bên ngoài menu (focus-out).
- Tương thích tốt trên cả Mobile và PC.

### 3.2. Hiển thị nội dung đa ngôn ngữ

#### 3.2.1. Nội dung Tiếng Việt (VI)

**Ở vị trí của mình, MDFoods đóng vai trò kiểm soát chất lượng hàng hoá và điều phối chuỗi cung ứng nông sản, thực phẩm.**

**Nguồn hàng của MDFoods:**
- Hộ nông dân, Trang trại sản xuất nông nghiệp.
- Nhà máy chế biến thực phẩm.
- Những nhà xuất khẩu quốc tế.

**Khách hàng mục tiêu của chúng tôi:**
- Kênh bán lẻ thực phẩm (MT, GT).
- Kênh nhà hàng, dịch vụ ăn uống (F&B).
- Nhà máy chế biến thực phẩm.
- Nhà nhập khẩu thực phẩm quốc tế.

*Chúng tôi đã tập hợp những quan tâm, thắc mắc thường thấy của khách hàng và đã xây dựng [bộ những câu hỏi đáp](/[lang]/faq) để cung cấp chi tiết, rõ ràng hơn về những thông tin liên quan, nhằm phục vụ quí vị được tốt hơn. Vui lòng vào xem [Thông tin Hỏi - Đáp](/[lang]/faq) để khám phá thêm về MDFoods chúng tôi.*

[Quay về Trang chủ](/[lang])

#### 3.2.2. Nội dung Tiếng Anh (EN)

**In its role, MDFoods is responsible for controlling the quality of goods and coordinating the agricultural products and food supply chain.**

***MDFoods' Sources:***
- Farmers and agricultural production farms.
- Food processing plants.
- International exporters.

***Our Target Customers:***
- Food retail channels (MT, GT).
- Restaurants and food service channels (F&B).
- Food processing plants.
- International food importers.

Hãy [liên hệ cho chúng tôi](/[lang]/contact) để chúng ta cùng kết nối và phát triển cơ hội kinh doanh của mình.

*We have compiled common customer concerns and questions and created a [Q&A section](/[lang]/faq) to provide more detailed and clear information, in order to better serve you. Please visit the [Q&A section](/[lang]/faq) to learn more about MDFoods.*

[Back to Homepage](/[lang])

## 4. Tiêu chí Hoàn thành (Acceptance Criteria)

| # | Tiêu chí |
|---|----------|
| 1 | Trang có thể truy cập qua URL `/vi/partners-and-customers` và `/en/partners-and-customers`. |
| 2 | Meta title hiển thị chính xác theo ngôn ngữ lựa chọn. |
| 3 | Chuyển đổi ngôn ngữ hoạt động chính xác và tự động đổi content tương ứng. |
| 4 | Hiển thị đúng định dạng danh sách và font chữ in đậm/in nghiêng như yêu cầu. |
| 5 | Menu top bar tự đóng khi click ra ngoài. |
| 6 | Menu chân trang và top bar cập nhật đúng thứ tự liên kết. |
