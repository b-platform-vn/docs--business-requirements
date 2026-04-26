# Bug-001: Reply Inbound từ Zalo — Quote hiển thị "..." và không scroll được

**Service:** `api-mcm-connector-zalo`, `api-mcm-connector-internal`  
**Báo cáo:** 2026-04-25  
**Mức độ:** High  
**Kiểm tra trên:** OA `946340628926596746` (Bếp Dì 5 Test) / User `5793224288448186469` (Trần Văn Long)  
**Nguồn:** `api-mcm-connector-internal/docs/bug-inbound-reply-broken-quote/`

---

## Mô tả

Khi khách hàng reply (quote) một tin nhắn trong ứng dụng Zalo, khối quote hiển thị trong Web MCM Messenger nhưng bị lỗi:
- Nội dung preview hiển thị `"..."` thay vì nội dung thực của tin được trích dẫn.
- `replyToMessageSenderName` rỗng.
- Click vào khối quote không scroll đến tin gốc.

---

## Endpoint bị ảnh hưởng

| Service | Method | Endpoint | Vai trò |
|---------|--------|----------|----------|
| `api-mcm-connector-zalo` | `POST` | `/webhook` | Nhận sự kiện `user_send_text` từ Zalo chứa `quote_msg_id` |
| `api-mcm-connector-internal` | `POST` | `/api/internal/messages` | Nhận OmniMessage từ omni-channel và lưu thành tin nội bộ |
| `api-mcm-connector-internal` | `GET` | `/api/internal/sync/pull` | Web poll để lấy tin; `replyToMessageId` sai được trả về ở đây |

---

## Đầu vào

### Zalo Webhook payload (inbound)

```json
{
  "app_id": "1657038245155334077",
  "event_name": "user_send_text",
  "sender": { "id": "5793224288448186469" },
  "recipient": { "id": "946340628926596746" },
  "message": {
    "text": "Ok got it",
    "msg_id": "acb08fadd7af31f668b9",
    "quote_msg_id": "792022fb76f990a0c9ef"
  },
  "timestamp": "1777097085069"
}
```

> `quote_msg_id` là **Zalo-native msg_id** của tin được trích dẫn.

---

## Đầu ra mong muốn

```json
{
  "messageId": "<new-uuid>",
  "replyToMessageId": "f928cd12-9bd3-4a05-9ace-7d013340e5d7",
  "replyToMessagePreview": "Hi",
  "replyToMessageSenderName": "Agent Name"
}
```

> `replyToMessageId` phải được chuyển đổi sang **UUID nội bộ** của tin được trích dẫn.

## Đầu ra thực tế (Lỗi)

```json
{
  "messageId": "<new-uuid>",
  "replyToMessageId": "792022fb76f990a0c9ef",
  "replyToMessagePreview": null,
  "replyToMessageSenderName": null
}
```

---

## Nguyên nhân gốc

Connector-zalo map `payload.message.quote_msg_id` trực tiếp vào `OmniMessage.replyTo.messageId` (Zalo msg_id). `resolveReplyContext` của connector-internal query `messageRepo.findOne({ messageId: "<zalo_msg_id>" })` — nhưng tất cả bản ghi `Message` đều dùng UUID nội bộ làm `messageId`. Zalo msg_id chỉ được lưu bên trong chuỗi JSON `externalMetadata` không có index.

**Luồng xử lý:**

```
POST /webhook  [connector-zalo]
  → ZaloService.handleWebhook()
      replyTo.messageId = payload.message.quote_msg_id   ← Zalo msg_id "792022fb..."
  → forwards OmniMessage to omni-channel
  → POST /api/internal/messages  [connector-internal]
      → SyncService.receiveMessage()                     [sync.service.ts ~line 185]
          → resolveReplyContext({ replyToMessageId: "792022fb..." })
              messageRepo.findOne({ messageId: "792022fb..." })  → null
              returns { preview: undefined, senderName: undefined }
          → replyToMessageId = "792022fb..."             ← Zalo msg_id saved to DB

GET /api/internal/sync/pull  [connector-internal]
  → web builds messageMap: Map<messageId(UUID), MCMMessage>
  → messageMap.get("792022fb...") → undefined            ← scroll-to fails
```

---

## Hướng dẫn sửa

Trong `SyncService.receiveMessage` (connector-internal), **trước** khi gọi `convertOmniMessageToInternalWithResolvedContext`, chuyển đổi `omniMessage.replyTo.messageId` từ Zalo msg_id sang UUID nội bộ:

1. Query DB tìm `Message` có chuỗi `externalMetadata` chứa `sourceMessageId = "<zalo_msg_id>"` và `tenantId = <tenantId>`.
2. Nếu tìm thấy, thay `omniMessage.replyTo.messageId` bằng `messageId` (UUID) của tin đó.
3. `resolveReplyContext` khi đó sẽ tìm thấy tin và điền đúng `preview` và `senderName`.

**File cần sửa:**

| File | Vị trí |
|------|----------|
| `connector-internal/src/app/sync/sync.service.ts` | `receiveMessage()` ~dòng 185, `resolveReplyContext()` ~dòng 652 |
| `connector-internal/src/app/sync/sync.service.spec.ts` | Thêm test: reply inbound với `replyTo.messageId` (Zalo msg_id) được giải quyết đúng thành UUID nội bộ |

---

## Sửa chữa kiến trúc nắn dài

Thêm cột `sourceMessageId` có index vào entity `Message` để tra cứu hiệu quả:

```typescript
// connector-internal/src/app/sync/entities/message.entity.ts
@Column({ nullable: true })
@Index()
sourceMessageId?: string;  // Zalo msg_id / Facebook mid / etc.
```

Populate trường này khi lưu bất kỳ tin inbound nào. Giúp việc dịch Zalo msg_id → UUID trở thành một query indexed dùng cho cả bug này lẫn Bug-002.
