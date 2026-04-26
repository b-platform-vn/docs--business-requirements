# Bug-003: `sourceMessageId` của tin Outbound không được lưu sau khi Zalo giao tin

**Service:** `api-mcm-connector-internal`  
**Báo cáo:** 2026-04-26  
**Mức độ:** High  
**Cluster / Tenant:** `k8s-dpsrv`, tenantId = `48`, OA `946340628926596746` (Bếp Dì 5 Test)  
**Nguồn:** `api-mcm-connector-internal/docs/bug-outbound-sourcemessageid-not-stored/`

---

## Tóm tắt

Khi nhân viên gửi tin từ Connector Internal đến user Zalo, tin outbound được lưu trong collection `messages` với `sourceMessageId = null`. Zalo trả về `msg_id` riêng lúc gửi, nhưng ID này **không bao giờ được ghi trở lại** vào bản ghi tin outbound.

Hậu quả: khi user Zalo reply tin outbound đó, reply inbound đến với `replyTo.messageId` là Zalo `msg_id`. Hệ thống không thể ánh xạ sang UUID nội bộ, dẫn đến quote hiển thị `"..."` và click không scroll được.

---

## Cách tái hiện

1. User Zalo gửi tin → tin xuất hiện trong Web MCM Messenger (inbound).
2. Nhân viên reply trong Web MCM Messenger → `POST /api/internal/sync/push` được gọi với `replyToMessageId` = UUID nội bộ của tin inbound.
3. Connector Internal chuyển tiếp đến omni-channel → connector-zalo → Zalo API.
4. Zalo API trả về `msg_id` (ví dụ: `9394f177a672402b1964`).
5. User Zalo reply lại tin của nhân viên. Webhook đến với `quote_msg_id = "9394f177a672402b1964"`.
6. Quote hiển thị `"..."` và click không làm gì.

---

## Phân tích Nguyên nhân Gốc

### Vòng đời ID của tin Outbound

```
Web (push)                         Connector-Internal            Omni-Channel           Connector-Zalo        Zalo API
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
POST /sync/push
  messageId = "m-out-..."           ─── stored, sourceMessageId = null ─────────────────────────────────────────────▶
                                     normalizeInternalMessage()
                                     omniMessage.sourceMessageId = "m-out-..."
                                     ───────────────────────────────────────────────────────────────────────────────▶
                                                                   Zalo API returns msg_id = "9394f177..."
                                                                   ◀── channelMessageId = "9394f177..."
                                     ◀── forwardResult.response ──────────────────────────────────────────────────
                                     forwardResult IS available ✓
                                     BUT channelMessageId is NEVER written back to messages.sourceMessageId  ← BUG
```

### Xảy ra gì khi nhận Reply Inbound

```
Zalo webhook: quote_msg_id = "9394f177a672402b1964"
  → connector-zalo: omniMessage.replyTo.messageId = "9394f177a672402b1964"
  → connector-internal: receiveMessage()
       resolveSourceMessageId("9394f177a672402b1964")
         → messageRepo.findOne({ where: { sourceMessageId: "9394f177a672402b1964" } })
         → NONE FOUND  (outbound record has sourceMessageId = null)
         → fallback: returns "9394f177a672402b1964" unchanged
       internalMessage.replyToMessageId       = "9394f177a672402b1964"  ← Zalo msg_id leaked through
       internalMessage.replyToMessagePreview  = undefined
       internalMessage.replyToMessageSenderName = undefined
```

---

## Endpoint bị ảnh hưởng

| Endpoint | Vai trò |
|----------|----------|
| `POST /api/internal/sync/push` (`SyncService.push`) | Nhận tin outbound, chuyển tiếp đến omni-channel, bỏ qua `channelMessageId` trả về |
| `POST /api/internal/messages` (`SyncService.receiveMessage`) | Nhận reply inbound; `resolveSourceMessageId` không giải quyết được Zalo delivery msg_id |

---

## Lỗ hổng trong sửa chữa của Image `:24`

Fix của Bug-001 đã thêm `resolveSourceMessageId()` dịch Zalo `msg_id → UUID` dùng `Message.sourceMessageId`. Hoạt động tốt với **tin inbound** có `sourceMessageId` được tạo khi khởi tạo.

Tuy nhiên, **tin outbound** có `sourceMessageId = null` vì Zalo delivery `msg_id` trong response forward **không bao giờ được ghi trở lại** vào bản ghi tin outbound.

---

## Hướng dẫn sửa

Trong `SyncService.push()`, sau khi chuyển tiếp thành công, đọc `forwardResult.response.forwardResults[0].channelMessageId` và cập nhật `Message.sourceMessageId` cho tin đã được push.

```
push() flow after fix:
  forwardResult.success = true
  channelMessageId = forwardResult.response.forwardResults[0].channelMessageId  // "9394f177a672402b1964"
  UPDATE messages SET sourceMessageId = channelMessageId WHERE messageId = id AND tenantId = tenantId
```

### Thay đổi trong `sync.service.ts`

1. **`SyncService.push()`** — sau khi chuyển tiếp thành công, gọi `extractChannelMessageId(forwardResult.response)` và `updateMessageSourceId(tenantId, id, channelMessageId)`.
2. **`extractChannelMessageId(response)`** — helper private mới; đọc an toàn `forwardResults[0].channelMessageId` từ response body omni-channel.
3. **`updateMessageSourceId(tenantId, messageId, channelMessageId)`** — helper private mới; ghi `sourceMessageId` + `updatedAt` trở lại vào bản ghi tin outbound.

---

## Bug liên quan

- **Bug-001: inbound-reply-broken-quote** — đã sửa trong `:24`; `resolveSourceMessageId` nay dịch inbound Zalo `msg_id → UUID`. Fix này đúng nhưng phụ thuộc vào `sourceMessageId` của tin outbound được lưu — đúng là điều bug này ngăn lại.
- **Bug-002: outbound-reply-missing-zalo-quote** — đã sửa trong `:24`; `push()` nay dịch `replyToMessageId` UUID → Zalo `msg_id` trước khi gửi.
