# Master Layout

| | |
|---|---|
| **Author** | _TBD_ |
| **Updated** | 2026.07.17 |
| **Product** | [MDFoods](/products/mdfoods/README.md) |
| **Priority** | P0 |
| **Figma** | [MDFoods.vn — Master Layout](https://www.figma.com/design/LhJxbmnpQ1D7CY3SIvyCU9/MDFoods.vn?node-id=51-2&t=nCwAzyQIrTYsPRAK-4) |

---

## Problem

Business customers browsing the MDFoods platform need consistent navigation, contact access, and branding across every page. Without a shared layout shell, pages feel disconnected, customers can't reach support or search quickly, and they lose orientation within the site.

- Users expect a persistent header with logo, product navigation, search, contact channels, and account access.
- Bilingual (VI/EN) business customers need an always-available language switcher.
- Users expect a footer that surfaces every informational, policy, and service page in one place.
- Trust signals (certifications) must be visible to reassure B2B buyers about quality.
- The layout must work on both desktop and mobile (responsive off-canvas navigation).

## Target Audiences

- **B2B buyers / business customers** — need fast access to contact channels and search before requesting quotes.
- **Bilingual customers (VI/EN)** — need to switch language from anywhere on the site.
- **Guest visitors** — need visible trust signals (certifications) and easy access to login/register.
- **Registered customers** — need quick access to account, orders, and quotes.
- **Mobile users** — need the full navigation available through a responsive off-canvas menu.

## Solution

A global **Master Layout** component wrapping all pages, providing:

- **Header** — A three-tier header: (1) top utility bar with contact channels, info menus, and language switcher; (2) main bar with logo, product mega-menu, and search; (3) action area with cart and authentication.
- **Certification Bar** — A horizontally-scrollable row of quality/organic certification badges shown above the footer.
- **Footer** — Four link columns (About, Business Policies, Online Services, Info Corner) plus brand/mission block, social links, and legal info.
- **Responsive Navigation** — A mobile off-canvas (hamburger) drawer mirroring the desktop navigation.

## Goals (Business Impact)

- Provide a consistent brand and navigation experience across all pages, reducing customer confusion and drop-off.
- Increase contact-channel engagement (Zalo, Messenger, phone) by keeping them reachable from every page.
- Drive product discovery and search usage from any page.
- Reduce support load by surfacing all policy/service pages through the footer.
- Build B2B trust via visible certifications, supporting quote conversion.
- Ensure 100% of pages render within a consistent Master Layout.

## Functions/Usecases

### Header — Top Utility Bar

- **Zalo OA link** — User sees the Zalo OA icon in the top utility bar. When user clicks it, the MDFoods Zalo Official Account chat opens in a new tab (`https://zalo.me/924539963250166636`).
- **Messenger link** — User sees the Messenger icon in the top utility bar. When user clicks it, the MDFoods Messenger chat opens in a new tab (`https://m.me/MDFoodsvietnam`).
- **Phone number** — User sees the hotline number (+84 907 450 144) in the top utility bar. When user clicks it on mobile, the device dials the number (`tel:+84907450144`).
- **Khuyến mãi (Promotions) link** — User sees the "Khuyến mãi" link in the top utility bar. When user clicks it, they are taken to the Promotions page (`/{locale}/promotions`).
- **MDFoods dropdown** — User sees the "MDFoods" menu in the top utility bar. When user clicks it, a dropdown opens with company-info links. When user clicks any company-info link, they are taken to that page:
  - Business Model (`/{locale}/mdfoods`)
  - Partners and Customers (`/{locale}/partners-and-customers`)
  - Trusted Partner (`/{locale}/partners`)
  - HRBP (`/{locale}/hrbp`)
  - Activities (`/{locale}/activities`)
- **Góc thông tin (Information corner) dropdown** — User sees the "Information corner" menu in the top utility bar. When user clicks it, a dropdown opens with informational links. When user clicks an information link, they are taken to that page:
  - Magazine (`/{locale}/magazine`)
  - F&B Community (`/{locale}/fnb-community`)
  - Food Recipes (`/{locale}/recipes`)
- **Language switcher (VI/EN)** — User sees "VI" and "EN" toggle buttons in the top utility bar. When user clicks one, the site reloads in the selected language (`/vi` ↔ `/en`).

### Header — Main Bar

- **Logo** — User sees the MDFoods logo in the main bar. When user clicks it, they are taken to the home page (`/{locale}`).
- **Sản phẩm (Product Catalog) mega-menu** — User sees the "Product Catalog" button in the main bar. When user clicks it, a mega-menu opens with top-level product categories. When user clicks a category, they are taken to that category's product list (`/{locale}/category/{slug}`).
- **Search input** — User sees the search box in the main bar with placeholder "Search Product by: SKU, Name, Category, Brand, Description". When user types a query and submits it, they are taken to the search results page for that query.

### Header — Action Area

- **Cart icon** — User sees the cart icon with an item-count badge in the action area. When user clicks it, they are taken to the cart page (`/{locale}/cart`).
- **Đăng nhập (Sign in) link** — User sees the "Sign in" link in the action area if they are not authenticated. When user clicks it, they are taken to the login page (`/{locale}/login`).
- **Đăng ký (Sign up) link** — User sees the "Sign up" link in the action area if they are not authenticated. When user clicks it, they are taken to the registration page (`/{locale}/register`).
- **Account menu** — User sees their account name/avatar in the action area if authenticated. When user clicks it, a menu opens with Profile, Orders, Quotes, and Logout links. When user clicks a menu item, they navigate to that page or log out.

### Responsive / Mobile

- **Hamburger trigger** — User sees the hamburger icon on mobile. When user clicks it, an off-canvas drawer opens with search, product navigation, info menus, auth links, and the language switcher. When user clicks an item in the drawer, it behaves the same as the desktop counterpart.

### Certification Bar

- **Certification badges** — User sees a horizontally-scrollable row of certification badges above the footer. When user clicks a badge, it opens an external page explaining that certification. Badges include: GlobalGAP, USDA Organic, EU Organic, ACO, JAS, ISO 22000, HACCP, FSSC 22000, BRC, HALAL.
- **Scroll controls** — User sees left/right arrow controls next to the badge row. When user clicks an arrow, the badge row scrolls in that direction.

### Footer — Brand Block

- **Mission statement** — User sees the MDFoods mission statement in the footer brand block.
- **Fanpage link** — User sees the Facebook Fanpage icon in the footer brand block. When user clicks it, the MDFoods Fanpage opens in a new tab (`https://www.facebook.com/MDFoodsvietnam`).
- **Zalo OA link** — User sees the Zalo icon in the footer brand block. When user clicks it, the MDFoods Zalo Official Account chat opens in a new tab (`https://zalo.me/924539963250166636`).

### Footer — Giới thiệu MDFoods (About MDFoods)

- **Mô hình Kinh doanh (Business Model)** — User sees this footer link. When user clicks it, they are taken to `/{locale}/mdfoods`.
- **Nguồn cung & Khách hàng (Partners and Customers)** — User sees this footer link. When user clicks it, they are taken to `/{locale}/partners-and-customers`.
- **Đối tác Tin cậy (Trusted Partner)** — User sees this footer link. When user clicks it, they are taken to `/{locale}/partners`.
- **Con người MDFoods (MDFoods' HRBP)** — User sees this footer link. When user clicks it, they are taken to `/{locale}/hrbp`.
- **Bảng tin Hoạt động (Activities News)** — User sees this footer link. When user clicks it, they are taken to `/{locale}/activities`.

### Footer — Chính sách Kinh doanh (Business Policy)

- **Hướng dẫn Chào hàng (Supplying Guide)** — User sees this footer link. When user clicks it, they are taken to `/{locale}/supplying-guide`.
- **Hướng dẫn Mua hàng (Buying Guide)** — User sees this footer link. When user clicks it, they are taken to `/{locale}/shopping-guide`.
- **CS Giao hàng (Delivery Policy)** — User sees this footer link. When user clicks it, they are taken to `/{locale}/delivery-policy`.
- **CS Thanh toán (Payment Policy)** — User sees this footer link. When user clicks it, they are taken to `/{locale}/payment-policy`.
- **CS Đổi & Trả hàng (Returns & Exchanges Policy)** — User sees this footer link. When user clicks it, they are taken to `/{locale}/returns-and-exchanges`.
- **Khách hàng Thân thiết (MDFoods' Loyalty)** — User sees this footer link. When user clicks it, they are taken to `/{locale}/loyalty-program`.

### Footer — Dịch vụ Trực tuyến (Online Services)

- **Đăng ký Thành viên (Register as a Member)** — User sees this footer link. When user clicks it, they are taken to `/{locale}/register`.
- **Đăng nhập (Login to Your Page)** — User sees this footer link. When user clicks it, they are taken to `/{locale}/login`.
- **Đăng ký Nhận bảng tin (Submit for Newsletter)** — User sees this footer action. When user clicks it, a newsletter subscription form opens.
- **Hỏi đáp (FAQ)** — User sees this footer link. When user clicks it, they are taken to `/{locale}/faq`.
- **Liên hệ (Contact to MDFoods)** — User sees this footer link. When user clicks it, they are taken to `/{locale}/contact`.
- **Khuyến mãi (Promotions)** — User sees this footer link. When user clicks it, they are taken to `/{locale}/promotions`.

### Footer — Góc thông tin (Information Corner)

- **Magazine** — User sees this footer link. When user clicks it, they are taken to `/{locale}/magazine`.
- **F&B Community** — User sees this footer link. When user clicks it, they are taken to `/{locale}/fnb-community`.
- **Food Recipes** — User sees this footer link. When user clicks it, they are taken to `/{locale}/recipes`.

### Footer — Legal Info

- **Company details** — User sees the company details area in the footer. When user reads it, they can confirm the site operator, tax ID, address, hotline, and email.
- **Copyright** — User sees the copyright text. When user clicks it, nothing happens (static information).
- **Privacy Policy link** — User sees the privacy policy link. When user clicks it, they are taken to `/{locale}/privacy-policy`.
- **Terms of Use link** — User sees the terms of use link. When user clicks it, they are taken to `/{locale}/terms-of-use`.

### Internal Links

- **Breadcrumb links** — User sees the breadcrumb trail on inner pages. When user clicks a breadcrumb item, they are taken to that ancestor page.
