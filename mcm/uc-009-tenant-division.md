# UC-009: Tenant = Khối (MaKhoi)

**Service:** `api-auth`, `api-mcm-connector-internal`, `api-mcm-connector-facebook`  
**Loại:** Use Case  
**Nguồn:**
- `api-auth/docs/feature-tenant-division/`
- `api-mcm-connector-internal/docs/feature-tenant-division/`
- `api-mcm-connector-facebook/docs/feature-tenant-division/`

## Tổng quan

Tenant context nay dựa trên **Khối** (`tblKhoi.MaKhoi`) thay vì **Site** (`tblSite.IDSite`).  
Một user thuộc một hoặc nhiều Site (`tblUserSite`), mỗi Site thuộc một Khối (`tblSite.IDKhoi → tblKhoi`).  
Mã Khối (`MaKhoi`, chuỗi) là định danh tenant mới được sử dụng xuyên suốt nền tảng.

Thay đổi này ảnh hưởng tất cả service MCM trước đây dùng `IDSite` (số nguyên) làm tenant identifier.

---

## Phần 1 — Thay đổi `api-auth`

### Sửa: `Authentication/Login`

**Endpoint:** `POST /api/auth/login` (không đổi)

**Response hiện tại:**
```json
{
  "access_token": "...",
  "refresh_token": "...",
  "user": {
    "idUser": 1,
    "username": "abc",
    "fullname": "Nguyen Van A",
    "sites": [{ "idSite": 3, "maSite": "HN", "tenSite": "Hà Nội" }]
  }
}
```

**Response mới:**
```json
{
  "access_token": "...",
  "refresh_token": "...",
  "user": {
    "idUser": 1,
    "username": "abc",
    "fullname": "Nguyen Van A",
    "divisions": [{ "idKhoi": 1, "maKhoi": "KD", "tenKhoi": "Khối Kinh Doanh" }]
  }
}
```

---

### Mới: `Authentication/Divisions`

**Endpoint:** `GET /api/auth/divisions`  
**Headers:** `authorization` (bắt buộc): Bearer access token

**Logic:**
1. Xác thực access token.
2. Tải tất cả bản ghi `UserSite` cho user đang xác thực.
3. Với mỗi site, join sang `tblSite` lấy `IDKhoi`.
4. Tải bản ghi `tblKhoi` cho các `IDKhoi` đó, loại trùng.
5. Trả về danh sách.

**Kết quả trả về:**
```json
[
  { "idKhoi": 1, "maKhoi": "KD", "tenKhoi": "Khối Kinh Doanh" }
]
```

---

### Mới: `Authentication/Switch-Division`

**Endpoint:** `POST /api/auth/switch-division`  
**Headers:** `authorization` (bắt buộc): Bearer access token  
**Body:** `{ "maKhoi": "KD" }`

**Logic:**
1. Xác thực access token.
2. Tìm bản ghi `Division` theo `maKhoi`.
3. Xác nhận user có ít nhất một `UserSite` có `tblSite.IDKhoi` khớp với Khối yêu cầu.
4. Trả về thông tin Khối nếu hợp lệ; `401 Unauthorized` nếu không.

**Kết quả trả về:**
```json
{
  "success": true,
  "division": { "idKhoi": 1, "maKhoi": "KD", "tenKhoi": "Khối Kinh Doanh" }
}
```

**Ghi chú:**
- `Authentication/Sites` và `Authentication/Switch-Site` **không bị xóa** — vẫn giữ nhưng không còn bất kỳ service MCM nào gọi.
- `web-mcm-messenger` sẽ gọi `Authentication/Divisions` để hiển thị trang chọn Khối, và đọc `divisions` từ response login để điền sẵn.

---

## Phần 2 — Thay đổi `api-mcm-connector-internal`

### `Internal/Pull/Collection` · `Internal/Push/Collection` · `Internal/Pull/Events` · `Internal/Stream/Events`

All sync endpoints are protected by `TenantContextInterceptor`.

**Hành vi hiện tại:**
- Header `X-Tenant-Id` được parse thành số nguyên → `siteId`.
- Xác thực bằng `POST /api/auth/switch-site` với body `{ siteId: number }`.
- `request.tenantContext` được gán là `{ userId: number, siteId: number }`.

**Hành vi mới:**
- Header `X-Tenant-Id` được đọc dưới dạng chuỗi thuần → `tenantId` (`maKhoi`). Không parse số nguyên.
- Xác thực bằng `POST /api/auth/switch-division` với body `{ maKhoi: string }`.
- `request.tenantContext` được gán là `{ userId: number, tenantId: string }`.

**Đầu vào (không đổi từ bên ngoài):**
```
GET /api/internal/sync/pull?collection=messages
X-Tenant-Id: KD
Authorization: Bearer <token>
```

**Đầu ra:** Không đổi — cùng dữ liệu collection, nay phân vùng theo `maKhoi`.

**Tác động phụ:**
- `messages`, `conversations`, `customers`, `users` records stored/queried with `tenantId = maKhoi`.

---

### Thay đổi Interface

#### `RequestWithTenant`

| Trường | Trước | Sau |
|---------|--------|-----|
| `tenantContext.siteId` | `number` (IDSite) | bỏ |
| `tenantContext.tenantId` | — | `string` (MaKhoi) |

#### Thông điệp lỗi

| Trước | Sau |
|--------|-----|
| `"Tenant ID required. Please select a site first."` | `"Tenant ID required. Please select a division first."` |
| `"Invalid tenant ID format"` (kiểm tra số nguyên) | bỏ (chuỗi không cần kiểm tra format) |

---

### Thay đổi API được sử dụng

| Trước | Sau |
|--------|-----|
| `Authentication/Switch-Site` · `POST /api/auth/switch-site` · `{ siteId: number }` | `Authentication/Switch-Division` · `POST /api/auth/switch-division` · `{ maKhoi: string }` |

---

## Phần 3 — Thay đổi `api-mcm-connector-facebook`

### `Fb/Webhook`

**Hành vi hiện tại:**
- `this.tenantId` được đọc từ `process.env['DEFAULT_TENANT_ID']`.
- Dùng trong `QUEUES.OMNI_INBOUND(this.tenantId)` làm tên hàng đợi RabbitMQ.
- Được nhúc vào `MCMMessage.tenantId` của mọi tin inbound được publish.

**Hành vi mới:**
- Không thay đổi code — cùng biến môi trường `DEFAULT_TENANT_ID`.
- **Giá trị** của `DEFAULT_TENANT_ID` phải được cập nhật từ site ID số nguyên sang `MaKhoi` chuỗi phù hợp.

**Tác động phụ:**
- `MCMMessage.tenantId` trong hàng đợi broker giờ sẽ mang `MaKhoi` thay vì số nguyên site ID.
- Mọi consumer downstream đọc hàng này phải sẵn sàng với `MaKhoi` làm `tenantId`.

### Thay đổi cấu hình

| Biến môi trường | Trước | Sau |
|-----------------|--------|-----|
| `DEFAULT_TENANT_ID` | Site ID số nguyên (ví dụ `"3"`) | Mã Khối (ví dụ `"KD"`) |

Thay đổi này phải được áp dụng trên tất cả môi trường deploy (`.env` local, Kubernetes secrets / ConfigMaps).
