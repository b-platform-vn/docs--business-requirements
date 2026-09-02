# api-service-agent

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-agent` |
| **v3 target** | `api-service-agent` (L2) — AI / agent domain |
| **Layer** | L2 — API Services |
| **Status** | planned (new) |
| **Language** | TBD (expected NestJS + TypeScript) |
| **Default branch** | `main` |

Last synced: 2026-09-02

## Purpose

AI domain service. Provides the platform's own AI capabilities: chat/assist flows, prompt orchestration, model-provider routing, tool execution, agent workflows, memory/state, and response shaping for internal products.

## Responsibility

- **AI orchestration** — prompt templates, tool calling, multi-step reasoning, and task execution.
- **Model routing** — select among external AI providers or local models behind a stable domain API.
- **Conversation state** — store and resume agent conversations, task context, and run metadata.
- **AI features** — summarization, classification, extraction, search assistance, and assistant-style workflows.
- **Policy boundaries** — apply guardrails, permissions, and tenant/user scope before returning results.

## Dependencies

- **Consumers (L1/L2)**: internal products and backoffice surfaces that need AI assistance; requests should enter through the Service Orchestrator, not via direct peer calls.
- **Messaging / bot delivery**: [`api-service-rocket`](./api-service-rocket.md) for Rocket.Chat notifications and bot replies, routed through the Service Orchestrator.
- **Database Operator (L3)**: [`dbo-head`](./dbo-head.md) — synchronous request/response for all datastore access (runs, prompts, memory, audit). This service does **not** touch the datastore directly.
- **External providers**: LLM / embedding providers, vector stores, and tool adapters (implementation detail; not modeled as repo dependencies).

## Public API surface

- HTTP AI APIs for chat, assistant runs, task execution, summarization, classification, embeddings, and agent state.
- Webhook/event intake for async agent runs and tool completions (shape TBD).

## Notes

- This service owns the AI brain, not transport. It must not talk to Rocket.Chat directly; delivery goes through `api-service-rocket`.
- Cross-domain calls from other L2 services should still be mediated by `api-service-orchestrator`.

## Owner

- Engineering (platform AI team). Architecture: B-Platform architect owns this doc only.
