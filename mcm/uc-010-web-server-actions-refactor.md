# UC-010: Refactor Web MCM Messenger — Server Actions + Route Handler SSE Proxy

**Service:** `web-mcm-messenger`  
**Loại:** Use Case (Refactor Kiến trúc)  
**Ngày tạo:** 2026-04-26  
**Trạng thái:** Draft (v3 — sau re-review BM, QA)  
**Nguồn yêu cầu:** Technical Requirement — loại bỏ gọi API từ client

---

## Tóm tắt

Hiện tại, `web-mcm-messenger` gọi API trực tiếp từ browser (client-side fetch) thông qua các biến môi trường `NEXT_PUBLIC_*`. Kiến trúc này có hai vấn đề:

1. **NEXT_PUBLIC_* bị bake lúc build** — không thể thay đổi backend URL mà không rebuild Docker image.
2. **Web Worker không đọc được `process.env`** — phải hardcode URL tuyệt đối vào build, gây CORS issues trên production.

Use case này mô tả việc refactor toàn bộ lớp API communication sang:
- **Server Actions** cho tất cả mutations và data queries
- **Route Handler proxy** cho SSE stream (realtime)
- **httpOnly cookies** thay localStorage để lưu token

Sau refactor, Dockerfile không còn `NEXT_PUBLIC_*` nào — runtime env vars trong K8s là đủ.

---

## Yêu cầu nghiệp vụ

1. Web App phải có khả năng kết nối tới các backend service khác nhau (local/stg/prd) chỉ bằng cách thay đổi K8s env vars, không cần rebuild image.
2. Token xác thực không được expose ra client-side JavaScript (bảo mật).
3. Offline-first architecture phải được giữ nguyên: dữ liệu vẫn đọc từ IndexedDB, các thay đổi offline vẫn được sync khi có kết nối.
4. Realtime sync qua SSE vẫn hoạt động bình thường.

---

## Yêu cầu kỹ thuật

### API Communication

| Loại operation | Cơ chế hiện tại | Cơ chế mới |
|---|---|---|
| Login | `fetch(NEXT_PUBLIC_AUTH_SERVICE_URL + "/api/auth/login")` | `loginAction()` — Server Action |
| Refresh token | `fetch(NEXT_PUBLIC_AUTH_SERVICE_URL + "/api/auth/refresh_token")` | `refreshTokenAction()` — Server Action |
| Logout | `fetch(NEXT_PUBLIC_AUTH_SERVICE_URL + "/api/auth/revoke")` | `logoutAction()` — Server Action |
| Validate token | `fetch(NEXT_PUBLIC_AUTH_SERVICE_URL + "/api/auth/validate")` | `validateTokenAction()` — Server Action |
| Đổi mật khẩu | `fetch(NEXT_PUBLIC_AUTH_SERVICE_URL + "/api/auth/change-password")` | `changePasswordAction()` — Server Action |
| Lấy danh sách Khối | `fetch(NEXT_PUBLIC_AUTH_SERVICE_URL + "/api/auth/divisions")` | `getDivisionsAction()` — Server Action |
| Chọn Khối (switch tenant) | `localStorage.setItem('selected_tenant', id)` | `setTenantAction(tenantId)` — Server Action (set regular cookie) |
| Pull dữ liệu | `fetch(NEXT_PUBLIC_INTERNAL_CHANNEL_URL + "/api/internal/sync/pull")` | `syncPullAction()` — Server Action |
| Push outbound | `fetch(NEXT_PUBLIC_INTERNAL_CHANNEL_URL + "/api/internal/sync/push")` | `syncPushAction()` — Server Action |
| Pull events (catchup) | `fetch(NEXT_PUBLIC_INTERNAL_CHANNEL_URL + "/api/internal/sync/pull/events")` | `syncPullEventsAction()` — Server Action |
| SSE stream | `new EventSource(NEXT_PUBLIC_INTERNAL_CHANNEL_URL + "/api/internal/sync/sse")` | `new EventSource('/api/sync/sse')` → Route Handler proxy |
| Upload file | `fetch(NEXT_PUBLIC_FILESTORE_SERVICE_URL + "/api/files/upload")` | `uploadFileAction()` — Server Action |

### Lưu trữ Token

| | Hiện tại | Mới |
|---|---|---|
| `access_token` | `localStorage` (JS đọc được) | `httpOnly cookie` (chỉ server đọc được) |
| `refresh_token` | `localStorage` | `httpOnly cookie` |
| `selected_tenant` | `localStorage` | Regular cookie (JS cần đọc để query IndexedDB theo tenantId) |
| `user` | `localStorage` | **Regular cookie** (`user_data`, JSON stringified, không httpOnly — JS cần đọc để render UI) |
| `mcm_client_id` | `localStorage` | Giữ nguyên localStorage |

### SSE Route Handler

```
GET /api/sync/sse
```

Next.js Route Handler tại `src/app/api/sync/sse/route.ts`:
- Đọc token từ `cookies()` (next/headers)
- Đọc `process.env.INTERNAL_CHANNEL_URL`
- Proxy stream từ `INTERNAL_CHANNEL_URL/api/internal/sync/sse` về client
- Truyền `Last-Event-ID` header và `X-Tenant-Id` header
- Nếu backend trả 401: **gửi SSE event đặc biệt rồi đóng stream** (Option A — client đọc được status code):
  ```
  event: error
  data: {"code":401,"message":"Unauthorized"}
  ```
  Client lắng nghe: `eventSource.addEventListener('error', handler)`, đọc `event.data` để phân biệt 401 vs network drop.

> **K8s requirement**: Ingress của `web-mcm-messenger` phải được tune bằng annotations (`proxy-read-timeout: 3600`, `proxy-buffering: off`) trong HelmRelease values. Không tạo ingress YAML riêng cho SSE. Xem mục [Thay đổi K8s/Ingress](#thay-đổi-k8singress).

### Server Actions đọc token thế nào

Tất cả Server Actions đọc token từ `cookies()` (next/headers) — không nhận token từ client:

```ts
// src/actions/sync.ts
'use server';
import { cookies } from 'next/headers';

export async function syncPullAction(collection: string, lastUpdatedAt?: string) {
  const cookieStore = await cookies();
  const token = cookieStore.get('access_token')?.value;
  const tenantId = cookieStore.get('selected_tenant')?.value;
  // fetch(process.env.INTERNAL_CHANNEL_URL + "/api/internal/sync/pull", ...)
}
```

---

## System Design

### Luồng hoạt động sau refactor

```
Client (Browser)                    Next.js Server (K8s pod)            Backend Services
────────────────────────────────────────────────────────────────────────────────────────
IndexedDB (offline cache)

SyncProvider (React Context, main thread)
  │
  ├── new EventSource('/api/sync/sse')
  │     (relative URL, same-origin)   ── Route Handler: /api/sync/sse/route.ts ──→ INTERNAL_CHANNEL_URL
  │                                       cookies().get('access_token')
  │                                       cookies().get('selected_tenant')
  │
  ├── syncPullAction(collection)       ── Server Action: actions/sync.ts ──────→ INTERNAL_CHANNEL_URL
  │     ↓ write to IndexedDB
  │
  └── syncPushAction(collection, docs) ── Server Action: actions/sync.ts ──────→ INTERNAL_CHANNEL_URL
        ↑ read from IndexedDB queue

Login Page
  └── loginAction(username, password)  ── Server Action: actions/auth.ts ──────→ AUTH_SERVICE_URL
        ↓ cookies().set('access_token', ..., { httpOnly: true, secure: true })

File Upload (từ MessageInput)
  └── POST /api/files/upload           ── Route Handler: proxy ────────────────→ FILESTORE_SERVICE_URL
        (không qua Server Action — tránh bodySizeLimit 1MB)
```

### SyncProvider — Behavioral Contract

`SyncProvider.tsx` phải replicate toàn bộ behavior hiện có của `sync-worker.ts`. Checklist đối chiếu:

| Behavior | sync-worker.ts | SyncProvider.tsx |
|---|---|---|
| SSE stream kết nối + đọc events | `startSseStream()` | `EventSource('/api/sync/sse')` trong `useEffect` |
| SSE reconnect với exponential backoff | `scheduleSseReconnect()`, `sseReconnectAttempt` | Phải implement tương đương với `useRef` |
| Pull fallback mỗi 60 giây | `setInterval(pullFallbackData, 60000)` | `setInterval` trong `useEffect`, clear khi unmount |
| Push queue xử lý mỗi 1 giây | `setInterval(processPendingEvents, 1000)` | `setInterval` trong `useEffect`, clear khi unmount |
| Cleanup events đã done mỗi 30 giây | `setInterval(cleanupEvents, 30000)` | `setInterval` trong `useEffect`, clear khi unmount |
| Initial catchup (pull events since lastSeq) | `initialCatchupAndStream()` | Gọi `syncPullEventsAction()` trước khi mở EventSource |
| SSE 401 → refresh token → restart stream | `waitForFreshToken()` + `UPDATE_TOKEN` message | `EventSource.onerror` → `ensureTokenFresh()` → tạo lại `EventSource` |
| Tenant change → restart toàn bộ loops | `UPDATE_TENANT` message | Cookie change detection → re-run `useEffect` |
| `window.online` → trigger push queue | Không có | `window.addEventListener('online', processPendingEvents)` |
| Concurrent refresh prevention | Không có (token qua postMessage) | `isRefreshing` guard — **bắt buộc** |

Tất cả intervals phải được cleanup trong `useEffect` return function để tránh memory leak khi component unmount.

### Cấu trúc file mới

```
src/
  actions/                            ← MỚI
    auth.ts                           ← loginAction, refreshTokenAction, logoutAction,
    │                                    validateTokenAction, changePasswordAction,
    │                                    getDivisionsAction,
    │                                    setTenantAction
    sync.ts                           ← syncPullAction, syncPushAction, syncPullEventsAction
    file.ts                           ← uploadFileAction

  app/
    api/
      sync/
        sse/
          route.ts                    ← MỚI: Route Handler proxy SSE stream
      files/
        upload/
          route.ts                    ← MỚI: Route Handler upload file (thay SA — tránh bodySizeLimit)

  lib/
    sync/
      SyncProvider.tsx                ← MỚI: thay sync-manager.ts + sync-worker.ts
      local-database.ts               ← GIỮ NGUYÊN
    auth-client.ts                    ← MỚI: helper client-side đọc cookie (selected_tenant, user_data)
    db/                               ← GIỮ NGUYÊN
    hooks/                            ← GIỮ NGUYÊN

─── XÓA ────────────────────────────
  lib/api-client.ts
  lib/auth.ts                         (client-side auth utils)
  lib/sync/sync-manager.ts
  lib/sync/sync-worker.ts
  lib/services/token-refresh.service.ts
  lib/services/activity-tracker.ts
```

> **Lưu ý file upload**: `uploadFileAction()` là Server Action nhận file, nhưng Next.js SA có `bodySizeLimit` mặc định 1MB. Để tránh giới hạn này, upload file đi qua Route Handler `/api/files/upload` (đã có rewrite sẵn trong `next.config.js`), không qua Server Action. `src/actions/file.ts` **không được tạo**.

### Token Refresh Strategy

Với httpOnly cookie, việc refresh token thay đổi như sau:

**Trigger**: Token refresh xảy ra **reactive** (khi nhận 401), không còn proactive mỗi 20 phút như `TokenRefreshService` cũ.

**Race condition prevention**: `SyncProvider` phải có `isRefreshing: boolean` guard để tránh concurrent refresh khi nhiều SA đồng loạt nhận 401:

```ts
let isRefreshing = false;
let refreshPromise: Promise<void> | null = null;

async function ensureTokenFresh() {
  if (isRefreshing) return refreshPromise!;
  isRefreshing = true;
  refreshPromise = refreshTokenAction().finally(() => {
    isRefreshing = false;
    refreshPromise = null;
  });
  return refreshPromise;
}
```

**SSE 401 flow**: Backend trả 401 → Route Handler gửi SSE event `event: error / data: {"code":401}` → `SyncProvider` lắng nghe `addEventListener('error', handler)` đọc `event.data?.code === 401` → gọi `ensureTokenFresh()` → tạo lại `EventSource` sau khi resolve. **Không dùng `onerror`** vì `EventSource.onerror` không trả về HTTP status.

**Khi `refreshTokenAction()` thất bại** (refresh_token cũng hết hạn): gọi `logoutAction()` → xóa cookies → redirect `/login`. **Không được để SSE retry loop chạy vô tận.**

**Cookie config**: tất cả httpOnly cookies set với:
```ts
cookies().set('access_token', token, {
  httpOnly: true,
  secure: process.env.NODE_ENV === 'production', // HTTPS only trên production; localhost HTTP vẫn set được
  sameSite: 'lax',     // CSRF protection
  path: '/',
  maxAge: tokenPayload.exp - Math.floor(Date.now() / 1000), // TTL từ JWT exp claim hoặc response body expires_in
});
```

> **Local dev note**: `secure: process.env.NODE_ENV === 'production'` — trên `localhost` (HTTP), `NODE_ENV=development` nên cookie vẫn được set. Không cần HTTPS local.

---

## Hàm / Endpoint bị ảnh hưởng

### Mới thêm

| File | Hàm | Mô tả |
|---|---|---|
| `src/actions/auth.ts` | `loginAction` | Đăng nhập, set httpOnly cookie |
| `src/actions/auth.ts` | `refreshTokenAction` | Refresh token, update cookie |
| `src/actions/auth.ts` | `logoutAction` | Revoke token, xóa cookie |
| `src/actions/auth.ts` | `validateTokenAction` | Validate token từ cookie |
| `src/actions/auth.ts` | `changePasswordAction` | Đổi mật khẩu |
| `src/actions/auth.ts` | `getDivisionsAction` | Lấy danh sách Khối |
| `src/actions/auth.ts` | `setTenantAction` | Set `selected_tenant` cookie (gọi khi user chọn Khối) |
| `src/actions/sync.ts` | `syncPullAction` | Pull collection từ connector-internal |
| `src/actions/sync.ts` | `syncPushAction` | Push docs lên connector-internal |
| `src/actions/sync.ts` | `syncPullEventsAction` | Pull events since seq |
| `src/app/api/files/upload/route.ts` | `POST` handler | Proxy upload file lên filestore (thay SA — tránh bodySizeLimit) |
| `src/app/api/sync/sse/route.ts` | `GET` handler | SSE proxy |
| `src/lib/sync/SyncProvider.tsx` | `SyncProvider` | React context thay Web Worker |

### Bị xóa

| File | Lý do |
|---|---|
| `src/lib/api-client.ts` | Không còn client-side fetch |
| `src/lib/auth.ts` | Thay bằng `src/actions/auth.ts` (server) + `src/lib/auth-client.ts` (client helper đọc cookie) |
| `src/lib/sync/sync-manager.ts` | Thay bằng `SyncProvider.tsx` |
| `src/lib/sync/sync-worker.ts` | Thay bằng main thread `SyncProvider.tsx` |
| `src/lib/services/token-refresh.service.ts` | Token refresh qua SA |
| `src/lib/services/activity-tracker.ts` | Không còn cần thiết |

> **Không xóa `sync-worker.ts` cho đến khi `SyncProvider` đã pass toàn bộ behavioral contract test trên staging.**

### Bị sửa

| File | Thay đổi |
|---|---|
| `Dockerfile` | Xóa 3 dòng `ENV NEXT_PUBLIC_*` |
| `next.config.js` | Giữ nguyên rewrites (vẫn cần cho Route Handler file upload proxy) |
| `src/app/layout.tsx` | Thêm `<SyncProvider>`, wrap `<DbProvider>` (hiện `DbProvider` init `SyncManager` — cần refactor) |
| `src/app/login/page.tsx` | Gọi `loginAction` thay `login()` |
| `src/app/profile/page.tsx` | Gọi `changePasswordAction` |
| `src/app/sites/page.tsx` | Gọi `getDivisionsAction` |
| `src/lib/site-manager.ts` | `switchToDivision()` gọi `setTenantAction(tenantId)` để set cookie, sau đó gọi callback từ `SyncProvider` để restart loops |
| `src/lib/db/db-context.tsx` | Xóa import `SyncManager`, `isAuthenticated` — `SyncProvider` đảm nhận khởi tạo sync |
| `src/components/Conversations/ChatWindow/MessageInput.tsx` | Xóa `getAccessToken()` call (line 85), file upload qua Route Handler, `syncPushAction` |
| `src/components/UserMenu.tsx` | `getUserData()` → đọc từ `user_data` cookie thay localStorage |
| `src/app/[site]/conversations/[id]/page.tsx` | Gọi `syncPushAction` thay `sync.pushOutbound()` |
| `src/app/conversations/page.tsx` + `layout.tsx` | `isAuthenticated()` → đọc cookie `access_token` existence thay localStorage |
| `src/lib/hooks/useConversations.ts` | `getSelectedTenant()` → đọc cookie thay localStorage |
| `src/components/Conversations/Sidebar.tsx` | `getSelectedTenant()` → đọc cookie |
| `src/components/Conversations/SearchDialog.tsx` | `getSelectedTenant()` → đọc cookie |

---

## Đầu vào mong đợi

### `loginAction(username, password)`

```ts
// Input
{ username: string; password: string }

// Output
{ success: true; user: UserData } | { success: false; error: string }

// Side effect
// Set httpOnly cookies: access_token, refresh_token
// Set regular cookie: user (JSON), nếu cần
```

### `syncPushAction(collection, docs)`

```ts
// Input
{ collection: 'messages' | 'conversations' | 'users' | 'customers'; docs: any[] }

// Output
{ success: true } | { success: false; error: string; status?: number }

// Headers gửi lên backend (server reads từ cookie)
// Authorization: Bearer <access_token>
// X-Tenant-Id: <selected_tenant>
```

### `GET /api/sync/sse` (Route Handler)

```
Request headers (từ browser):
  Cookie: access_token=...; selected_tenant=BEPDI

Response:
  Content-Type: text/event-stream
  Transfer-Encoding: chunked

  data: {"collection":"messages","seq":1234,...}

  data: {"collection":"conversations","seq":1235,...}
```

---

## Đầu ra mong đợi

1. Sau login thành công: `access_token` và `refresh_token` được lưu trong httpOnly cookie — không accessible qua `document.cookie` hay `localStorage`.
2. SSE stream kết nối qua `/api/sync/sse` — client không biết backend URL.
3. Mọi push/pull đi qua Server Actions — không có `fetch()` nào từ browser trỏ đến backend trực tiếp.
4. Dockerfile không còn `NEXT_PUBLIC_*` — image build một lần dùng mọi environment.

---

## Tiêu chí chấp nhận

### Auth & Session
- [ ] Đăng nhập thành công → `access_token` trong httpOnly cookie: `document.cookie` không chứa `access_token`.
- [ ] `selected_tenant` trong regular cookie: `document.cookie` chứa `selected_tenant` (JS đọc được).
- [ ] `user_data` trong regular cookie: `getUserData()` đọc từ cookie, không từ localStorage.
- [ ] Sau refresh trang, session vẫn còn và đúng tenant.
- [ ] `logoutAction()`: tất cả cookies bị clear, redirect về `/login`.
- [ ] `refreshTokenAction()` thất bại → tự động logout → redirect `/login` (không retry vô tận).

### Token Refresh
- [ ] Access token hết hạn → SA/Route Handler nhận 401 → `refreshTokenAction()` được gọi **đúng 1 lần** dù có nhiều SA đang chạy concurrent.
- [ ] Sau refresh thành công: SA/EventSource retry tự động không cần user action.
- [ ] SSE nhận 401 → `SyncProvider` gọi `ensureTokenFresh()` → EventSource restart → stream tiếp tục nhận events.

### Realtime Sync
- [ ] SSE stream kết nối qua `/api/sync/sse` (relative URL — kiểm tra DevTools Network).
- [ ] Tin nhắn mới từ Zalo xuất hiện trong UI không cần refresh trang.
- [ ] SSE disconnect → reconnect tự động với exponential backoff (không cần user action).
- [ ] Chuyển tenant: SSE stream restart, IndexedDB query đúng tenant mới.

### Offline-first
- [ ] Gửi tin nhắn khi offline → message vào IndexedDB ngay → queue event `pending`.
- [ ] Khi online lại → `window.online` event → push queue tự động xử lý.
- [ ] IndexedDB vẫn hiển thị đúng dữ liệu khi offline (UI không blank).

### File Upload
- [ ] Upload ảnh/file qua `POST /api/files/upload` (Route Handler, không phải Server Action).
- [ ] File >1MB upload thành công.

### Triển khai
- [ ] Dockerfile không còn dòng `ENV NEXT_PUBLIC_*` nào.
- [ ] DevTools → Network: không có request nào từ browser trỏ thẳng đến `AUTH_SERVICE_URL` hay `INTERNAL_CHANNEL_URL`.
- [ ] Thay đổi `INTERNAL_CHANNEL_URL` trong K8s env vars → không cần rebuild → app hoạt động đúng.
- [ ] Session migration: user đang login với token localStorage → sau deploy → bị redirect to login (expected breaking change, có thông báo).

### Regression
- [ ] `e2e/tenant-division.spec.ts` pass sau khi update spec đọc cookie thay localStorage.
- [ ] Pull fallback (60s), push queue (1s), cleanup (30s) intervals hoạt động đúng trong `SyncProvider`.

---

## Phương án triển khai

### Phase 1 — Auth Actions + httpOnly Cookies (0.5 ngày)

1. Tạo `src/actions/auth.ts` với đủ 7 actions (bao gồm `setTenantAction`)
2. Migrate `LoginPage` → dùng `loginAction`
3. **Giữ nguyên localStorage write song song** cho `access_token` và `refresh_token` cho đến khi Phase 2 merge — `sync-worker.ts` vẫn đọc localStorage trong giai đoạn này

> **Phase 1 + Phase 2 phải release cùng lúc** trong một deployment (v1.2.0). Không release Phase 1 riêng lẻ để tránh trạng thái broken: cookie có token nhưng sync-worker vẫn đọc localStorage rỗng.

### Phase 2 — SSE Proxy + Sync Actions (2 ngày)

1. Tạo `src/app/api/sync/sse/route.ts`
2. Tạo `src/actions/sync.ts`
3. Tạo `src/lib/sync/SyncProvider.tsx` (main thread, EventSource + SA)
4. Mount `SyncProvider` vào `layout.tsx`

### Phase 3 — Migrate Components + Cleanup (0.5 ngày)

1. Migrate `MessageInput`, `ConversationDetailPage`, `ProfilePage`, `SitesPage`, `UserMenu`, `Sidebar`, `SearchDialog`, `db-context.tsx`, `site-manager.ts`, `useConversations.ts`
2. Tạo `src/lib/auth-client.ts` (helper đọc cookie client-side: `getSelectedTenant`, `getUserData`, `isAuthenticated`)
3. Xóa `api-client.ts`, `auth.ts`, `sync-manager.ts`, `sync-worker.ts`, `token-refresh.service.ts`, `activity-tracker.ts`
4. Xóa `NEXT_PUBLIC_*` khỏi Dockerfile
5. Update `e2e/tenant-division.spec.ts` đọc cookie thay localStorage
6. Release v1.2.0

### Phase 4 — K8s/Ingress (DevOps, song song với Phase 2)

Xem mục [Thay đổi K8s/Ingress](#thay-đổi-k8singress) — cần deploy trước khi release v1.2.0.

---

## Thay đổi K8s/Ingress

### Tune ingress annotations trực tiếp trong HelmRelease values (bắt buộc)

Chart `backend-api` đã có template ingress và hỗ trợ `values.ingress.annotations`, nên chỉ cần chỉnh trong các file hiện có:

- `k8s-dpsrv/web/web-mcm-messenger.yaml`
- `k8s-dpsrv-prd/web/web-mcm-messenger.yaml`

```yaml
ingress:
  annotations:
    kubernetes.io/ingress.class: "nginx"
    nginx.ingress.kubernetes.io/use-regex: "true"
    nginx.ingress.kubernetes.io/proxy-read-timeout: "3600"
    nginx.ingress.kubernetes.io/proxy-send-timeout: "3600"
    nginx.ingress.kubernetes.io/proxy-buffering: "off"
    nginx.ingress.kubernetes.io/proxy-http-version: "1.1"
```

> Mục tiêu là đảm bảo request `/api/sync/sse` không bị cắt ở timeout mặc định của nginx ingress, nhưng không tăng thêm tài nguyên YAML riêng để vận hành.

### SSL redirect (khuyến nghị)

Thêm vào annotations của main ingress `web-mcm-messenger.yaml`:
```yaml
nginx.ingress.kubernetes.io/ssl-redirect: "true"
```

### Không cần thay đổi env vars K8s

Ba biến `AUTH_SERVICE_URL`, `INTERNAL_CHANNEL_URL`, `FILESTORE_SERVICE_URL` trong manifest hiện tại đã đúng. Không cần thêm hay xóa gì.

---

## Breaking Changes

| Breaking Change | Hành vi | Ghi chú |
|---|---|---|
| Session migration | User đang login với token trong `localStorage` → sau deploy v1.2.0 → bị redirect về `/login` | Expected, không cần migration script |
| Cookie SameSite | Cookie `access_token` có `SameSite=Lax` — không gửi qua cross-site request | Không ảnh hưởng — app chỉ dùng same-origin |
| `document.cookie` không còn `access_token` | Code nào đang read `access_token` từ JS sẽ nhận `null` | Phải migrate hết trước Phase 3 |



### Phương án Local Testing

**Chuẩn bị:**
```bash
# Terminal 1 — Auth service
cd api-auth && npm run start:dev          # port 3001

# Terminal 2 — Connector Internal
cd api-mcm-connector-internal && npm run start:dev  # port 3002

# Terminal 3 — Filestore
cd api-filestore && npm run start:dev     # port 3003

# Terminal 4 — Web App
# Đặt trong .env:
# AUTH_SERVICE_URL=http://localhost:3001
# INTERNAL_CHANNEL_URL=http://localhost:3002
# FILESTORE_SERVICE_URL=http://localhost:3003
cd web-mcm-messenger && npm run dev
```

**Checklist xác nhận:**
1. DevTools → Application → Cookies: `access_token` có flag `HttpOnly`, `selected_tenant` **không** có flag `HttpOnly`.
2. DevTools → Network: không có request nào đến `localhost:3001`, `3002`, `3003` từ browser — chỉ có requests đến `localhost:3000`.
3. DevTools → Network → filter `sse`: kết nối đến `/api/sync/sse`, tab EventStream có events.
4. Simulate offline: DevTools → Network → Offline → gửi tin → Online lại → tin được sync.
5. Simulate token expiry: xóa `access_token` cookie thủ công → thực hiện action → verify redirect to login hoặc auto-refresh.
6. E2E test (sau khi update spec):
   ```bash
   npx ts-node --compiler-options '{"module":"commonjs",...}' e2e/tenant-division.spec.ts
   ```

**Docker test (verify không còn `NEXT_PUBLIC_*`):**
```bash
docker build -t web-mcm-test .
docker run -e AUTH_SERVICE_URL=http://host.docker.internal:3001 \
           -e INTERNAL_CHANNEL_URL=http://host.docker.internal:3002 \
           -e FILESTORE_SERVICE_URL=http://host.docker.internal:3003 \
           -p 3000:3000 web-mcm-test
# Verify: strings dist/... | grep NEXT_PUBLIC — không có kết quả
```
