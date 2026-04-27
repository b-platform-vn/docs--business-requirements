# L'Farm - Tổng quan dự án

## Giới thiệu

**L'Farm** (lfarm.vn) là website thương hiệu nông sản từ An toàn đến Hữu cơ của Đông Phát. Website đóng vai trò là **landing page + content hub** giới thiệu thương hiệu, sản phẩm, và kết nối khách hàng với L'Farm.

- **IDKhoi** trong hệ thống B-Platform: `30`
- **UserWeb token**: `LFarm@email.com`
- **Loại ứng dụng**: ASP.NET MVC 5 (C#), multi-language (VI/EN)

## Phiên bản hiện tại (Legacy)

| Thành phần | Công nghệ |
|---|---|
| Frontend | ASP.NET MVC 5, Razor Views, Bootstrap 3 |
| Backend | ASP.NET Web API 2 (`DongPhatAPI`) |
| Database | SQL Server (via Stored Procedures) |
| Authentication | OAuth2 Bearer Token (session-based) |
| Caching | In-memory cache |
| i18n | Cookie-based (`langs`: `vn`/`en`), JSON translate file |

## Tài liệu liên quan

- [use-cases.md](lfarm.vn/use-cases.md) — Danh sách tất cả use case / feature
- [uc-001-feature-analysis.md](lfarm.vn/uc-001-feature-analysis.md) — Phân tích tính năng theo từng trang
- [uc-002-migration-plan.md](lfarm.vn/uc-002-migration-plan.md) — Kế hoạch chuyển đổi sang NestJS/NextJS
