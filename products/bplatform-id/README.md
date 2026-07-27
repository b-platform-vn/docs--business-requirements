# B-Platform / ID

> Central authentication, authorization, and identity management service for the B-Platform ecosystem.

**Category:** Back-office / Platform Foundation

## Mission

B-Platform / ID provides a shared identity layer that allows B-Platform applications to authenticate users through one trusted service. Once a user is authenticated with any integrated application, other integrated applications can rely on the same authentication service so the user does not need to sign in again unnecessarily.

## Role & Responsibility

| Area | Description |
|---|---|
| Category | Back-office / Platform Foundation |
| Domain | Identity, Authentication & Authorization |
| Form Factor | Web authentication experience, internal admin UI, public/internal APIs |
| Users | End users, internal operators, developers, integrated applications |
| Key Outcome | Secure single sign-on across ecosystem applications with centralized user, application, role, and permission management |

## What B-Platform / ID Does

- Provides centralized authentication for ecosystem applications.
- Enables Single Sign-On (SSO) across applications integrated with the authentication service.
- Issues OAuth/OIDC-style authorization codes and tokens for client applications.
- Maintains a secure server-side SSO session so users can return without logging in again during the configured session window.
- Manages registered applications, redirect URIs, scopes, and client configuration.
- Manages users, roles, and permissions for Platform ID administration.
- Provides internal back-office screens for identity and access management operations.

## Core Capabilities

```mermaid
graph TD
    ID[🔐 B-Platform / ID]
    ID --> Auth[Authentication]
    ID --> SSO[Single Sign-On]
    ID --> Apps[Application Management]
    ID --> Users[User Management]
    ID --> Roles[Role Management]
    ID --> Perms[Permission Management]
    ID --> Audit[Security & Session Controls]

    style ID fill:#1a73e8,color:#fff
    style Auth fill:#34a853,color:#fff
    style SSO fill:#9C27B0,color:#fff
    style Apps fill:#FF9800,color:#fff
    style Users fill:#00BCD4,color:#fff
    style Roles fill:#607D8B,color:#fff
    style Perms fill:#E91E63,color:#fff
    style Audit fill:#F44336,color:#fff
```

### 1. Authentication

- Login experience for users accessing integrated applications.
- Credential validation and user session creation.
- Password recovery and account lifecycle support where required.

### 2. Single Sign-On

- Central SSO session shared by integrated applications through the authentication service.
- Users authenticated through one integrated app can access another integrated app without repeating login, subject to session validity and authorization policy.
- Session expiry follows an idle-window model so inactive users are required to authenticate again after the configured period.

### 3. Application Management

- Register and manage OAuth/OIDC client applications.
- Configure client identifiers, redirect URIs, allowed scopes, and application status.
- Support sample/demo clients for validating the full authentication flow.

### 4. User, Role & Permission Management

- Manage user records and account status.
- Define roles for operational access patterns.
- Define permissions and attach them to roles.
- Support internal administration screens for Applications, Users, Roles, and Permissions.

## Architecture Overview

```mermaid
graph TD
    subgraph ClientApps[Integrated Applications]
        WebApp[Web App]
        AdminApp[Internal Admin App]
        SampleApp[Sample OAuth Client]
    end

    subgraph IDPlatform[B-Platform / ID]
        Web[Authentication Web]
        API[Public Auth API]
        InternalWeb[Internal Admin UI]
        InternalAPI[Internal Admin API]
        Session[SSO Session]
        Policy[Roles & Permissions]
    end

    DB[(MSSQL Identity Store)]

    WebApp -->|Authorize request| API
    SampleApp -->|Authorize request| API
    API --> Web
    Web --> Session
    API -->|Token / session validation| Session
    InternalWeb --> InternalAPI
    InternalAPI --> Policy
    API --> DB
    InternalAPI --> DB
    Session --> DB
    Policy --> DB

    style API fill:#1a73e8,color:#fff
    style Web fill:#34a853,color:#fff
    style InternalWeb fill:#00BCD4,color:#fff
    style InternalAPI fill:#607D8B,color:#fff
    style Session fill:#9C27B0,color:#fff
    style Policy fill:#FF9800,color:#fff
    style DB fill:#455A64,color:#fff
```

## Tools, Apps & Platforms

| Tool / App / Platform | Type | Purpose | Feature Details |
|---|---|---|---|
| Public Auth API | API | Handles OAuth/OIDC-style authorization, token, session, JWKS, introspection, and revoke flows. | _To be documented_ |
| Authentication Web | Web App | User-facing login and authorization experience for integrated applications. | _To be documented_ |
| Internal Admin API | API | Back-office API for managing applications, users, roles, and permissions. | _To be documented_ |
| Internal Admin UI | Web App | Platform ID management console for internal operators. | _To be documented_ |
| Sample OAuth Client | Web App | Demonstrates and validates the full authentication flow for client applications. | _To be documented_ |

## Dependencies

| Depends On | Category | What It Provides to B-Platform / ID |
|---|---|---|
| MSSQL | Infrastructure | Persistent identity store for users, applications, sessions, roles, and permissions. |
| Integrated applications | Ecosystem apps | OAuth/OIDC client requests and redirect targets. |

## Key Contacts

| Role | Name | Team |
|---|---|---|
| Product Owner | _TBD_ | _TBD_ |
| Tech Lead | _TBD_ | _TBD_ |
| Engineering Manager | _TBD_ | _TBD_ |
