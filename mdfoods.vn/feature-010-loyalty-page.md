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
- Khách hàng B2B không nắm rõ cơ chế tích lũy điểm M-Point, các hạng thành viên (Đồng, Bạc, Vàng, Kim Cương) cũng như các đặc quyền ưu đãi tương ứng khi mua hàng.
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

##### CHÍNH SÁCH KHÁCH HÀNG THÂN THIẾT MDFOODS

Chào mừng Quý khách đến với Chương trình Khách hàng Thân thiết của MDFoods (MDFoods' Loyalty Program) - chương trình đặc quyền dành riêng cho các đối tác B2B (Nhà hàng, Khách sạn, Chuỗi F&B, Bếp ăn công nghiệp, v.v.) đồng hành cùng MDFoods trên hành trình cung cấp thực phẩm sạch và chất lượng.

###### 1. Cơ Chế Tích Lũy Điểm (Loyalty Points)

Với mỗi đơn hàng hoàn thành thành công trên hệ thống MDFoods, Quý khách sẽ nhận được số điểm tích lũy tương ứng:
- **Tỷ lệ quy đổi tiêu chuẩn:** Mỗi **100.000 VND** thanh toán hợp lệ = **1 Điểm MDFoods (M-Point)**.
- **Cách thức tính:** Điểm tích lũy được tính trên tổng giá trị thanh toán thực tế của đơn hàng (sau khi đã áp dụng các chương trình khuyến mãi, giảm giá và không bao gồm phí vận chuyển).
- **Thời điểm cộng điểm:** Điểm sẽ được tự động cộng vào tài khoản của Quý khách ngay sau khi đơn hàng chuyển sang trạng thái **"Hoàn thành" (Delivered & Paid)**.

###### 2. Hạng Thành Viên & Đặc Quyền (Membership Tiers & Benefits)

Chương trình Khách hàng Thân thiết của MDFoods được chia làm 4 hạng thành viên dựa trên tổng chi tiêu tích lũy trong vòng **01 tháng dương lịch**:

*   **Hạng Đồng (Bronze)**
    *   **Điều kiện:** Mặc định cho mọi tài khoản đăng ký mới hoặc chi tiêu dưới 10.000.000 VND/tháng.
    *   **Đặc quyền:** Tích điểm với tỷ lệ tiêu chuẩn (100.000 VND = 1 M-Point).
*   **Hạng Bạc (Silver)**
    *   **Điều kiện:** Chi tiêu từ 10.000.000 VND đến dưới 30.000.000 VND/tháng.
    *   **Đặc quyền:**
        *   Hệ số tích điểm: **1.2x M-Points** (Tiết kiệm thêm 20% điểm thưởng).
        *   Ưu đãi giảm giá 1% trực tiếp trên mọi đơn hàng rau củ quả tươi.
*   **Hạng Vàng (Gold)**
    *   **Điều kiện:** Chi tiêu từ 30.000.000 VND đến dưới 50.000.000 VND/tháng.
    *   **Đặc quyền:**
        *   Hệ số tích điểm: **1.5x M-Points** (Tiết kiệm thêm 50% điểm thưởng).
        *   Ưu đãi giảm giá 2% trực tiếp trên mọi đơn hàng rau củ quả và thịt tươi.
        *   Miễn phí vận chuyển cho toàn bộ đơn hàng có giá trị từ 500.000 VND trở lên.
        *   Ưu tiên xử lý đơn hàng và giao hàng trong khung giờ vàng.
*   **Hạng Kim Cương (Diamond)**
    *   **Điều kiện:** Chi tiêu từ 50.000.000 VND/tháng trở lên.
    *   **Đặc quyền:**
        *   Hệ số tích điểm: **2.0x M-Points** (Nhân đôi điểm thưởng tích lũy).
        *   Ưu đãi giảm giá 3% trực tiếp trên toàn bộ danh mục sản phẩm của MDFoods.
        *   Miễn phí vận chuyển cho mọi đơn hàng (không giới hạn giá trị tối thiểu).
        *   Chăm sóc khách hàng VIP: Có chuyên viên hỗ trợ 24/7 và xử lý khiếu nại/đổi trả ưu tiên trong vòng 2 giờ.
        *   Quà tặng đặc quyền vào các dịp Lễ, Tết và ngày kỷ niệm thành lập doanh nghiệp của đối tác.

###### 3. Quy Định Sử Dụng Điểm & Đổi Thưởng

- **Giá trị quy đổi điểm:** **1 M-Point = 1.000 VND**.
- **Cách thức sử dụng:**
  - Khách hàng có thể sử dụng điểm tích lũy để trừ trực tiếp vào hóa đơn mua hàng ở bước Thanh toán (tối đa không quá 50% giá trị đơn hàng).
  - Đổi các gói voucher mua sắm hoặc quà tặng độc quyền của MDFoods tại "Cửa hàng đổi quà".
- **Hạn sử dụng điểm:** Điểm tích lũy của mỗi năm sẽ có giá trị sử dụng đến hết ngày **31 tháng 12** của năm đó. Điểm chưa sử dụng hết sẽ tự động hết hạn và reset về 0.

###### 4. Điều Khoản Chung

- Chương trình áp dụng cho tất cả khách hàng doanh nghiệp và hộ kinh doanh cá nhân mua hàng trực tiếp trên hệ thống website/app của MDFoods.
- MDFoods có quyền thay đổi cơ chế tích lũy điểm, giá trị quy đổi và các đặc quyền của hạng thành viên tùy theo định hướng chiến lược nhưng sẽ thông báo trước ít nhất 15 ngày trên website/app.
- Mọi trường hợp có dấu hiệu gian lận điểm, hệ thống có quyền tạm khóa tài khoản để xác minh và hủy bỏ số điểm không hợp lệ mà không cần báo trước.

---

#### 3.2.2. Nội dung Tiếng Anh (EN)

##### MDFOODS LOYALTY POLICY

Welcome to MDFoods' Loyalty Program - a premium reward initiative designed exclusively for our B2B partners (Restaurants, Hotels, F&B Chains, Central Kitchens, etc.) who accompany MDFoods on the journey of delivering clean, traceably-sourced, and high-quality food.

###### 1. Loyalty Points Accumulation (M-Points)

For every successfully completed order on the MDFoods system, you will earn loyalty points:
- **Standard Exchange Rate:** Every **100,000 VND** spent = **1 MDFoods Point (M-Point)**.
- **Calculation Method:** Points are calculated based on the actual payment amount of the order (after applying promotions/discounts and excluding shipping fees).
- **Points Crediting:** Points will be automatically credited to your account once the order status is updated to **"Completed" (Delivered & Paid)**.

###### 2. Membership Tiers & Exclusive Benefits

The MDFoods Loyalty Program features 4 membership tiers based on your accumulated spending within **01 calendar month**:

*   **Bronze Tier**
    *   **Requirement:** Default for all new registrations or monthly spending under 10,000,000 VND.
    *   **Benefits:** Standard points accumulation rate (100,000 VND = 1 M-Point).
*   **Silver Tier**
    *   **Requirement:** Monthly spending from 10,000,000 VND to under 30,000,000 VND.
    *   **Benefits:**
        *   Earning multiplier: **1.2x M-Points** (Earn 20% more bonus points).
        *   Direct 1% discount on all fresh vegetables and fruits.
*   **Gold Tier**
    *   **Requirement:** Monthly spending from 30,000,000 VND to under 50,000,000 VND.
    *   **Benefits:**
        *   Earning multiplier: **1.5x M-Points** (Earn 50% more bonus points).
        *   Direct 2% discount on all fresh vegetables, fruits, and fresh meat products.
        *   Free shipping on all orders of 500,000 VND and above.
        *   High-priority order processing and delivery during peak hours.
*   **Diamond Tier**
    *   **Requirement:** Monthly spending of 50,000,000 VND and above.
    *   **Benefits:**
        *   Earning multiplier: **2.0x M-Points** (Double your earned loyalty points).
        *   Direct 3% discount on the entire MDFoods product catalog.
        *   Free shipping on all orders (no minimum order value required).
        *   Dedicated VIP Support: 24/7 dedicated customer care agent and prioritized return/refund handling within 2 hours.
        *   Exclusive corporate gifts on Holidays, New Year, and the partner's business anniversary.

###### 3. Points Redemption Rules

- **Points Redemption Value:** **1 M-Point = 1,000 VND**.
- **How to use:**
  - Customers can apply M-Points directly as a discount during checkout (up to 50% of the total order value).
  - Exchange M-Points for shopping vouchers or exclusive MDFoods branded gifts at our "Redemption Store".
- **Points Expiration:** Accumulated points in a calendar year will be valid until **December 31st** of that year. Any unused points will automatically expire and reset to 0.

###### 4. General Terms

- The program applies to all corporate clients and individual businesses purchasing directly on the MDFoods website/app.
- MDFoods reserves the right to modify the points accumulation rate, redemption values, and tier privileges in accordance with our business strategy, with a minimum of 15 days' advance notice on the website/app.
- In cases of suspected fraud or point abuse, the system reserves the right to temporarily lock the account for investigation and cancel invalid points without prior notice.

### 3.3. Yêu cầu về Giao diện & Trải nghiệm (UI/UX)

- Giao diện phải tương thích tốt trên cả Desktop và Mobile (Responsive Design).
- Thiết kế dạng bảng so sánh (Comparison Table) trực quan cho các đặc quyền hạng thành viên (Bronze, Silver, Gold, Diamond) để khách hàng dễ theo dõi.
- Sử dụng các biểu tượng (icons) minh họa sinh động cho các điều khoản và cơ chế tích điểm.

## 4. Tiêu chí Hoàn thành (Acceptance Criteria)

| # | Tiêu chí |
|---|----------|
| 1 | Trang "Khách hàng Thân thiết" có thể truy cập được từ URL dạng `/vi/loyalty-policy` hoặc `/en/loyalty-policy` (hoặc tương đương theo quy hoạch URL của dự án). |
| 2 | Tiêu đề trang (Meta Title) hiển thị chính xác theo ngôn ngữ lựa chọn. |
| 3 | Link chuyển đổi ngôn ngữ hoạt động chính xác trên trang này, khi chuyển đổi ngôn ngữ thì nội dung bài viết tự động chuyển đổi tương ứng (VI ↔ EN). |
| 4 | Hiển thị đầy đủ các phần: Giới thiệu chương trình, Cơ chế tích điểm, Bảng so sánh đặc quyền các hạng thành viên, Quy định sử dụng/đổi điểm và Điều khoản chung. |
| 5 | Giao diện responsive tốt, không lỗi tràn viền (overflow), bảng so sánh hạng thành viên dễ đọc trên mobile. |
| 6 | Đầy đủ anchor link trên Footer/Header dẫn đến đúng trang. |
