# UC-100 đến UC-401: Nền Tảng GenUI "One Screen Go" – B2B Food Supplement

## Thông Tin Tài Liệu

| Trường | Nội dung |
|---|---|
| **Dự án** | AIFoods – One Screen Go |
| **Phiên bản** | 1.0 |
| **Ngày tạo** | 2026-05-06 |
| **Trạng thái** | Backlog |
| **Nguồn PRD** | Product Requirements Document: "One Screen Go" B2B GenUI Platform |

---

## 1. Tổng Quan

**"One Screen Go"** là nền tảng B2B thế hệ mới dành cho lĩnh vực thực phẩm và thực phẩm bổ sung, hợp nhất toàn bộ hành trình mua sắm — **Khám phá sản phẩm → Tạo đơn hàng → Xác nhận & Thanh toán** — trên một dashboard duy nhất.

### Triết Lý Thiết Kế

- **"One Screen Go"**: Layout đa panel (multi-pane) persistent — người dùng không bao giờ cần điều hướng ra khỏi màn hình chính.
- **Generative UI (GenUI)**: Giao diện thích nghi theo role, lịch sử đặt hàng, và mục tiêu hiện tại của buyer.
- **Predictive UX**: Từ catalog phản ứng (reactive) sang hướng dẫn chủ động (proactive).
- **Zero UI**: Tích hợp voice, gesture, và upload dữ liệu phi cấu trúc (PDF, email).

---

## 2. Cấu Trúc Use Cases

### 🏗️ EPIC Foundation – Kiến Trúc Nền Tảng

| UC | Tiêu Đề | Mức Độ Ưu Tiên | Issue |
|---|---|---|---|
| UC-100 | One Screen Layout – Multi-pane Persistent Dashboard | 🔴 Urgent | BPL-34 |
| UC-101 | Design System – Atomic Design & GenUI Components | 🔴 Urgent | BPL-35 |
| UC-102 | Role-based GenUI Module System | 🟠 High | BPL-36 |

### 🔍 EPIC Phase 1 – Explore Products (AI Discovery)

| UC | Tiêu Đề | Mức Độ Ưu Tiên | Issue |
|---|---|---|---|
| UC-110 | AI-Tailored Catalog – Gợi Ý Sản Phẩm Thông Minh | 🟠 High | BPL-37 |
| UC-111 | Visual Intervention Cues – Tín Hiệu AI Trên Sản Phẩm | 🟡 Medium | BPL-38 |
| UC-112 | Smart Search – Voice Input & NLP Navigation | 🟠 High | BPL-39 |

### 🛒 EPIC Phase 2 – Build Orders (Intelligent Procurement)

| UC | Tiêu Đề | Mức Độ Ưu Tiên | Issue |
|---|---|---|---|
| UC-200 | Order Builder Side-pane – Cart & B2B Control Center | 🔴 Urgent | BPL-40 |
| UC-201 | Smart Capture – PO/PDF Import & Auto-fill Cart | 🟠 High | BPL-41 |
| UC-202 | Consumption-Based Replenishment Engine | 🟠 High | BPL-42 |
| UC-203 | B2B KPI Dashboard – Real-time Procurement Metrics | 🟠 High | BPL-43 |

### 💳 EPIC Phase 3 – Confirm & Pay (Seamless Reconciliation)

| UC | Tiêu Đề | Mức Độ Ưu Tiên | Issue |
|---|---|---|---|
| UC-300 | Three-Way Reconciliation Assistant | 🟠 High | BPL-44 |
| UC-301 | AI Digest – Tóm Tắt Tác Động Đơn Hàng | 🟡 Medium | BPL-45 |
| UC-302 | One-Click Confirmation – Ergonomic Checkout Flow | 🟡 Medium | BPL-46 |

### 💊 EPIC Retention & Anti-Churn

| UC | Tiêu Đề | Mức Độ Ưu Tiên | Issue |
|---|---|---|---|
| UC-400 | Health Scoring Engine – Buyer Engagement Monitor | 🟡 Medium | BPL-47 |
| UC-401 | Adaptive UI Simplification – Chống Paradox of Choice | 🟡 Medium | BPL-48 |

---

## 3. Roadmap Theo Sprint

### Sprint 1–2: Foundation
- UC-100: One Screen Layout
- UC-101: Design System

### Sprint 3–4: Core B2B
- UC-102: GenUI Module System
- UC-200: Order Builder Side-pane
- UC-302: One-Click Confirmation (cơ bản)

### Sprint 5–6: AI Layer 1
- UC-110: AI-Tailored Catalog (rule-based)
- UC-111: Visual Intervention Cues
- UC-201: Smart Capture (PDF/PO)

### Sprint 7–8: AI Layer 2
- UC-112: Smart Search + Voice
- UC-202: Replenishment Engine
- UC-203: KPI Dashboard

### Sprint 9–10: Advanced & Retention
- UC-300: Three-Way Reconciliation
- UC-301: AI Digest
- UC-400: Health Scoring
- UC-401: Adaptive UI Simplification

---

## 4. Phụ Thuộc Kỹ Thuật

```
UC-100 (Layout) ──────────────────────────────────────┐
UC-101 (Design System) ───────────────────────────────┤
                                                       ▼
UC-102 (GenUI Modules) ──────── UC-110 (AI Catalog) ──► UC-111 (Visual Cues)
                        │
                        └──────► UC-200 (Order Builder) ──► UC-201 (Smart Capture)
                                                        ├──► UC-202 (Replenishment)
                                                        └──► UC-203 (KPI Dashboard)
                                                                      │
                                                                      ▼
                                              UC-300 (Reconciliation) ──► UC-301 (AI Digest)
                                                                      └──► UC-302 (Checkout)

UC-202 + UC-203 ─────────────────────────────────────► UC-400 (Health Score) ──► UC-401 (Adaptive UI)
```

---

## 5. Yêu Cầu Kỹ Thuật Tổng Hợp

### Frontend
- Framework: Next.js App Router + TypeScript
- State: Zustand (cross-pane) + React Query (server state)
- UI: Tailwind CSS + Radix UI + shadcn/ui
- Animation: Framer Motion
- Charts: Recharts

### Backend/AI Layer
- API: NestJS hoặc Express (TypeScript)
- Queue: BullMQ (async processing)
- Cache: Redis
- OCR: Google Document AI
- NLP/LLM: OpenAI GPT-4o
- Search: Typesense hoặc Meilisearch
- ML: Python FastAPI microservice (phase 2)

### Infrastructure
- Real-time: WebSocket (KPI dashboard)
- Event tracking: PostHog hoặc custom analytics
- Email: SendGrid / Resend
- Storage: S3-compatible (temporary file upload)

---

## 6. Success Metrics

| Metric | Baseline | Target |
|---|---|---|
| Order Cycle Time (login → checkout) | TBD | Giảm 40% |
| Cart Size / AOV | TBD | Tăng 20% |
| Inventory Accuracy (DoH prediction) | N/A | ≥ 95% |
| Churn Rate (recurring orders) | TBD | Giảm 25% |
| Smart Capture adoption | 0% | ≥ 30% đơn hàng |
| AI Catalog CTR uplift | 0% | ≥ 15% vs. non-AI |
