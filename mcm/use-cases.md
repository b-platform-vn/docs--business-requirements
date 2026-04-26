# MCM — Tính năng & Use Cases

## Use Cases

- [UC-001: Kết nối Zalo OA](/mcm/uc-001-zalo-connect.md)
- [UC-007: Đồng bộ thời gian thực qua MongoDB Change Streams + SSE](/mcm/uc-007-realtime-sync-sse-changestreams.md)
- [UC-009: Tenant = Khối (MaKhoi)](/mcm/uc-009-tenant-division.md)

## Tính năng

### Kênh Zalo

- [Feature-002: Tự động làm mới Access Token](/mcm/feature-002-auto-refresh-token.md)
- [Feature-003: Nhận tin nhắn từ Zalo (Webhook)](/mcm/feature-003-receive-message-from-zalo.md)
- [Feature-004: Gửi tin nhắn đến Zalo (qua OmniChannel)](/mcm/feature-004-send-message-to-zalo.md)

### Connector Internal & Đồng bộ

- [Feature-005: Nhận tin nhắn từ OmniChannel (Connector Internal)](/mcm/feature-005-receive-message-from-omnichannel.md)
- [Feature-006: Đồng bộ dữ liệu giữa Connector Internal và Web Messenger](/mcm/feature-006-sync-data-connector-internal-web-messenger.md)

### OmniChannel

- [Feature-008: Retry Chuyển tiếp — Exponential Backoff](/mcm/feature-008-forward-retry-backoff.md)

## Bugs

- [Bug-001: Reply Inbound từ Zalo — Quote hiển thị "..." và không scroll được](/mcm/bug-001-inbound-reply-broken-quote.md)
- [Bug-002: Reply Outbound — Quote không hiển thị trong Zalo](/mcm/bug-002-outbound-reply-missing-zalo-quote.md)
- [Bug-003: `sourceMessageId` của tin outbound không được lưu sau khi Zalo giao tin](/mcm/bug-003-outbound-sourcemessageid-not-stored.md)
