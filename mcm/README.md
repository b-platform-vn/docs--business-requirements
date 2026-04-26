# MCM — Nền tảng Nhắn tin Đa kênh

## Tổng quan

MCM (Multi-Channel Messaging) là nền tảng nhắn tin đa kênh của B-Platform, cho phép doanh nghiệp giao tiếp với khách hàng qua nhiều kênh nhắn tin (Zalo, Facebook Messenger, ...) thông qua một giao diện tổng hợp duy nhất cho nhân viên tư vấn.

## Kiến trúc hệ thống

Nền tảng MCM gồm các service sau:

| Service | Vai trò |
|---------|---------|
| `api-mcm-omni-channel` | Định tuyến, chuyển tiếp và quản lý retry tin nhắn trung tâm |
| `api-mcm-connector-zalo` | Connector kênh Zalo — xử lý webhook inbound và gửi tin outbound |
| `api-mcm-connector-facebook` | Connector kênh Facebook Messenger — xử lý webhook inbound và gửi tin outbound |
| `api-mcm-connector-internal` | Lưu trữ nội bộ, quản lý tin nhắn/hội thoại theo tenant, đồng bộ với Web Messenger |
| `web-mcm-messenger` | Giao diện web cho nhân viên quản lý hội thoại và trả lời khách hàng |
| `api-auth` | Dịch vụ xác thực và phân quyền dùng chung |

## Luồng tin nhắn

```
Khách hàng (Zalo / Facebook)
    │
    ▼
Channel Connector (connector-zalo / connector-facebook)
    │  chuẩn hoá thành OmniMessage, chuyển tiếp tin inbound
    ▼
Omni Channel (api-mcm-omni-channel)
    │  định tuyến đến các đích đăng ký, quản lý retry backoff
    ▼
Connector Internal (api-mcm-connector-internal)
    │  lưu hội thoại, tin nhắn, khách hàng; đồng bộ với Web Messenger
    ▼
Web Messenger (web-mcm-messenger)
    │  nhân viên trả lời qua push sync
    ▼
Connector Internal → Omni Channel → Channel Connector → Khách hàng
```

## Mô hình Tenant

Mỗi workspace của nhân viên được phân vùng theo **Khối** (`MaKhoi`) — một chuỗi định danh lấy từ cấu trúc tổ chức B-Platform (`tblKhoi`). Header `X-Tenant-Id` mang giá trị `MaKhoi` trong mọi request đã xác thực.
