# MCM Events Redis Runbook

Events Redis is a **dedicated** Redis instance separate from the session/cache Redis.
- Session/cache Redis: `redis-master.database.svc.cluster.local:6379` — env var `REDIS_URL`
- Events Redis: `redis-events-master.database.svc.cluster.local:6379` — env var `EVENTS_REDIS_URL`

Services consuming Events Redis: `api-mcm-connector-zalo`, `api-mcm-connector-internal`, `api-mcm-omni-channel`, `api-auth`.

## Confirm AOF persistence

```bash
kubectl exec -n database deploy/redis-events-master -- redis-cli CONFIG GET appendonly
# → "appendonly" "yes"

kubectl exec -n database deploy/redis-events-master -- redis-cli CONFIG GET appendfsync
# → "appendfsync" "everysec"
```

## Inspect a stream

```bash
# Summary — length, first/last entry, consumer groups
kubectl exec -n database deploy/redis-events-master -- redis-cli XINFO STREAM mcm:events:<stream-name>

# List consumer groups
kubectl exec -n database deploy/redis-events-master -- redis-cli XINFO GROUPS mcm:events:<stream-name>
```

## Check pending entries (PEL)

```bash
# Summary count
kubectl exec -n database deploy/redis-events-master -- \
  redis-cli XPENDING mcm:events:<stream-name> <group-name>

# Detailed list (first 100 entries idle > 30s)
kubectl exec -n database deploy/redis-events-master -- \
  redis-cli XPENDING mcm:events:<stream-name> <group-name> IDLE 30000 - + 100
```

## Reclaim stuck entries

Reclaim entries that have been pending for more than `EVENTS_MIN_IDLE_MS` (default 30 000 ms) and have exceeded `EVENTS_MAX_DELIVERY_ATTEMPTS` (default 5):

```bash
# Claim entry for a consumer (resets idle clock, increments delivery count)
kubectl exec -n database deploy/redis-events-master -- \
  redis-cli XCLAIM mcm:events:<stream-name> <group-name> <consumer> 30000 <entry-id>

# Acknowledge and remove after manual reprocessing
kubectl exec -n database deploy/redis-events-master -- \
  redis-cli XACK mcm:events:<stream-name> <group-name> <entry-id>
```

## Drain the Dead-Letter Queue (DLQ)

Entries that exceed `EVENTS_MAX_DELIVERY_ATTEMPTS` are moved to a DLQ stream named `mcm:events:dlq:<original-stream>`.

```bash
# Read all DLQ entries
kubectl exec -n database deploy/redis-events-master -- \
  redis-cli XRANGE mcm:events:dlq:<stream-name> - + COUNT 100

# Delete DLQ entry after investigation
kubectl exec -n database deploy/redis-events-master -- \
  redis-cli XDEL mcm:events:dlq:<stream-name> <entry-id>

# Trim DLQ to last 1000 entries (emergency purge)
kubectl exec -n database deploy/redis-events-master -- \
  redis-cli XTRIM mcm:events:dlq:<stream-name> MAXLEN 1000
```

## Stream size management

Streams are capped at `EVENTS_STREAM_MAXLEN=100000` entries using approximate trimming on XADD. To trim manually:

```bash
kubectl exec -n database deploy/redis-events-master -- \
  redis-cli XTRIM mcm:events:<stream-name> MAXLEN ~ 100000
```

## Round-trip smoke test

```bash
kubectl exec -n database deploy/redis-events-master -- \
  redis-cli XADD mcm:events:test '*' k v

kubectl exec -n database deploy/redis-events-master -- \
  redis-cli XREAD COUNT 1 STREAMS mcm:events:test 0
```

## Local development

`redis-events` runs on port **6380** in `docker-compose.local.yml`.

Set `EVENTS_REDIS_URL=redis://localhost:6380` in your local `.env` file.

```bash
# Start local infra (from api-mcm-connector-zalo or any MCM service directory)
docker compose -f docker-compose.local.yml up -d

# Verify AOF is active on the local events Redis
docker compose -f docker-compose.local.yml exec redis-events redis-cli CONFIG GET appendonly
```
