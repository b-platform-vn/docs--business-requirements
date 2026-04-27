# Bug-006: Chuyển Division xóa local DB nhưng không re-sync (site-switch clears local data, no re-sync)

**Service:** `web-mcm-messenger` (client), `SyncProvider` client-side logic, `api-mcm-connector-internal` (server sync endpoints)
**Báo cáo:** 2026-04-27
**Mức độ:** High
**Kiểm tra trên:** Local dev / Staging
**Nguồn:** QA E2E reproduction (`e2e/site-switch-sync.spec.ts`), runtime observation in browser client

---


## Tóm tắt

Ban đầu, khi chuyển Division (tenant) `switchToDivision()` gọi `clearLocalData()` để xóa IndexedDB (`mcm_local_db_dexie`), dẫn đến trạng thái local rỗng và Sync flow không re-sync tự động. QA đã cung cấp E2E reproduction `e2e/site-switch-sync.spec.ts` để chứng minh hành vi.

Hiện tại (tạm thời) hành vi đã được thay đổi: thao tác tự động xóa local data khi chuyển Division đã bị loại bỏ. Thay vào đó sẽ giữ nguyên dữ liệu local sau khi chuyển tenant. Kế hoạch dài hạn: thêm tùy chọn cho người dùng trong Profile Menu để chủ động xóa local data khi cần.

---

## Cách tái hiện

1. Đăng nhập vào Web MCM Messenger.
2. Chuyển Division (ví dụ MDF → ODL) qua UI (gọi `switchToDivision()`).
3. Quan sát IndexedDB `mcm_local_db_dexie` — các bảng `conversations`, `events`, `messages` được clear.
4. Chờ vài giây — SyncProvider không lấp đầy lại DB (no initial pull), UI hiển thị rỗng.

E2E reproduction: `npx tsc -p tsconfig.e2e.json && node e2e-dist/site-switch-sync.spec.js` (file: `e2e/site-switch-sync.spec.ts`).

---

## Endpoint / chức năng bị ảnh hưởng

| Thành phần | Vai trò | Ảnh hưởng |
|---|---|---|
| `SyncProvider` (client) | Pull initial events, open SSE | Không pull lại khi tenant thay đổi nếu `lastSeq` không reset |
| `site-manager.switchToDivision()` | Set tenant cookie, clear local DB | Xóa DB nhưng không khởi tạo lại checkpoint sync |
| `api-mcm-connector-internal` endpoints | `sync/pull/events`, `sync/pull` | Không được trigger để phục hồi trạng thái local tự động |

---

## Đầu vào / Đầu ra mong muốn vs thực tế

Đầu vào: user action `switchToDivision()`

Đầu ra mong muốn:
- Local DB cleared
- Sync provider immediately performs initial pull (or restart) for the new tenant and repopulates DB

Đầu ra thực tế:
- Local DB cleared
- `lastSeq` giữ nguyên → SyncProvider cho rằng không cần pull → DB vẫn rỗng

---

## Nguyên nhân gốc

- `clearLocalData()` xóa các bảng `conversations`, `messages`, `events`, nhưng không reset/checkpoint `lastSeq` cho tenant.
- `SyncProvider` quyết định pull dựa trên `lastSeq` (checkpoint). Nếu `lastSeq` tồn tại và > 0, provider sẽ gọi `syncPullEventsAction(since)` với giá trị đó — điều này dẫn đến không lấy lại dữ liệu đã bị xóa.
- Không có cơ chế trực tiếp giữa `switchToDivision()` và `SyncProvider` để ép restart/startSync ngay lập tức (close SSE → pull from zero → open SSE).

---

## Thay đổi tạm thời (đã áp dụng)

- **Loại bỏ cuộc gọi xóa local data tự động**: `switchToDivision()` đã tạm thời không còn gọi `clearLocalData()` — mục tiêu tránh mất dữ liệu người dùng không chủ ý khi đổi tenant.
- E2E `site-switch-sync.spec.ts` vẫn tồn tại để xác minh hành vi trước và sau thay đổi; test đã chạy trong môi trường local.

Lưu ý: bản thay đổi trước đó (reset `lastSeq`) không còn cần nếu không thực hiện clearLocalData tự động; nếu sau này tái áp dụng clearLocalData thì cần restore checkpoint-reset hoặc restart flow.

---

## Giải pháp đề xuất (đề nghị triển khai chính thức)

1. Primary fix (recommended):
    - Prefer giữ **không tự động xóa** local data khi người dùng chuyển Division.
    - Nếu vẫn cần xóa local data (ví dụ trường hợp chuyển tenant cần thay đổi bảo mật), thực hiện một re-init flow:
       - Stop/close existing SSE connection in `SyncProvider`.
       - Reset or delete `lastSeq` checkpoint for the tenant (prefer delete or set to 0).
       - Trigger `syncPullEventsAction(0)` / initial catchup and wait for completion (or start background paged pulls).
       - Open new SSE EventSource for the new tenant after catchup begins/confirmed.
    - Rationale: đảm bảo race-free reinitialization và tránh missing data.

2. Secondary improvements:
   - Make `switchToDivision()` emit a domain event (`window.dispatchEvent(new CustomEvent('tenant:changed'))`) and have `SyncProvider` listen and perform restart flow (clean separation of concerns).
   - Implement throttling/paging for initial pulls to avoid saturating network when tenant has large dataset.

3. Observability & Tests:
   - Add logs/metrics when `lastSeq` reset and when initial catchup completes.
   - Add unit test for `clearLocalData()` ensuring `lastSeq` is reset/removed.
   - Add integration/e2e asserting that after tenant switch DB is re-populated (staging dataset or mocked responses).

---

## Các file cần thay đổi

| File | Thay đổi đề xuất |
|---|---|
| `web-mcm-messenger/src/lib/site-manager.ts` | **Removed automatic `clearLocalData()` call on tenant switch**; if auto-clear is reintroduced, ensure `lastSeq` is reset and restart flow invoked |
| `web-mcm-messenger/src/lib/sync/SyncProvider.tsx` | Add listener to tenant-change event; implement safe restart: close SSE → initial pull (since=0) → open SSE; improve initial catchup logic to always run when `lastSeq` absent/zero |
| `web-mcm-messenger/src/lib/db/local-database.ts` | (Optional) helper APIs to delete `lastSeq` record easily |
| `web-mcm-messenger/e2e/site-switch-sync.spec.ts` | Existing reproduction test — extend to validate DB re-population after fix in staging CI |

---

## Tiêu chí nghiệm thu

- Sau thay đổi tạm thời: khi chuyển Division, dữ liệu local vẫn tồn tại (không bị xóa) và UI không làm mất state người dùng.
- Khi triển khai tính năng xóa local thủ công (Profile Menu): thao tác xóa sẽ clear DB và khởi trigger re-sync an toàn (initial pull hoặc restart flow).
- E2E/CI: bổ sung test cho luồng manual-clear → verify re-sync tiếp theo.

---

## Ghi chú vận hành

- Reset `lastSeq` sẽ gây initial pull lớn nếu tenant có nhiều dữ liệu — cần xác minh kích thước dataset và áp dụng paging/throttling.
- Nếu muốn tránh full-pull, cân nhắc server-side endpoint `sync/pull/sinceTimestamp` hoặc snapshot API để phục hồi nhanh hơn.
