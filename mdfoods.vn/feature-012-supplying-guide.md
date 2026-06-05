# Yêu cầu Chức năng: Trang "Hướng dẫn Chào hàng" (Supplying Guide Page)

## 1. Tổng quan

Chức năng này cung cấp một trang chuyên biệt hướng dẫn quy trình, cách thức và thông tin cần cung cấp khi đối tác muốn trở thành nhà cung cấp (Supplier) cho MDFoods.

- **Tên link trên Navigation / Footer:** `Hướng dẫn Chào hàng | Supplying Guide` (nằm ngay dưới link "Hướng dẫn Mua hàng")
- **Tiêu đề trang (Page Title / Meta Title):** `Hướng dẫn Chào hàng | Supplying Guide`
- **Liên quan:** [FEATURE-004: Hỗ trợ Đa Ngôn Ngữ (EN/VI)](/mdfoods.vn/feature-004-i18n-en-vi.md)

## 2. Bối cảnh / Vấn đề hiện tại

- Nhu cầu mở rộng nguồn cung từ các nhà nông, trang trại và nhà máy chế biến thực phẩm đòi hỏi một quy trình rõ ràng hướng dẫn họ cách gửi thông tin đề nghị hợp tác chào hàng đến MDFoods.

## 3. Yêu cầu Chức năng

### 3.1. Hiển thị nội dung đa ngôn ngữ

#### 3.1.1. Nội dung Tiếng Việt (VI)

**Chào mừng bạn đến với MDFoods!**

***Nếu bạn là:***
- Nhà sản xuất nông sản: nông hộ, trang trại.
- Vựa thu mua nông sản tại các địa phương.
- Nhà chế biến thực phẩm: xưởng, nhà máy.
- Nhà nhập khẩu độc quyền thực phẩm.

Muốn phân phối sản phẩm của mình ra thị trường thông qua kênh bán hàng của MDFoods. Bạn có thể xúc tiến cơ hội bán hàng của mình với chúng tôi bằng các cách thức như:
- ***Cách 1***: Gửi các thông tin liên quan của nhà cung cấp cho chúng tôi qua email [supplier@MDFoods.vn](mailto:supplier@MDFoods.vn).
- ***Cách 2***: [Liên lạc trực tiếp với chúng tôi](/[lang]/contact) thông qua các kênh liên lạc chính thức của MDFoods mà chúng tôi đã công bố trên website này.

***Các thông tin cơ bản của Nhà cung cấp MDFoods mà chúng tôi cần bạn cung cấp bao gồm:***
1. Pháp nhân Đăng ký kinh doanh của Nhà cung cấp.
2. Thông tin liên lạc cơ bản của người liên hệ phụ trách phía nhà cung cấp (Tên, Số điện thoại, Email, Chức vụ).
3. Các chứng nhận liên quan đến phương thức canh tác đối với nông sản hay chứng nhận liên quan đến Vệ sinh An toàn Thực phẩm đối với thực phẩm chế biến.
4. Thông tin chi tiết về sản phẩm và Bảng giá của nhà cung cấp.
5. Đề nghị phương thức hợp tác theo nhu cầu đặc thù (nếu có).

Bộ phận phụ trách Quản lý nhà cung cấp của MDFoods tiếp nhận thông tin và chủ động liên lạc lại với quý vị để cùng xúc tiến cơ hội hợp tác giữa hai bên.

**Chân thành cảm ơn sự quan tâm và thiện chí hợp tác của Quý vị dành cho MDFoods!**

*Chúng tôi đã tập hợp những quan tâm, thắc mắc thường thấy của khách hàng và đã xây dựng [bộ những câu hỏi đáp](/[lang]/faq) để cung cấp chi tiết, rõ ràng hơn về những thông tin liên quan, nhằm phục vụ quí vị được tốt hơn. Vui lòng vào xem [Thông tin Hỏi - Đáp](/[lang]/faq) để khám phá thêm về MDFoods chúng tôi.*

[Quay về Trang chủ](/[lang])

#### 3.1.2. Nội dung Tiếng Anh (EN)

**Welcome to MDFoods!**

***If you are:***
- An agricultural producer: smallholders, farms.
- A local agricultural product purchasing center.
- A food processor: a factory, a plant.
- An exclusive food importer.

***Want to distribute your products to the market through MDFoods' sales channel? You can promote your sales opportunities with us in the following ways:***
- ***Method 1***: Send us your supplier information via email to [supplier@MDFoods.vn](mailto:supplier@MDFoods.vn).
- ***Method 2***: [Contact us](/[lang]/contact) directly through MDFoods' official contact channels published on this website.

***The basic supplier information we need you to provide includes:***
- The supplier's business registration number.
- Basic contact information of the supplier's representative (Name, Phone Number, Email, Job Title).
- Relevant certifications related to farming methods for agricultural products or Food Safety and Hygiene certifications for processed foods.
- Detailed product information and supplier price list.
- Proposed cooperation methods based on specific needs (if any).

MDFoods' Supplier Management Department will receive this information and proactively contact you to further explore cooperation opportunities.

**Thank you for your interest and willingness to cooperate with MDFoods!**

*We have compiled common customer concerns and questions and created a [Q&A section](/[lang]/faq) to provide more detailed and clear information, in order to better serve you. Please visit the [Q&A section](/[lang]/faq) to learn more about MDFoods.*

[Back to Homepage](/[lang])

## 4. Tiêu chí Hoàn thành (Acceptance Criteria)

| # | Tiêu chí |
|---|----------|
| 1 | Trang có thể truy cập qua URL `/vi/supplying-guide` và `/en/supplying-guide`. |
| 2 | Meta title hiển thị chính xác theo ngôn ngữ lựa chọn. |
| 3 | Chuyển đổi ngôn ngữ hoạt động chính xác và tự động đổi content tương ứng. |
| 4 | Email link `supplier@MDFoods.vn` là link `mailto:` hoạt động chính xác. |
| 5 | Các liên kết FAQ và Liên hệ dẫn tới đúng trang theo ngôn ngữ hiện tại. |
