# Customer Relationship Management (CRM)

> The central platform for everything about the customer: **Customer Management**, **Customer Communication**, and **Customer Support**. It unifies customer data and every conversation — regardless of channel — into one workspace for In-house Customer Service and Freelancer (contractor) Customer Service teams.

**Category:** Back-office

## Mission

Provide a single, powerful workspace that centralizes customer records, conversations, and support activities. It enables Customer Service teams (both in-house and freelancer contractors) to know their customers, respond faster across every channel, resolve issues efficiently, and convert conversations into sales.

## Role & Responsibility

| Area | Description |
|---|---|
| Category | Back-office (supports operation & customer support) |
| Domain | Customer Management, Communication & Support |
| Users | In-house CS Agents, Freelancer (Contractor) CS Agents, Management |
| Key Outcome | Unified customer view with omni-channel communication, support, and seamless sales conversion |

## Pillars

CRM covers three core pillars:

```mermaid
graph TD
    CRM[📇 CRM]
    CRM --> CM[👥 Customer Management]
    CRM --> CC[💬 Customer Communication]
    CRM --> CS[🎧 Customer Support]

    style CRM fill:#1a73e8,color:#fff
    style CM fill:#34a853,color:#fff
    style CC fill:#FF9800,color:#fff
    style CS fill:#9C27B0,color:#fff
```

### 1. Customer Management

- Centralized customer profiles (identity, contact details, history, tags, segments)
- Unified customer record consolidated across all channels and touchpoints
- Customer lifecycle and relationship tracking
- Segmentation for targeting and personalization

### 2. Customer Communication

Unified, multi-channel messaging that brings every conversation into one screen.

Collect customer messages from multiple social and messaging platforms:

- Facebook Messenger
- WhatsApp
- Zalo
- Telegram
- Email
- Web Widget (Live Chat)

```mermaid
graph LR
    FB[Facebook Messenger] --> CRM[🔀 CRM Hub]
    WA[WhatsApp] --> CRM
    ZL[Zalo] --> CRM
    TG[Telegram] --> CRM
    EM[Email] --> CRM
    WW[Web Widget] --> CRM
    CRM --> CS[👤 CS Agent Screen]

    style CRM fill:#1a73e8,color:#fff
    style CS fill:#34a853,color:#fff
    style FB fill:#0084FF,color:#fff
    style WA fill:#25D366,color:#fff
    style ZL fill:#0068FF,color:#fff
    style TG fill:#0088cc,color:#fff
    style EM fill:#EA4335,color:#fff
    style WW fill:#FF9800,color:#fff
```

**Centralized communication view** — all conversations in one screen with clear distinction of:

- **Channel origin** — Which platform the message came from (Facebook, WhatsApp, Zalo, etc.)
- **Sender identity** — Who sent the message (customer name, agent name, bot)
- **Conversation threading** — Grouped by customer across channels

**Unified communication protocol** — a standardized way for CS agents to communicate in both:

- **Proactive mode** — Agents initiate outbound messages (campaigns, follow-ups, notifications)
- **Reactive mode** — Agents respond to incoming customer inquiries

### 3. Customer Support

- Ticket / case management for tracking customer issues to resolution
- Assignment and routing of support requests to the right agent
- SLA tracking and escalation
- Support history tied to the unified customer profile

## Cross-Cutting Capabilities

### Secure Access Control

- Role-based permissions controlling which agents can see which customers / conversations
- Freelancer (contractor) agents have scoped access — only see conversations assigned to them
- In-house agents can have broader or full visibility based on team configuration
- Audit trail for all customer data and message access

### One-Click Sales Conversion

- Turn any customer conversation into a sales order in other products with a single click
- Seamless handoff from communication context to the sales/retail system
- Preserve conversation and customer context so the sales team has full history

## Architecture Overview

```mermaid
graph TD
    subgraph Channels
        FB[Facebook Messenger]
        WA[WhatsApp]
        ZL[Zalo]
        TG[Telegram]
        EM[Email]
        WW[Web Widget]
    end

    subgraph CRM Platform
        Ingest[📥 Message Ingestion]
        Router[🔀 Channel Router]
        Profile[👥 Customer Profiles]
        Support[🎧 Support / Tickets]
        Store[(💾 Data Store)]
        Auth[🔐 Access Control]
        UI[🖥️ Agent Workspace]
    end

    subgraph Integrations
        Sales[🛒 Sales System]
    end

    FB & WA & ZL & TG & EM & WW --> Ingest
    Ingest --> Router
    Router --> Store
    Profile --> Store
    Support --> Store
    Store --> UI
    Auth --> UI
    UI -->|One-Click Sale| Sales

    style Ingest fill:#1a73e8,color:#fff
    style Router fill:#4285f4,color:#fff
    style Profile fill:#34a853,color:#fff
    style Support fill:#9C27B0,color:#fff
    style Store fill:#607D8B,color:#fff
    style Auth fill:#F44336,color:#fff
    style UI fill:#00BCD4,color:#fff
    style Sales fill:#FF9800,color:#fff
```

## Tools, Apps & Platforms

| Tool / App / Platform | Type | Purpose | Feature Details |
|---|---|---|---|
| _To be documented_ | — | — | — |

> As tools and apps are identified, add them here and create corresponding feature pages under `features/`.

## Key Contacts

| Role | Name | Team |
|---|---|---|
| Product Owner | _TBD_ | _TBD_ |
| Tech Lead | _TBD_ | _TBD_ |
| Engineering Manager | _TBD_ | _TBD_ |
