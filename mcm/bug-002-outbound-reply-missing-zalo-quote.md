# Bug-002: Reply Outbound — Quote không hiển thị trong Zalo

**Service:** `api-mcm-connector-internal`, `api-mcm-connector-zalo`  
**Báo cáo:** 2026-04-25  
**Mức độ:** High  
**Kiểm tra trên:** OA `946340628926596746` (Bếp Dì 5 Test) / User `5793224288448186469` (Trần Văn Long)  
**Nguồn:** `api-mcm-connector-internal/docs/bug-outbound-reply-missing-zalo-quote/`

---

## Mô tả

Khi nhân viên reply một tin (có quote) trong Web MCM Messenger, tin được gửi đến Zalo **không kèm quote**. Zalo chỉ hiển thị văn bản thuần; tin được trích dẫn không được hiển thị.

---

## Endpoint bị ảnh hưởng

| Service | Method | Endpoint | Vai trò |
|---------|--------|----------|----------|
| `api-mcm-connector-internal` | `POST` | `/api/internal/sync/push` | Nhận reply outbound từ web; trigger chuyển tiếp đến omni-channel → zalo |
| `api-mcm-connector-zalo` | `POST` | `/messages` | Nhận OmniMessage từ omni-channel và gửi đến Zalo Send Message API |

---

## Đầu vào

### `/api/internal/sync/push` — request body (web gửi)

```json
{
  "collection": "messages",
  "docs": [
    {
      "messageId": "m-out-1777097077150",
      "conversationId": "zalo-946340628926596746-5793224288448186469",
      "tenantId": "KD",
      "platform": "zalo",
      "direction": "outbound",
      "content": "Hi, noted!",
      "replyToMessageId": "f928cd12-9bd3-4a05-9ace-7d013340e5d7"
    }
  ]
}
```

> `replyToMessageId` là **UUID nội bộ** của tin được trích dẫn.

---

## Đầu ra mong muốn

### Zalo Send Message API — nội dung nên gửi

```json
{
  "recipient": { "user_id": "5793224288448186469" },
  "message": {
    "text": "Hi, noted!",
    "quote_message_id": "acb08fadd7af31f668b9"
  }
}
```

> `quote_message_id` must be the **Zalo-native msg_id** (`acb08fadd7af31f668b9`), not the internal UUID.

## Actual Output (Broken)

```json
{
  "recipient": { "user_id": "5793224288448186469" },
  "message": {
    "text": "Hi, noted!",
    "quote_message_id": "f928cd12-9bd3-4a05-9ace-7d013340e5d7"
  }
}
```

---

## Nguyên nhân gốc

`replyToMessageId` do web gửi là UUID MongoDB nội bộ. Code trong `SyncService.pushCollection` truyền thẳng vào `normalizeInternalMessage`, dẫn đến `OmniMessage.replyTo.messageId` mang UUID này. Connector-zalo chuyển tiếp dưới dạng `quote_message_id` đến Zalo API. Zalo không nhận ra UUID và bỏ qua im lặng.

**Luồng xử lý:**

```
POST /api/internal/sync/push
  → SyncService.pushCollection()               [sync.service.ts ~line 148]
      → normalizeInternalMessage()             [sync.service.ts ~line 797]
          sets OmniMessage.replyTo.messageId = "f928cd12-..."  ← internal UUID
      → forwardMessageToOmni(omniMessage)
  → POST /messages  [connector-zalo]
      → ZaloService.receiveMessage()
          → buildZaloSendMessagePayload()      [zalo.service.ts line 1198]
              quote_message_id = omniMessage.replyTo?.messageId  ← still UUID
  → Zalo API → ignores unknown quote_message_id → no quote shown
```

---

## Hướng dẫn sửa

Trong `SyncService.pushCollection` (connector-internal), trước khi gọi `normalizeInternalMessage`, chuyển đổi `message.replyToMessageId` từ UUID nội bộ sang ID kênh gốc:

1. Nếu `message.replyToMessageId` có giá trị, query DB tìm `Message` theo `{ messageId: message.replyToMessageId, tenantId }`.
2. Parse chuỗi JSON `externalMetadata` của tin đó.
3. Lấy `sourceMessageId` (Zalo msg_id).
4. Nếu tìm thấy, thay `message.replyToMessageId` bằng `sourceMessageId` trước khi tạo OmniMessage.

**File cần sửa:**

| File | Vị trí |
|------|----------|
| `connector-internal/src/app/sync/sync.service.ts` | `pushCollection()` ~dòng 148, `normalizeInternalMessage()` ~dòng 797 |
| `connector-internal/src/app/sync/sync.service.spec.ts` | Thêm test: reply outbound với `replyToMessageId` được dịch đúng sang Zalo msg_id |
