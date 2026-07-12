# MCM — Multi-Channel Messengers

Nền tảng nhắn tin đa kênh của B-Platform. Cho phép nhân viên giao tiếp với khách hàng qua nhiều kênh (Zalo OA, Facebook, WhatsApp, Internal) trên một giao diện thống nhất, phân chia theo tenant (Khối / MaKhoi).

## Overall Architecture

MCM đang chuyển từ mô hình **REST-only** (các service gọi HTTP trực tiếp lẫn nhau) sang mô hình **event-driven qua Redis Streams**. REST vẫn giữ lại cho các đường request/response đồng bộ (webhook từ bên ngoài, trình duyệt gọi backend, xác thực). Bus nội bộ chuyển sang Redis Streams cho mọi luồng fan-out, retry và fire-and-forget.

```
                              EXTERNAL
                                 │
      ┌──────────────────────────┼──────────────────────────────┐
      │                          │                              │
      ▼ (HTTPS webhook)          ▼ (HTTPS browser)              ▼ (HTTPS webhook)
┌──────────────┐          ┌──────────────────┐          ┌──────────────┐
│  Zalo OA     │          │  web-mcm-        │          │  Facebook /  │
│  Platform    │          │  messenger       │          │  WhatsApp    │
└──────┬───────┘          │  (Next.js)       │          └──────┬───────┘
       │                  └────┬─────────────┘                 │
       │                       │   ▲                           │
       │                       │   │ SSE (fetch-event-source)  │
       │                       ▼   │                           │
       │              ┌─────────────────────────┐              │
       │              │       REST + SSE        │              │
       │              │  (browser-facing only)  │              │
       │              └────┬───────────┬────────┘              │
       │                   │           │                       │
       ▼                   ▼           ▼                       ▼
┌──────────────┐   ┌──────────────┐  ┌────────────────┐  ┌──────────────┐
│  connector-  │   │  api-auth    │  │  connector-    │  │  connector-  │
│  zalo        │   │  (JWT,       │  │  internal      │  │  facebook /  │
│  (webhook,   │   │  divisions)  │  │  (sync API,    │  │  whatsapp    │
│   OAuth)     │◄──┤              │  │   SSE fan-out) │  │  (future)    │
└──────┬───────┘   └──────┬───────┘  └────┬───────────┘  └──────┬───────┘
       │                  │               │                     │
       │        validate  │               │                     │
       │        (REST)    │               │                     │
       │                  ▼               │                     │
       │           ┌──────────────┐       │                     │
       │           │  MSSQL       │       │                     │
       │           │  (users,     │       │                     │
       │           │   sites,     │       │                     │
       │           │   khoi)      │       │                     │
       │           └──────────────┘       │                     │
       │                                  │                     │
       ▼                                  ▼                     ▼
╔═══════════════════════════════════════════════════════════════════════╗
║                                                                       ║
║             REDIS STREAMS  (event bus, at-least-once)                 ║
║                                                                       ║
║   mcm:events:message:inbound            ── from any connector         ║
║   mcm:events:message:outbound:{shard}   ── sharded by conversationId  ║
║   mcm:events:message:delivered                                        ║
║   mcm:events:message:failed                                           ║
║   mcm:events:conversation:updated                                     ║
║   mcm:events:customer:updated                                         ║
║   mcm:events:tenant:division:changed                                  ║
║   mcm:events:deadletter                 ── after N failed XCLAIMs     ║
║                                                                       ║
║   mcm:retry:zset                        ── delayed retry scheduler    ║
║                                                                       ║
║   Consumer groups:                                                    ║
║     group=omni       reads inbound          → publish outbound:{sh}   ║
║     group=sync       reads inbound/delivered/failed → Mongo + SSE     ║
║     group=zalo       reads outbound:*  filter channel=zalo            ║
║     group=facebook   reads outbound:*  filter channel=facebook        ║
║     group=audit      reads *  (future)                                ║
║                                                                       ║
╚═══════════════════════════════════════════════════════════════════════╝
                    ▲                    │                ▲
                    │                    │                │
                    │                    ▼                │
                    │            ┌──────────────┐         │
                    │            │  omni-       │         │
                    │            │  channel     │         │
                    │            │  (routing    │         │
                    │            │   rules,     │         │
                    │            │   retry via  │         │
                    │            │   PEL+XCLAIM │         │
                    │            │   +zset)     │         │
                    │            └──────┬───────┘         │
                    │                   │                 │
                    │                   ▼                 │
                    │            ┌──────────────┐         │
                    │            │  MongoDB     │         │
                    │            │  Atlas       │         │
                    │            │  (routing    │         │
                    │            │   rules)     │         │
                    │            └──────────────┘         │
                    │                                     │
                    │                                     │
              ┌─────┴─────────────────────────────────────┴─────┐
              │                                                 │
              ▼                                                 ▼
      ┌──────────────┐                                  ┌──────────────┐
      │  MongoDB     │                                  │  api-        │
      │  Atlas       │                                  │  filestore   │
      │  (messages,  │                                  │  (S3/GCS/    │
      │   convos,    │                                  │   Azure/     │
      │   customers, │                                  │   local)     │
      │   users)     │                                  └──────────────┘
      └──────┬───────┘
             │
             │ Change Streams (M10+)
             │
             ▼
      ┌──────────────┐
      │  connector-  │
      │  internal    │──► SSE ──► web-mcm-messenger  (browser Web Worker)
      │  SSE fan-out │           (Dexie / IndexedDB, offline-first)
      └──────────────┘
```

### What speaks REST vs. what speaks events

| Boundary | Transport | Why |
|---|---|---|
| External platform (Zalo/FB/WA) → connector | HTTPS webhook | Platforms only speak HTTP; connector translates webhook → event |
| Browser ↔ backend (login, send, upload, actions) | HTTPS REST + Server Actions | Client needs a response value |
| Backend → browser (realtime) | SSE via `@microsoft/fetch-event-source` | One-way push, long-lived, works through the Web Worker |
| Auth validate / switch-division | HTTPS REST (sync) | Hot-path check on every tenant-scoped request |
| Filestore upload/download | HTTPS REST | Binary payloads |
| Connector ↔ omni ↔ sync (any internal fan-out) | **Redis Streams** | Fire-and-forget, multi-subscriber, retry semantics |
| Delayed retries (backoff) | Redis sorted set + scheduler | Replaces omni-channel's in-process retry timer |
| Auth tenant/division changes | Redis Streams (broadcast) | Multiple services invalidate caches |

Rule of thumb: **request/response with a caller waiting for a value = REST. Fire-and-forget or fan-out = event.**

### Event envelope (shared via `@b-platform-vn/sdk-mcm-schemas`)

```
{
  eventId:       uuid       // idempotency key — consumers dedup on this
  eventType:     string     // e.g. "message.inbound.zalo" (v1)
  eventVersion:  int        // schema version for this eventType
  correlationId: uuid       // ties an inbound message to all downstream events
  tenantId:      string     // MaKhoi (Division code)
  occurredAt:    iso8601    // producer clock; consumers do not trust for ordering
  payload:       object     // typed body per eventType
}
```

### Ordering, sharding, delivery

- **At-least-once** delivery. Every consumer MUST be idempotent on `eventId`.
- **Per-conversation FIFO** where it matters (outbound messages): stream is sharded by `hash(conversationId) mod N` (default N=16). One consumer per shard preserves order without serializing the whole system.
- **Retries** — a failed handler leaves the entry in the group's Pending Entries List (PEL). Reclaim after `min-idle-time` via `XCLAIM`. After M attempts, XADD to `mcm:events:deadletter` and XACK the original.
- **Delayed retries** (exponential backoff) — publisher ZADDs `{score = deliverAtEpochMs}` into `mcm:retry:zset`; a scheduler process ZRANGEBYSCORE-pops ripe entries back onto the target stream.

### Durability & operational shape

- Events run on a **separate Redis instance** from session/cache (different failure blast radius, different durability policy).
- **AOF `everysec` minimum**, `always` on the events DB for zero-loss on crash.
- **Retention** per stream via `MAXLEN ~ 100000` or age-based `XTRIM`. Long-term persistence is a *consumer* concern (audit → Mongo), not a Redis one.
- **Metrics that matter**: `XLEN` per stream, `XPENDING` per group (queue depth), consumer lag, DLQ length. These are the new alarm surfaces — memory pressure replaces "queue length" as the primary alert.

### Migration phases

1. **Double-write.** Producers XADD alongside existing REST calls. New consumers (audit, analytics) attach with zero risk to the hot path.
2. **Flip primary path.** Consumers switch to XREADGROUP. Omni's retry timer replaced by PEL + XCLAIM + retry-zset. Service-by-service; each swap is rollback-able.
3. **Remove internal REST.** Delete connector↔omni↔sync HTTP endpoints. Only external webhooks and browser-facing REST/SSE remain.

Each phase is a safe stopping point. Pausing after phase 1 already unlocks new subscribers (audit, dashboards) without touching production traffic.
