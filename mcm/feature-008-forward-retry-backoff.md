# Feature-008: Retry Chuyển tiếp — Exponential Backoff

**Service:** `api-mcm-omni-channel`  
**Loại:** Feature  
**Nguồn:** `api-mcm-omni-channel/docs/feature-forward-retry-backoff/`

## Quyết định & Ràng buộc

| # | Câu hỏi | Quyết định |
|---|---------|----------|
| 1 | Nguyên nhân gốc của độ trễ | `nextRetryAt` luôn được lập lịch tại `now + OMNI_RETRY_INTERVAL_MS` (flat 30s). Thay bằng lịch **exponential backoff** theo số lần thử tính bên trong `upsertForwardingRecord()`. |
| 2 | Lịch backoff | `[1s, 5s, 15s, 30s]` — lần 1 thất bại chờ 1s, lần 2 chờ 5s, lần 3 chờ 15s, lần 4+ giới hạn 30s. Mã cứng trong helper mới `getBackoffMs(attemptCount)`. Không cần config mới. |
| 3 | Tốc độ processor tick | `setInterval` của `processPendingForwardingRecords` cần giảm xuống. Đặt `OMNI_RETRY_INTERVAL_MS = 1000` (1s). Nếu không, một `nextRetryAt` 1s vẫn chờ 30s cho processor chạy. |
| 4 | Giữ lại `ForwardingRecord`? | **Giữ nguyên.** Cung cấp audit trail và crash recovery. Không cần thay đổi schema. |
| 5 | Thêm retry ngay in-process? | **Không.** Chỉ dùng queue với lịch nhanh hơn. Không có retry loop in-process. |
| 6 | `OMNI_RETRY_MAX_ATTEMPTS` | Giữ ở **5**. Với lịch mới, 5 lần thử trải dài ~51s (so với 150s hiện tại). |
| 7 | Giữ hay xóa `getRetryIntervalMs()`? | **Giữ** — vẫn điều khiển tốc độ processor tick. Chỉ giá trị env thay đổi từ 30000 → 1000. |
| 8 | Tải DB khi rảnh | Processor query `forwarding_records` 60 lần/phút. Chấp nhận được — query dùng compound index `status + nextRetryAt` và trả về 0 hàng khi rảnh. |

---

## Hàm bị ảnh hưởng

| Hàm | File | Loại thay đổi |
|-----|------|---------------|
| `OmniService.upsertForwardingRecord()` | `src/app/omni/omni.service.ts` | **Sửa** — thay `this.getRetryIntervalMs()` bằng `this.getBackoffMs(nextAttemptCount)` trên dòng `nextRetryAt` |
| `OmniService.getBackoffMs()` | `src/app/omni/omni.service.ts` | **Thêm** — helper private mới; trả về ms từ lịch cố định `[1000, 5000, 15000, 30000]` |
| `OmniService.getRetryIntervalMs()` | `src/app/omni/omni.service.ts` | **Không đổi code** — chỉ giá trị env thay đổi |
| `api-mcm-omni-channel.yaml` | `platform-fluxcd/k8s-dpsrv/backend/` | **Đổi config** — `OMNI_RETRY_INTERVAL_MS: "1000"` |

---

## Cải thiện

| Chỉ số | Trước | Sau |
|--------|---------|----|
| Độ trễ sau lần 1 thất bại | ≥ 30s | ≥ 1s |
| Độ trễ sau lần 2 thất bại | ≥ 60s | ≥ 6s |
| Độ trễ sau lần 3 thất bại | ≥ 90s | ≥ 21s |
| Thời gian tối đa (5 lần thử) | 150s | 51s |
| Độ nhạy của processor | ±30s | ±1s |

---

## Kế hoạch triển khai

### Bước 1 — Thêm helper `getBackoffMs` vào `OmniService`

```ts
private getBackoffMs(attemptCount: number): number {
  const schedule = [1000, 5000, 15000, 30000];
  const index = Math.min(attemptCount - 1, schedule.length - 1);
  return schedule[index];
}
```

### Bước 2 — Sửa `upsertForwardingRecord`

```ts
// BEFORE (line ~456)
record.nextRetryAt = this.addMilliseconds(now, this.getRetryIntervalMs());

// AFTER
record.nextRetryAt = this.addMilliseconds(now, this.getBackoffMs(nextAttemptCount));
```

### Bước 3 — Đổi config trong FluxCD

```yaml
# platform-fluxcd/k8s-dpsrv/backend/api-mcm-omni-channel.yaml
- name: OMNI_RETRY_INTERVAL_MS
  value: "1000"            # was "30000"
```
