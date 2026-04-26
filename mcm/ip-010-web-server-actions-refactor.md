# IP-010: Implementation Plan — Server Actions + Route Handler SSE Proxy

**Use Case:** UC-010  
**Service:** `web-mcm-messenger`  
**Ngày tạo:** 2026-04-26  
**Estimate tổng:** 3 ngày dev  
**Release target:** v1.2.0  
**Test Plan:** [tp-010-web-server-actions-refactor.md](tp-010-web-server-actions-refactor.md)  
**Deployment Plan:** [dp-010-web-server-actions-refactor.md](dp-010-web-server-actions-refactor.md)

---

## Tổng quan

| Phase | Task | Estimate | Gate trước khi next |
|---|---|---|---|
| Phase 1 | Auth Actions + Cookie | 0.5 ngày | QA test Nhóm 1 (TC-AUTH-*) |
| Phase 2 | SSE Proxy + SyncProvider | 2 ngày | QA test Nhóm 2-5 |
| Phase 3 | Migrate Components + Cleanup | 0.5 ngày | QA test Nhóm 6 (regression) |
| Phase 4 | K8s/Ingress (DevOps) | Song song Phase 2 | DevOps sign-off trước production |

> **Phase 1 + 2 + 3 release cùng lúc trong v1.2.0** — không deploy Phase 1 riêng lẻ.  
> **QA test local** (localhost:3000-3003) — không cần chờ staging cho primary testing.

---

## Phase 1 — Auth Actions + httpOnly Cookies (0.5 ngày)

### 1.1 Tạo `src/actions/auth.ts`

Tạo mới file với 7 Server Actions:

```ts
'use server';
import { cookies } from 'next/headers';

// Helper set httpOnly cookie
function setAuthCookie(name: string, value: string, maxAge: number) {
  cookies().set(name, value, {
    httpOnly: name === 'access_token' || name === 'refresh_token',
    secure: process.env.NODE_ENV === 'production',
    sameSite: 'lax',
    path: '/',
    maxAge,
  });
}
```

**7 actions cần implement:**

| Action | Method backend | Cookie side effects |
|---|---|---|
| `loginAction(username, password)` | POST `/api/auth/login` | Set `access_token` (httpOnly), `refresh_token` (httpOnly), `user_data` (regular, JSON), `selected_tenant` (regular, nếu 1 division) |
| `refreshTokenAction()` | POST `/api/auth/refresh_token` | Update `access_token`, `refresh_token` |
| `logoutAction()` | POST `/api/auth/revoke` | Delete tất cả auth cookies |
| `validateTokenAction()` | GET `/api/auth/validate` | Không set cookie |
| `changePasswordAction(oldPw, newPw)` | POST `/api/auth/change-password` | Không set cookie |
| `getDivisionsAction()` | GET `/api/auth/divisions` | Không set cookie — trả về array |
| `setTenantAction(tenantId)` | Không gọi backend | Set `selected_tenant` (regular cookie, không httpOnly) |

**Cookie `maxAge`**: đọc từ JWT payload `exp` claim:
```ts
import { jwtDecode } from 'jwt-decode'; // hoặc parse thủ công
const payload = JSON.parse(Buffer.from(token.split('.')[1], 'base64').toString());
const maxAge = payload.exp - Math.floor(Date.now() / 1000);
```

### 1.2 Migrate `src/app/login/page.tsx`

**Hiện tại**: `login()` → `saveAuthData(tokens, user)` → `localStorage.setItem(...)`  
**Sau**: `loginAction(email, password)` — cookie set server-side

```ts
// Thay thế
const { tokens, user } = await login(email, password);
saveAuthData(tokens, user);

// Bằng
const result = await loginAction(email, password);
if (!result.success) throw new Error(result.error);
```

### 1.3 Migrate `src/app/login/select-site/page.tsx`

**Hiện tại**: `isAuthenticated()` đọc localStorage  
**Sau**: Đọc cookie `access_token` existence (dùng `auth-client.ts` — tạo ở Phase 3)  
**Phase 1 workaround**: giữ nguyên tạm — file này sẽ migrate trong Phase 3

### 1.4 Giữ nguyên localStorage write (tạm thời)

`sync-worker.ts` vẫn đang đọc `localStorage` (`accessToken`, `tenantId`). Trong Phase 1, `loginAction` set cookie, nhưng **không xóa** localStorage write cũ. `lib/auth.ts` vẫn tồn tại nguyên trạng.

> **QA note**: Trong Phase 1, `localStorage` vẫn chứa `access_token` — đây là intentional parallel write. TC-AUTH-001 assertion "localStorage không chứa access_token" chỉ verify sau Phase 3.

### Phase 1 Delivery Checklist

- [ ] `src/actions/auth.ts` với 7 actions — TypeScript strict no errors
- [ ] `src/app/login/page.tsx` gọi `loginAction`
- [ ] Login thành công → cookies set trong DevTools (Application → Cookies)
- [ ] `access_token` có flag `HttpOnly`, `selected_tenant` không có
- [ ] `logoutAction()` clear cookies
- [ ] `npm run build` thành công
- [ ] **Thông báo QA**: "Phase 1 ready — test Nhóm 1. `localStorage` write song song là intentional, skip assertion đó cho đến Phase 3."

---

## Phase 2 — SSE Route Handler + SyncProvider (2 ngày)

### 2.1 Tạo `src/app/api/sync/sse/route.ts`

```ts
import { NextRequest } from 'next/server';
import { cookies } from 'next/headers';

export async function GET(req: NextRequest) {
  const cookieStore = await cookies();
  const token = cookieStore.get('access_token')?.value;
  const tenantId = cookieStore.get('selected_tenant')?.value;

  if (!token || !tenantId) {
    return new Response('Unauthorized', { status: 401 });
  }

  const lastEventId = req.headers.get('Last-Event-ID') || '0';
  const upstreamUrl = `${process.env.INTERNAL_CHANNEL_URL}/api/internal/sync/sse?since=${lastEventId}`;

  const upstreamRes = await fetch(upstreamUrl, {
    headers: {
      Authorization: `Bearer ${token}`,
      'X-Tenant-Id': tenantId,
      'Accept': 'text/event-stream',
      'Cache-Control': 'no-cache',
    },
  });

  if (!upstreamRes.ok) {
    if (upstreamRes.status === 401 || upstreamRes.status === 403) {
      // Gửi SSE error event rồi đóng (Option A — client phân biệt được 401 vs network)
      const errorEvent = `event: error\ndata: ${JSON.stringify({ code: 401, message: 'Unauthorized' })}\n\n`;
      return new Response(errorEvent, {
        headers: { 'Content-Type': 'text/event-stream' },
      });
    }
    return new Response('Bad Gateway', { status: 502 });
  }

  return new Response(upstreamRes.body, {
    headers: {
      'Content-Type': 'text/event-stream',
      'Cache-Control': 'no-cache',
      'Connection': 'keep-alive',
      'X-Accel-Buffering': 'no', // tắt Nginx buffering
    },
  });
}
```

### 2.2 Tạo `src/actions/sync.ts`

3 actions:

```ts
'use server';
import { cookies } from 'next/headers';

async function getSyncHeaders() {
  const cookieStore = await cookies();
  return {
    Authorization: `Bearer ${cookieStore.get('access_token')?.value}`,
    'X-Tenant-Id': cookieStore.get('selected_tenant')?.value ?? '',
    'Content-Type': 'application/json',
  };
}

export async function syncPullAction(collection: string, lastUpdatedAt?: string) {
  const headers = await getSyncHeaders();
  const params = new URLSearchParams({ collection });
  if (lastUpdatedAt) params.set('lastUpdatedAt', lastUpdatedAt);
  const res = await fetch(`${process.env.INTERNAL_CHANNEL_URL}/api/internal/sync/pull?${params}`, { headers });
  if (!res.ok) return { success: false, error: `HTTP ${res.status}`, status: res.status };
  return { success: true, data: await res.json() };
}

export async function syncPushAction(collection: string, docs: any[]) {
  const headers = await getSyncHeaders();
  const res = await fetch(`${process.env.INTERNAL_CHANNEL_URL}/api/internal/sync/push`, {
    method: 'POST',
    headers,
    body: JSON.stringify({ collection, docs }),
  });
  if (!res.ok) return { success: false, error: `HTTP ${res.status}`, status: res.status };
  return { success: true };
}

export async function syncPullEventsAction(since: number) {
  const headers = await getSyncHeaders();
  const res = await fetch(`${process.env.INTERNAL_CHANNEL_URL}/api/internal/sync/pull/events?since=${since}`, { headers });
  if (!res.ok) return { success: false, error: `HTTP ${res.status}`, status: res.status };
  return { success: true, data: await res.json() };
}
```

### 2.3 Tạo `src/lib/sync/SyncProvider.tsx`

**Quan trọng**: Đặt `isRefreshing` và `refreshPromise` ở **module scope** (ngoài component) để không reset khi re-render:

```ts
// Module-scope — không đặt trong component
let isRefreshing = false;
let refreshPromise: Promise<void> | null = null;

async function ensureTokenFresh(): Promise<void> {
  if (isRefreshing) return refreshPromise!;
  isRefreshing = true;
  refreshPromise = refreshTokenAction().finally(() => {
    isRefreshing = false;
    refreshPromise = null;
  });
  return refreshPromise;
}
```

**Structure SyncProvider:**

```ts
'use client';
export function SyncProvider({ children }: { children: React.ReactNode }) {
  const db = useLiveQuery(...); // from db-context or direct
  const eventSourceRef = useRef<EventSource | null>(null);
  const reconnectAttemptRef = useRef(0);
  const reconnectTimerRef = useRef<NodeJS.Timeout | null>(null);

  // --- SSE + 3 intervals trong 1 useEffect ---
  useEffect(() => {
    let pullInterval: NodeJS.Timeout;
    let pushInterval: NodeJS.Timeout;
    let cleanupInterval: NodeJS.Timeout;

    async function startSync() {
      // 1. Initial catchup
      const lastSeq = await getLastSeq();
      const catchupResult = await syncPullEventsAction(lastSeq);
      if (catchupResult.success) await enqueueServerEvents(catchupResult.data);

      // 2. Open EventSource
      openEventSource();

      // 3. Intervals
      pullInterval = setInterval(pullFallback, 60_000);
      pushInterval = setInterval(processPendingEvents, 1_000);
      cleanupInterval = setInterval(cleanupDoneEvents, 30_000);
    }

    function openEventSource() {
      eventSourceRef.current?.close();
      const es = new EventSource('/api/sync/sse');
      eventSourceRef.current = es;

      es.onmessage = handleSseMessage;

      // SSE 401 — qua event name 'error' với data.code
      es.addEventListener('error', async (event: MessageEvent) => {
        try {
          const data = JSON.parse(event.data);
          if (data.code === 401) {
            es.close();
            try {
              await ensureTokenFresh();
              reconnectAttemptRef.current = 0;
              openEventSource();
            } catch {
              await logoutAction();
              window.location.href = '/login';
            }
            return;
          }
        } catch {
          // không phải structured error → network drop → reconnect backoff
        }
        scheduleReconnect();
      });

      es.onerror = () => scheduleReconnect();
    }

    function scheduleReconnect() {
      if (reconnectTimerRef.current) return;
      const delay = Math.min(1000 * Math.pow(2, reconnectAttemptRef.current), 30_000);
      reconnectAttemptRef.current += 1;
      reconnectTimerRef.current = setTimeout(() => {
        reconnectTimerRef.current = null;
        openEventSource();
      }, delay);
    }

    // window.online → trigger push
    window.addEventListener('online', processPendingEvents);

    startSync();

    return () => {
      eventSourceRef.current?.close();
      clearInterval(pullInterval);
      clearInterval(pushInterval);
      clearInterval(cleanupInterval);
      if (reconnectTimerRef.current) clearTimeout(reconnectTimerRef.current);
      window.removeEventListener('online', processPendingEvents);
    };
  }, [tenantId]); // re-run khi tenant thay đổi
```

> **Tenant change detection**: `tenantId` đọc từ cookie `selected_tenant` bằng `auth-client.ts` (tạo ở Phase 3). Trong Phase 2, có thể dùng `document.cookie` parse trực tiếp tạm thời.

### 2.4 Mount `SyncProvider` vào `src/app/layout.tsx`

```ts
import { SyncProvider } from '../lib/sync/SyncProvider';
// ...
<DbProvider>
  <SyncProvider>
    {children}
  </SyncProvider>
</DbProvider>
```

### 2.5 Update `src/app/api/files/upload/route.ts`

Tạo Route Handler proxy file upload để bypass bodySizeLimit:

```ts
import { NextRequest } from 'next/server';
import { cookies } from 'next/headers';

export async function POST(req: NextRequest) {
  const cookieStore = await cookies();
  const token = cookieStore.get('access_token')?.value;
  const tenantId = cookieStore.get('selected_tenant')?.value;
  const body = await req.arrayBuffer();

  const res = await fetch(`${process.env.FILESTORE_SERVICE_URL}/api/files/upload`, {
    method: 'POST',
    headers: {
      Authorization: `Bearer ${token}`,
      'X-Tenant-Id': tenantId ?? '',
      'Content-Type': req.headers.get('Content-Type') ?? 'application/octet-stream',
    },
    body,
  });

  const data = await res.json();
  return Response.json(data, { status: res.status });
}
```

> **Note**: `next.config.js` đã có rewrite `/api/files/*` → `FILESTORE_SERVICE_URL`. Route Handler này *override* rewrite cho `/api/files/upload` cụ thể — Next.js ưu tiên Route Handler hơn rewrite.

### Phase 2 Delivery Checklist

- [ ] `src/app/api/sync/sse/route.ts` — SSE proxy hoạt động, header `X-Accel-Buffering: no`
- [ ] `src/actions/sync.ts` — 3 actions compile và gọi được
- [ ] `src/lib/sync/SyncProvider.tsx` — 10 behaviors từ behavioral contract
- [ ] `SyncProvider` mounted trong `layout.tsx`
- [ ] `isRefreshing` guard ở module scope (không trong component)
- [ ] SSE 401 dùng `addEventListener('error')` với data parse, không dùng `onerror` cho 401
- [ ] Logout fallback khi `refreshTokenAction()` thất bại
- [ ] `window.online` listener cleanup khi unmount
- [ ] `src/app/api/files/upload/route.ts` proxy hoạt động
- [ ] `npm run build` thành công
- [ ] **QA note**: "Phase 2 ready — QA test Nhóm 2-5 trên local. Staging không cần."  
  **Dev confirm**: "Delete cookie `access_token` trong DevTools = intended test method cho TC-REFRESH-001."

---

## Phase 3 — Migrate Components + Cleanup (0.5 ngày)

### 3.1 Tạo `src/lib/auth-client.ts`

Helper client-side đọc cookie (không httpOnly — JS đọc được):

```ts
// Chỉ đọc regular cookies (selected_tenant, user_data)
// access_token KHÔNG đọc được từ JS — đúng như thiết kế

export function getSelectedTenant(): string | null {
  if (typeof document === 'undefined') return null;
  const match = document.cookie.match(/(?:^|;\s*)selected_tenant=([^;]+)/);
  return match ? decodeURIComponent(match[1]) : null;
}

export function getUserData(): UserData | null {
  if (typeof document === 'undefined') return null;
  const match = document.cookie.match(/(?:^|;\s*)user_data=([^;]+)/);
  if (!match) return null;
  try { return JSON.parse(decodeURIComponent(match[1])); }
  catch { return null; }
}

export function isAuthenticated(): boolean {
  // Không thể check access_token (httpOnly) từ client
  // Dùng selected_tenant presence làm proxy — nếu user đã login và chọn tenant
  if (typeof document === 'undefined') return false;
  return document.cookie.includes('selected_tenant=');
}
```

### 3.2 Migrate 14 files

Thứ tự thực hiện (bottom-up, ít dependency trước):

**Bước 1 — Utility layer (không depend vào component khác):**

| File | Thay đổi |
|---|---|
| `src/lib/hooks/useConversations.ts` | `getSelectedTenant()` → `getSelectedTenant()` từ `auth-client.ts` |
| `src/lib/site-manager.ts` | `switchToDivision()`: gọi `setTenantAction(tenantId)` + callback SyncProvider |

**Bước 2 — DB/Context layer:**

| File | Thay đổi |
|---|---|
| `src/lib/db/db-context.tsx` | Xóa `SyncManager` import và init, xóa `isAuthenticated` từ `auth.ts` |

**Bước 3 — Component layer:**

| File | Thay đổi |
|---|---|
| `src/components/UserMenu.tsx` | `getUserData()` → từ `auth-client.ts` |
| `src/components/Conversations/Sidebar.tsx` | `getSelectedTenant()` → từ `auth-client.ts` |
| `src/components/Conversations/SearchDialog.tsx` | `getSelectedTenant()` → từ `auth-client.ts` |
| `src/components/Conversations/ChatWindow/MessageInput.tsx` | Xóa `getAccessToken()` line 85; file upload qua `/api/files/upload` (no auth header needed — Route Handler add từ cookie) |

**Bước 4 — Page layer:**

| File | Thay đổi |
|---|---|
| `src/app/login/select-site/page.tsx` | `isAuthenticated()` → từ `auth-client.ts` |
| `src/app/sites/page.tsx` | `getUserData()`, `isAuthenticated()`, `getDivisionsAction()`, `setTenantAction()` |
| `src/app/profile/page.tsx` | `changePasswordAction()` |
| `src/app/conversations/page.tsx` + `layout.tsx` | `isAuthenticated()` → từ `auth-client.ts` |
| `src/app/[site]/conversations/[id]/page.tsx` | `syncPushAction()` |

### 3.3 Xóa files cũ

```bash
rm src/lib/api-client.ts
rm src/lib/auth.ts
rm src/lib/sync/sync-manager.ts
# sync-worker.ts: giữ nguyên cho đến khi staging pass (rename thành .bak nếu muốn)
rm src/lib/services/token-refresh.service.ts
rm src/lib/services/activity-tracker.ts
```

> **Không xóa `sync-worker.ts`** cho đến khi QA confirm SyncProvider pass trên staging.

### 3.4 Xóa `NEXT_PUBLIC_*` khỏi Dockerfile

Xóa 3 dòng:
```dockerfile
# XÓA các dòng này:
ENV NEXT_PUBLIC_AUTH_SERVICE_URL=...
ENV NEXT_PUBLIC_INTERNAL_CHANNEL_URL=...
ENV NEXT_PUBLIC_FILESTORE_SERVICE_URL=...
```

### 3.5 Update `e2e/tenant-division.spec.ts`

Migrate assertions đọc cookie thay localStorage:

```ts
// Thay
const tenant = await page.evaluate(() => localStorage.getItem('selected_tenant'));

// Bằng
const cookies = await context.cookies();
const tenant = cookies.find(c => c.name === 'selected_tenant')?.value;
```

### Phase 3 Delivery Checklist

- [ ] `src/lib/auth-client.ts` tạo với 3 helpers
- [ ] 14 files migrated, không còn import từ `lib/auth.ts` hay `lib/api-client.ts`
- [ ] `npm run build` thành công — zero TypeScript errors
- [ ] `NEXT_PUBLIC_*` xóa khỏi Dockerfile
- [ ] `e2e/tenant-division.spec.ts` đọc cookie (không localStorage)
- [ ] `npx ts-node ... e2e/tenant-division.spec.ts` pass
- [ ] **Gate cho QA**: "Phase 3 ready — spec updated. Commit: [hash]. QA chạy TC-REG-001."
- [ ] **Docker test**:
  ```bash
  docker build -t web-mcm-test .
  strings dist/static/**/*.js | grep NEXT_PUBLIC  # phải không có kết quả
  ```

---

## Local Testing — Developer Checklist

```bash
# Terminal 1
cd api-auth && npm run start:dev           # port 3001

# Terminal 2
cd api-mcm-connector-internal && npm run start:dev  # port 3002

# Terminal 3
cd api-filestore && npm run start:dev      # port 3003

# Terminal 4 — .env
# AUTH_SERVICE_URL=http://localhost:3001
# INTERNAL_CHANNEL_URL=http://localhost:3002
# FILESTORE_SERVICE_URL=http://localhost:3003
cd web-mcm-messenger && npm run dev        # port 3000
```

**Verify sau mỗi Phase:**

| Check | Phase 1 | Phase 2 | Phase 3 |
|---|---|---|---|
| `access_token` có flag HttpOnly | ✓ | ✓ | ✓ |
| Network tab: không request đến :3001/:3002/:3003 từ browser | — | ✓ | ✓ |
| SSE kết nối `/api/sync/sse` (không phải absolute URL) | — | ✓ | ✓ |
| Offline → gửi tin → online → sync | — | ✓ | ✓ |
| `document.cookie` không chứa `access_token` | — | — | ✓ |
| Dockerfile không có `NEXT_PUBLIC_*` | — | — | ✓ |
| e2e spec pass | — | — | ✓ |

---

## Dependencies

```
Phase 1 ──→ Phase 2 ──→ Phase 3
                │
                └──→ QA test Nhóm 2-5 (local, không cần DevOps)

Phase 4 (DevOps) ──────────────────────→ Staging SSE Ingress
                                          Production SSE Ingress
                                                │
                                                └──→ Production deploy gate
```

**Hard constraints:**
1. Phase 1+2+3 merge vào `main` cùng lúc (1 PR hoặc 3 PRs merge liên tiếp trong cùng 1 deployment window)
2. K8s SSE Ingress staging phải lên trước QA staging verify (không block local QA)
3. K8s SSE Ingress production phải lên trước v1.2.0 production deploy

---

## Ghi chú kỹ thuật

### Tại sao không dùng `EventSource.onerror` cho 401?
`EventSource.onerror` không expose HTTP status code. Mọi lỗi (401, 502, network drop) đều vào cùng handler. Giải pháp: Route Handler gửi SSE event `event: error` với JSON data trước khi đóng stream — client parse `data.code === 401` để phân biệt.

### Tại sao `isRefreshing` ở module scope?
Nếu đặt trong component với `useRef`, khi React strict mode re-render component 2 lần trong dev, `useRef` reset → `isRefreshing` sẽ bị `false` giữa chừng → race condition. Module scope là singleton thực sự cho cả session.

### Tại sao file upload qua Route Handler, không phải Server Action?
Next.js Server Actions có `bodySizeLimit` default 1MB (`next.config.js` → `experimental.serverActions.bodySizeLimit`). Route Handler không có giới hạn này. Upload route đã có rewrite sẵn trong `next.config.js` — Route Handler tự động override rewrite.

### Tenant change detection trong SyncProvider
`useEffect([tenantId])` — `tenantId` đọc từ `getSelectedTenant()` (auth-client.ts). Khi `setTenantAction()` set cookie mới và component re-render (e.g., router.push sau switch division), `tenantId` thay đổi → useEffect re-run → EventSource restart + intervals reset.
