# UC-002 — Kế hoạch chuyển đổi LFarm sang NestJS / NextJS (TypeScript)

> **Mục tiêu:** Thay thế toàn bộ stack C# .NET (ASP.NET MVC 5 + ASP.NET Web API 2 + SQL Server Stored Procedures) bằng **NextJS 14+ (App Router)** ở frontend và **NestJS** ở backend, giữ nguyên toàn bộ tính năng hiện có.  
> Cập nhật: 2026-04-27

---

## 1. Mapping công nghệ: Cũ → Mới

| Thành phần | Legacy (C#) | New (TypeScript) |
|------------|-------------|-----------------|
| Frontend rendering | ASP.NET MVC 5 Razor Views | **NextJS 14** (App Router, Server Components) |
| API layer | ASP.NET Web API 2 (`DongPhatAPI`) | **NestJS** (REST modules) |
| Database access | SQL Server Stored Procedures (via DLL) | **TypeORM** hoặc **Prisma** + SQL Server |
| Authentication | Session-based Bearer Token | **JWT** (stateless) hoặc NextAuth.js |
| Caching | In-memory cache (MemoryCache, 15 phút) | **Redis** (NestJS CacheModule) |
| i18n | Cookie `langs` + JSON file | **next-intl** (NextJS) |
| CAPTCHA | Server-side PNG (System.Drawing) | **@nestjs/throttler** + `svg-captcha` hoặc Google reCAPTCHA v3 |
| Static content | Hardcoded Razor `.cshtml` | NextJS MDX hoặc CMS (Sanity/Strapi) |
| Search | MVC action + partial render | NextJS Server Action hoặc NestJS search endpoint |
| Deployment | IIS / Windows Server | Docker + Kubernetes (k8s) |

---

## 2. Kiến trúc mới đề xuất

```
┌─────────────────────────────────────────────────┐
│  Browser                                         │
└────────────────┬────────────────────────────────┘
                 │ HTTPS
┌────────────────▼────────────────────────────────┐
│  NextJS 14 (App Router)                         │
│  ─ Server Components (SSR/SSG)                  │
│  ─ Client Components (interactive)              │
│  ─ Route Handlers (BFF / thin proxy)            │
│  ─ next-intl (vi/en/zh)                         │
└────────────────┬────────────────────────────────┘
                 │ Internal HTTP / gRPC
┌────────────────▼────────────────────────────────┐
│  NestJS API Service                             │
│  ─ AuthModule (JWT)                             │
│  ─ ArticleModule (Tạp Chí)                      │
│  ─ NewsletterModule (Bản Tin)                   │
│  ─ FaqModule (Hỏi Đáp)                          │
│  ─ ContactModule (Liên hệ)                      │
│  ─ CategoryModule (Danh mục)                    │
│  ─ CaptchaModule                                │
│  ─ CacheModule (Redis)                          │
└────────────────┬────────────────────────────────┘
                 │ TypeORM / Prisma
┌────────────────▼────────────────────────────────┐
│  SQL Server (existing schema)                   │
│  hoặc PostgreSQL (nếu migrate data)             │
└─────────────────────────────────────────────────┘
```

---

## 3. Phân rã module NestJS

### 3.1 AuthModule

**Mục đích:** Thay thế `TonkenAttribute` + `config.GetToken()`.

```typescript
// auth.service.ts
async getServiceToken(): Promise<string>  // Internal service account JWT
```

- Service account: `LFarm@email.com`
- Token cache: Redis với TTL = 3600s
- Guard: `JwtAuthGuard` cho toàn bộ internal endpoints

---

### 3.2 CategoryModule (`/api/categories`)

**Thay thế:** `api/DongPhatAPI/GetListLoaiForumNavbar`

| Method | Endpoint | Mô tả |
|--------|----------|-------|
| GET | `/api/categories/navbar` | Danh sách danh mục tạp chí cho navbar |

```typescript
// category.entity.ts
@Entity('tblLoaiForum')
class Category {
  @Column() IDLoaiForum: string;
  @Column() TenLoaiForum: string;
  @Column() IDLoaiCha: string;
}
```

---

### 3.3 ArticleModule (`/api/articles`)

**Thay thế:** `getDanhSachThaoLuan`, `getDanhSachThaoLuanTrangChu`, `getThongTinThaoLuan`

| Method | Endpoint | Mô tả |
|--------|----------|-------|
| GET | `/api/articles` | Danh sách bài viết (paginated) |
| GET | `/api/articles/homepage` | Bài viết nổi bật trang chủ |
| GET | `/api/articles/:id` | Chi tiết bài viết |
| GET | `/api/articles/:id/comments` | Danh sách bình luận |
| GET | `/api/articles/search?q=` | Tìm kiếm bài viết |

```typescript
// article.entity.ts
@Entity('tblThaoLuan')
class Article {
  @PrimaryColumn() IDThaoLuan: string;
  @Column() Subject: string;
  @Column() Tips: string;
  @Column() NoiDung: string;
  @ManyToOne(() => Category) Category: Category;
  @OneToMany(() => ArticlePicture) Pictures: ArticlePicture[];
  @OneToMany(() => ArticleVideo) Videos: ArticleVideo[];
}

@Entity('tblThaoLuanPicture')
class ArticlePicture {
  @Column() ThuTu: number;
  @Column() TenFile: string;
  @Column() GhiChu: string;
  @Column() IsMacDinh: boolean;
}
```

**Query params:**
```
GET /api/articles?categoryId=&lang=vi&page=1&limit=12
GET /api/articles/search?q={keyword}&categoryId=&lang=vi
```

---

### 3.4 NewsletterModule (`/api/newsletters`)

**Thay thế:** `getDanhSachBanTin`, `GetDanhSachBanTinTrangChu`, `getThongTinBanTin`

| Method | Endpoint | Mô tả |
|--------|----------|-------|
| GET | `/api/newsletters` | Danh sách bản tin |
| GET | `/api/newsletters/homepage` | Bản tin trang chủ |
| GET | `/api/newsletters/:id` | Chi tiết bản tin |

---

### 3.5 FaqModule (`/api/faqs`)

**Thay thế:** `GetDanhSachHoiDap`

| Method | Endpoint | Mô tả |
|--------|----------|-------|
| GET | `/api/faqs` | Danh sách câu hỏi theo nhóm |

```typescript
@Entity('tblHoiDap')
class Faq {
  @PrimaryColumn() IDHoiDap: string;
  @Column() CauHoi: string;
  @Column() DapAn: string;
  @Column() TenNhomHoiDap: string;
}
```

---

### 3.6 ContactModule (`/api/contact`)

**Thay thế:** `PostGuiTinNhan`

| Method | Endpoint | Mô tả |
|--------|----------|-------|
| POST | `/api/contact` | Gửi tin nhắn liên hệ |

```typescript
// create-contact.dto.ts
class CreateContactDto {
  @IsNotEmpty() @MaxLength(200) hoTen: string;
  @IsEmail() email: string;
  @IsPhoneNumber('VN') phone: string;
  @IsNotEmpty() @MaxLength(500) subject: string;
  @IsNotEmpty() @MaxLength(2000) noiDung: string;
}
```

**Validation:** `class-validator` + `class-transformer`  
**Rate limiting:** `@nestjs/throttler` (max 5 requests/minute per IP)

---

### 3.7 CaptchaModule (`/api/captcha`)

**Thay thế:** `CaptchaController.rendercaptcha()` (server-side PNG)

| Method | Endpoint | Mô tả |
|--------|----------|-------|
| GET | `/api/captcha` | Tạo CAPTCHA, trả về `{token, imageData}` |
| POST | `/api/captcha/verify` | Verify CAPTCHA token + answer |

**Implementation:** `svg-captcha` package. Token lưu Redis với TTL = 120s (thay Session).

---

## 4. NextJS Pages & Routes

### Route mapping: Legacy → NextJS App Router

| Legacy Route | NextJS Route | Rendering |
|-------------|-------------|-----------|
| `/` | `app/[locale]/page.tsx` | SSG + ISR (revalidate 900s) |
| `/dstapchi?id=&page=` | `app/[locale]/tap-chi/page.tsx` | SSR (dynamic params) |
| `/tapchidetail?id=` | `app/[locale]/tap-chi/[id]/page.tsx` | SSR + ISR |
| `/newsdetail?id=` | `app/[locale]/ban-tin/[id]/page.tsx` | SSR + ISR |
| `/nhanhieudefault` | `app/[locale]/thuong-hieu/page.tsx` | SSG |
| `/organicsdetail` | `app/[locale]/thuong-hieu/organics/page.tsx` | SSG |
| `/naturedetail` | `app/[locale]/thuong-hieu/nature/page.tsx` | SSG |
| `/lfarmEcodetail` | `app/[locale]/thuong-hieu/eco/page.tsx` | SSG |
| `/dieukhoan` | `app/[locale]/dieu-khoan/page.tsx` | SSG |
| `/chinhsach` | `app/[locale]/chinh-sach/page.tsx` | SSG |
| `/thongbao` | `app/[locale]/thong-bao/page.tsx` | Client Component |

### Server Actions (thay AJAX)

```typescript
// app/actions/contact.ts
'use server'
export async function submitContact(formData: FormData) { ... }

// app/actions/search.ts
'use server'
export async function searchArticles(query: string, categoryId?: string) { ... }
```

### i18n với next-intl

```
messages/
  vi.json   ← {key: "giá trị tiếng Việt"}
  en.json
  zh.json

middleware.ts  ← locale detection từ cookie/header
```

---

## 5. Kế hoạch triển khai (Sprint)

### Sprint 1 — Hạ tầng & Backend Core (2 tuần)

**Developer tasks:**
- [ ] Khởi tạo NestJS project, cấu hình TypeORM + SQL Server
- [ ] Implement `AuthModule` (JWT, service account token)
- [ ] Implement `CategoryModule` (navbar endpoint)
- [ ] Implement `ArticleModule` (list + detail + search)
- [ ] Cấu hình Redis cache (CategoryModule, ArticleModule)
- [ ] Unit tests cho Service layer

**DevOps tasks:**
- [ ] Tạo Dockerfile cho NestJS service
- [ ] Cấu hình Redis instance (k8s-local)
- [ ] CI pipeline: lint + test + build

**QA tasks:**
- [ ] Viết test cases cho Category API
- [ ] Viết test cases cho Article API (list, detail, search, pagination)
- [ ] So sánh response với legacy API

---

### Sprint 2 — Backend Remaining + NextJS Setup (2 tuần)

**Developer tasks:**
- [ ] Implement `NewsletterModule`
- [ ] Implement `FaqModule`
- [ ] Implement `ContactModule` (validation, rate limiting)
- [ ] Implement `CaptchaModule` (svg-captcha + Redis token)
- [ ] Khởi tạo NextJS 14 project (App Router, next-intl vi/en/zh)
- [ ] Layout component, Navbar (CategoryModule), Footer
- [ ] Trang chủ (`/`) — SSG/ISR

**DevOps tasks:**
- [ ] Dockerfile cho NextJS service
- [ ] Environment variables management (k8s Secrets)

**QA tasks:**
- [ ] Test Newsletter, FAQ, Contact APIs
- [ ] Test CAPTCHA flow (generate → verify)
- [ ] Test rate limiting Contact endpoint

---

### Sprint 3 — Frontend Pages (2 tuần)

**Developer tasks:**
- [ ] Page: Danh sách Tạp Chí (`/tap-chi`) — phân trang + lọc danh mục
- [ ] Page: Chi tiết Tạp Chí (`/tap-chi/[id]`) — nội dung + bình luận + related
- [ ] Page: Chi tiết Bản Tin (`/ban-tin/[id]`)
- [ ] Component: CAPTCHA + Form Liên hệ (Server Action)
- [ ] Component: Search (AJAX replace → Server Action + `useOptimistic`)
- [ ] i18n: dịch tất cả strings sang `vi.json`, `en.json`, `zh.json`

**DevOps tasks:**
- [ ] Staging deployment (k8s-dpsrv)
- [ ] Ingress + TLS cho `lfarm.vn` staging

**QA tasks:**
- [ ] Regression test toàn bộ luồng trang chủ
- [ ] Test phân trang, tìm kiếm
- [ ] Test form liên hệ + CAPTCHA
- [ ] Test i18n (vi/en/zh)
- [ ] Responsive test (mobile/tablet/desktop)

---

### Sprint 4 — Static Pages, SEO, Performance (1 tuần)

**Developer tasks:**
- [ ] Trang Thương hiệu + 3 dòng sản phẩm (SSG — có thể dùng MDX)
- [ ] Trang Điều khoản / Chính sách (SSG)
- [ ] SEO: `metadata` API cho mọi trang, `sitemap.ts`, `robots.ts`
- [ ] `next/image` cho tất cả ảnh bài viết
- [ ] OpenGraph tags

**QA tasks:**
- [ ] Lighthouse audit (Performance, SEO, Accessibility)
- [ ] 404 / error boundary test

---

### Sprint 5 — Production Cutover (1 tuần)

**Developer tasks:**
- [ ] Feature flag: old IIS ↔ new k8s behind nginx
- [ ] Data migration validation (không migrate data — chỉ đọc từ DB hiện có)

**DevOps tasks:**
- [ ] Production deployment (k8s-dpsrv-prd)
- [ ] DNS cutover: `lfarm.vn` → new ingress
- [ ] Monitor: Grafana dashboard (request rate, error rate, latency)
- [ ] Rollback plan: revert DNS nếu có sự cố

**QA tasks:**
- [ ] Smoke test production
- [ ] Load test (k6): mô phỏng 100 concurrent users
- [ ] So sánh Visual Regression (Playwright screenshots: legacy vs new)

---

## 6. Rủi ro & Giảm thiểu

| Rủi ro | Mức độ | Giảm thiểu |
|--------|--------|-----------|
| Stored procedures phức tạp, khó map sang TypeORM | Cao | Giai đoạn 1: gọi SP trực tiếp qua `query()`, sau đó từng bước refactor sang entities |
| Schema SQL Server không có migration files | Trung bình | Reverse-engineer schema với `typeorm-model-generator` trước Sprint 1 |
| SEO regression (URL thay đổi) | Cao | Giữ nguyên URL slug ở NextJS; thêm `next/headers` redirects cho URL cũ |
| Cache stale khi content editor cập nhật | Thấp | ISR revalidate theo tag; hoặc on-demand revalidation từ CMS webhook |
| Legacy JS CAPTCHA gọi `Default.aspx/GetData` (WebForms) | Trung bình | Thay bằng `/api/captcha` NestJS endpoint; cập nhật JS client |
| Đa ngôn ngữ: Nội dung bài viết trong DB theo ngôn ngữ | Trung bình | Kiểm tra cột `Lang` trong `tblThaoLuan`; đảm bảo NestJS truyền đúng `lang` param |

---

## 7. Checklist chấp nhận (Definition of Done)

### Developer
- [ ] Tất cả API endpoints trả về cùng data structure với legacy
- [ ] Không có `any` type trong TypeScript
- [ ] Unit test coverage ≥ 80% (NestJS services)
- [ ] E2E tests cho critical flows (Playwright)

### DevOps
- [ ] CI/CD pipeline chạy full (lint → test → build → deploy)
- [ ] Health check endpoint: `GET /health`
- [ ] Liveness + Readiness probe cấu hình trong k8s
- [ ] Secrets không hardcode trong repo

### QA
- [ ] Tất cả trang hiển thị đúng trên Chrome, Firefox, Safari
- [ ] Mobile responsive (320px–1440px)
- [ ] Form liên hệ: validation đúng, message lưu vào `tblTinNhanWeb`
- [ ] CAPTCHA hoạt động đúng
- [ ] i18n: chuyển đổi vi/en/zh không bị lỗi layout
- [ ] Lighthouse Performance score ≥ 80

---

## 8. Cấu trúc thư mục đề xuất

```
lfarm/
├── apps/
│   ├── web/                    ← NextJS 14 (App Router)
│   │   ├── app/
│   │   │   ├── [locale]/
│   │   │   │   ├── page.tsx            ← Trang chủ
│   │   │   │   ├── tap-chi/
│   │   │   │   │   ├── page.tsx        ← Danh sách
│   │   │   │   │   └── [id]/page.tsx   ← Chi tiết
│   │   │   │   ├── ban-tin/[id]/page.tsx
│   │   │   │   ├── thuong-hieu/
│   │   │   │   ├── dieu-khoan/page.tsx
│   │   │   │   └── chinh-sach/page.tsx
│   │   │   ├── actions/
│   │   │   │   ├── contact.ts
│   │   │   │   └── search.ts
│   │   │   └── api/             ← Route Handlers (BFF thin proxy nếu cần)
│   │   ├── components/
│   │   ├── messages/            ← vi.json, en.json, zh.json
│   │   └── middleware.ts        ← next-intl locale detection
│   │
│   └── api/                    ← NestJS
│       └── src/
│           ├── auth/
│           ├── category/
│           ├── article/
│           ├── newsletter/
│           ├── faq/
│           ├── contact/
│           ├── captcha/
│           └── common/
│               ├── cache/
│               └── database/
├── docker-compose.yml
└── package.json                ← pnpm workspace
```
