# UC-001 — Phân tích tính năng theo từng trang (LFarm Legacy)

> **Công nghệ hiện tại:** ASP.NET MVC 5 (C#) + DongPhatAPI (ASP.NET Web API 2) + SQL Server  
> **IDKhoi:** 30 | **UserWeb:** `LFarm@email.com`  
> **API Base URL:** `http://192.168.1.202:8083/`  
> Cập nhật: 2026-04-27

---

## Tổng quan kiến trúc

```
Browser
  │
  ▼
ASP.NET MVC 5 (Lfarm/)          ← Thin display layer
  │  Auth: Bearer token (session-cached)
  │  Cache: InMemoryCache (TTL 900s)
  │  i18n: Cookie "langs" (vi/en/zh) + JSON file
  ▼
DongPhatAPI (REST)               ← Business logic layer
  │  Stored Procedures only
  ▼
SQL Server                       ← Data layer
```

---

## Trang 1: Trang chủ (`/`)

**Controller:** `defaultController.Index()`  
**Views:** `Index.cshtml`, `PartialIndex.cshtml`, `DsTapChiTrangChu.cshtml`, `DSTapChiDetail.cshtml`, `PartialDetailBanTin.cshtml`, `PartialCauhoi.cshtml`, `PartialMenuTapChi.cshtml`

### Luồng tính năng

| # | Tính năng | Mô tả |
|---|-----------|-------|
| 1 | Social links session | Đọc config (Zalo, Facebook Messenger, WhatsApp, Viber, Skype) → lưu vào `Session` để render trên layout |
| 2 | Navigation bar Tạp Chí | Lấy danh sách danh mục tạp chí (navbar) |
| 3 | Danh sách Tạp Chí trang chủ | Hiển thị các bài viết tạp chí nổi bật trên homepage |
| 4 | Danh sách Bản Tin trang chủ | Hiển thị newsletter section trên homepage |
| 5 | Newsletter detail partial | Chi tiết bản tin trong homepage |
| 6 | FAQ section | Câu hỏi thường gặp |

### API Calls

| Partial/Action | HTTP | Endpoint | Parameters |
|----------------|------|----------|-----------|
| `PartialMenuTapChi` | GET | `api/DongPhatAPI/GetListLoaiForumNavbar` | `UserWeb`, `IDLoaiCha`, `Lang` |
| `DsTapChiTrangChu` | GET | `api/DongPhatAPI/getDanhSachThaoLuanTrangChu` | `UserWeb`, `domaintapchi`, `Lang` |
| `DSTapChiDetail` | GET | `api/DongPhatAPI/getDanhSachThaoLuanTrangChu` | `UserWeb`, `domaintapchi`, `Lang` |
| `DanhSachBanTinIndex` | GET | `api/DongPhatAPI/GetDanhSachBanTinTrangChu` | `UserWeb`, `Lang`, `domain` (domainbantin) |
| `PartialIndex` | GET | `api/DongPhatAPI/getDanhSachBanTin` | `UserWeb`, `Lang`, `domain` |
| `PartialDetailBanTin` | GET | `api/DongPhatAPI/getDanhSachBanTin` | `UserWeb`, `Lang`, `domain` |
| `PartialCauhoi` | GET | `api/DongPhatAPI/GetDanhSachHoiDap` | `UserWeb`, `Lang` |

### Database Tables

| Table | Thao tác | SP (gọi qua DongPhatAPI) |
|-------|----------|--------------------------|
| `tblLoaiForum` | READ | `sp_API_W_DSLoaiForumNavbar` |
| `tblThaoLuan` | READ | `sp_API_W_DSThaoLuanCapThuN`, `sp_API_W_DSBanTinTrangChu`, `sp_API_W_DSBanTin` |
| `tblHoiDap` | READ | `sp_API_W_DSDanhSachHoiDap` |

---

## Trang 2: Danh sách Tạp Chí (`/dstapchi`)

**Controller:** `defaultController.PageListNews(id, page, slang)`  
**Views:** `PageListNews.cshtml`, `PageListNewssearch1.cshtml` (partial)

### Luồng tính năng

| # | Tính năng | Mô tả |
|---|-----------|-------|
| 1 | Phân trang bài viết | Lấy danh sách bài viết tạp chí, phân trang 12 bài/trang |
| 2 | Lọc theo danh mục | Lọc bài viết theo `IDLoaiForum` |
| 3 | Tìm kiếm bài viết | AJAX search từ khoá, replace partial `.partialsearch` |

### API Calls

| Action | HTTP | Endpoint | Parameters |
|--------|------|----------|-----------|
| `PageListNews` | GET | `api/DongPhatAPI/getDanhSachThaoLuan` | `UserWeb`, `domaintapchi`, `IDLoaiForum`, `Lang` |
| `PageListNewssearch1` | GET | `api/DongPhatAPI/getDanhSachThaoLuan` | `UserWeb`, `domaintapchi`, `IDLoaiForum`, `Lang`, `text` |

### Database Tables

| Table | Thao tác | SP |
|-------|----------|-----|
| `tblThaoLuan` | READ | `sp_API_W_DSThaoLuanCapThuN` |
| `tblLoaiForum` | READ | `sp_API_W_DSThaoLuanCapThuN` (JOIN) |
| `tblThaoLuanPicture` | READ | `sp_API_W_ThongTinThaoLuanPicture` (thumbnail) |

### Client-side AJAX

```javascript
// js/searchbaiviet/searchbaiviet.js
GET /default/PageListNewssearch1?text={keyword}
→ replaces .partialsearch div
```

---

## Trang 3: Chi tiết bài viết Tạp Chí (`/tapchidetail`)

**Controller:** `defaultController.ThongTinThaoLuanDetail(id, IDLoaiForum, slang, page)`  
**View:** `ThongTinThaoLuanDetail.cshtml`

### Luồng tính năng

| # | Tính năng | Mô tả |
|---|-----------|-------|
| 1 | Chi tiết bài viết | Nội dung đầy đủ, ảnh, video |
| 2 | Breadcrumb | Đường dẫn phân cấp danh mục |
| 3 | Bình luận | Hiển thị danh sách bình luận, phân cấp (cha/con) |
| 4 | Bài viết liên quan | Sidebar danh sách bài cùng danh mục |

### API Calls

| Action | HTTP | Endpoint | Parameters |
|--------|------|----------|-----------|
| `ThongTinThaoLuanDetail` | GET | `api/DongPhatAPI/getThongTinThaoLuan` | `UserWeb`, `IDThaoLuan`, `domaintapchi`, `Lang` |
| `getDanhSachThaoLuan` (related) | GET | `api/DongPhatAPI/getDanhSachThaoLuan` | `UserWeb`, `domaintapchi`, `IDLoaiForum`, `Lang` |
| `GetBinhLuanTapChi` | GET | `api/DongPhatAPI/GetListBinhLuanThaoLuan` | `UserWeb`, `IDThaoLuan` |

### Database Tables

| Table | Thao tác | SP |
|-------|----------|-----|
| `tblThaoLuan` | READ | `sp_API_W_ThongTinThaoLuan` |
| `tblThaoLuanPicture` | READ | `sp_API_W_ThongTinThaoLuanListPicture` |
| `tblThaoLuanVideo` | READ | `sp_API_W_ThongTinThaoLuanListVideo` |
| `tblBinhLuanThaoLuan` | READ | `sp_API_W_ListBinhLuanThaoLuan` |

### Data Models

```
ThongTinThaoLuanAPI
  IDThaoLuan      string
  Subject         string   ← Tiêu đề
  Tips            string   ← Tóm tắt
  NoiDung         string   ← Nội dung HTML
  IDLoaiForum     string
  TenLoaiForum    string
  ListPicture     List<ThongTinBanTinListPictureAPI>
    ThuTu, TenFile, GhiChu, IsMacDinh
  ListVideo       List<ThongTinBanTinListVideoAPI>
    ThuTu, TenFile, GhiChu

ListBinhLuanThaoLuanAPI
  IDBinhLuanThaoLuan        string
  HoTenThanhVien            string
  NoiDung                   string
  Ngay                      string
  IDBinhLuanThaoLuanCha     string
  Muc                       int    ← depth level
```

---

## Trang 4: Chi tiết Bản Tin (`/newsdetail`)

**Controller:** `NewsController.NewsDetail(id, slang)`  
**View:** `Views/News/NewsDetail.cshtml`

### Luồng tính năng

| # | Tính năng | Mô tả |
|---|-----------|-------|
| 1 | Chi tiết bản tin | Nội dung bản tin (Newsletter), ảnh, video |

### API Calls

| Action | HTTP | Endpoint | Parameters |
|--------|------|----------|-----------|
| `NewsDetail` | GET | `api/DongPhatAPI/getThongTinBanTin` | `UserWeb`, `IDThaoLuan`, `DomainBanTin`, `Lang` |

### Database Tables

| Table | Thao tác | SP |
|-------|----------|-----|
| `tblThaoLuan` | READ | `sp_API_W_ThongTinBanTin` |
| `tblThaoLuanPicture` | READ | `sp_API_W_ThongTinThaoLuanListPicture` |
| `tblThaoLuanVideo` | READ | `sp_API_W_ThongTinThaoLuanListVideo` |

---

## Trang 5: Liên hệ / Gửi tin nhắn (Contact Form)

**Controller:** `defaultController.guitinnhan(f)` [POST]  
**View:** Inline modal form trong layout (xử lý bằng `js/formtinnhan/formtinnhan.js`)

### Luồng tính năng

| # | Tính năng | Mô tả |
|---|-----------|-------|
| 1 | Form liên hệ | Nhập Họ tên, Email, Điện thoại, Tiêu đề, Nội dung |
| 2 | CAPTCHA | Sinh ảnh captcha server-side, validate trước khi submit |
| 3 | Submit | POST lên controller → gọi API → lưu DB → redirect `/thongbao` |
| 4 | Thông báo | Trang kết quả gửi tin nhắn thành công/thất bại |

### CAPTCHA Flow

```
GET /Captcha/rendercaptcha
  → tạo ảnh PNG ngẫu nhiên
  → lưu đáp án vào Session["captcha"]
  → trả về đường dẫn ảnh
```

### API Calls

| Action | HTTP | Endpoint | Parameters |
|--------|------|----------|-----------|
| `guitinnhan` | POST | `api/DongPhatAPI/PostGuiTinNhan` | JSON body: `TinNhanAPI` |

```json
// TinNhanAPI payload
{
  "HoTen":   "Nguyễn Văn A",
  "Email":   "a@example.com",
  "Phone":   "0901234567",
  "Subject": "Tiêu đề",
  "NoiDung": "Nội dung",
  "WebSite": "LFarm.vn"
}
```

### Database Tables

| Table | Thao tác | SP |
|-------|----------|-----|
| `tblTinNhanWeb` | **WRITE** | `sp_API_W_LuuTinNhan` |

**Columns:** `IDTinNhanWeb`, `HoTenUnicode`, `Email`, `Phone`, `SubjectUnicode`, `NoiDungUnicode`, `WebSite`

---

## Trang 6: Thương hiệu (`/nhanhieudefault`)

**Controller:** `defaultController.NhanHieuDefault()`  
**View:** `NhanHieuDefault.cshtml`

> Trang tĩnh — không có API call, không truy cập Database.

### Luồng tính năng

| # | Tính năng | Mô tả |
|---|-----------|-------|
| 1 | Brand landing page | Giới thiệu thương hiệu L'Farm, liên kết đến 3 dòng sản phẩm |

---

## Trang 7: Chi tiết dòng sản phẩm

| Route | Controller Action | View |
|-------|------------------|------|
| `/organicsdetail` | `Organicsdetail()` | `Organicsdetail.cshtml` |
| `/naturedetail` | `Naturedetail()` | `Naturedetail.cshtml` |
| `/lfarmEcodetail` | `LfarmEcodetail()` | `LfarmEcodetail.cshtml` |

> Tất cả đều là **trang tĩnh** — không có API call, không truy cập Database.  
> Nội dung hardcoded trong Razor view.

---

## Trang 8: Điều khoản & Chính sách

| Route | Controller Action | View |
|-------|------------------|------|
| `/dieukhoan` | `dieukhoansudung()` | `dieukhoansudung.cshtml` |
| `/chinhsach` | `chinhsachbaomat()` | `chinhsachbaomat.cshtml` |

> Trang tĩnh — không có API call, không truy cập Database.

---

## Tóm tắt: Toàn bộ API endpoints được sử dụng

| Endpoint | Phương thức | Sử dụng ở trang |
|----------|-------------|-----------------|
| `api/DongPhatAPI/GetListLoaiForumNavbar` | GET | Tất cả (navbar) |
| `api/DongPhatAPI/getDanhSachThaoLuanTrangChu` | GET | Trang chủ |
| `api/DongPhatAPI/getDanhSachThaoLuan` | GET | Danh sách Tạp Chí, Chi tiết |
| `api/DongPhatAPI/GetDanhSachBanTinTrangChu` | GET | Trang chủ |
| `api/DongPhatAPI/getDanhSachBanTin` | GET | Trang chủ |
| `api/DongPhatAPI/getThongTinThaoLuan` | GET | Chi tiết Tạp Chí |
| `api/DongPhatAPI/getThongTinBanTin` | GET | Chi tiết Bản Tin |
| `api/DongPhatAPI/GetListBinhLuanThaoLuan` | GET | Chi tiết Tạp Chí |
| `api/DongPhatAPI/GetDanhSachHoiDap` | GET | Trang chủ (FAQ) |
| `api/DongPhatAPI/PostGuiTinNhan` | **POST** | Liên hệ |

## Tóm tắt: Toàn bộ Database Tables

| Table | Đọc | Ghi | Trang sử dụng |
|-------|-----|-----|--------------|
| `tblThaoLuan` | ✅ | — | Tạp Chí, Bản Tin, Trang chủ |
| `tblLoaiForum` | ✅ | — | Tạp Chí (danh mục, navbar) |
| `tblThaoLuanPicture` | ✅ | — | Chi tiết bài viết |
| `tblThaoLuanVideo` | ✅ | — | Chi tiết bài viết |
| `tblBinhLuanThaoLuan` | ✅ | — | Chi tiết Tạp Chí |
| `tblTinNhanWeb` | — | ✅ | Form Liên hệ |
| `tblHoiDap` | ✅ | — | FAQ (Trang chủ) |

---

## Hệ thống phụ trợ

### Authentication
- `TonkenAttribute` (ActionFilter): Kiểm tra `Session["authToken"]`. Nếu hết hạn → gọi `config.GetToken()` lấy Bearer token mới từ API.
- Token được cache trong Session.

### Caching
- `InMemoryCaChe` với TTL mặc định **900 giây (15 phút)**.
- Áp dụng cho các partial views gọi API (menu, trang chủ partials).

### Đa ngôn ngữ (i18n)
- Cookie `langs`: `vn` (0), `en` (1), `zh` (2).
- Client-side: `GET muiple_languge_defautl.json` → binding qua `[data-translate]`.
- Server-side: truyền `Lang` parameter vào mọi API call.

### CAPTCHA
- Server-side PNG generation (`/Captcha/rendercaptcha`).
- Đáp án lưu trong `Session["captcha"]`.
- Validate trước khi POST form liên hệ.
