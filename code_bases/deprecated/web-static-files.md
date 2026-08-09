# web-static-files

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/web-static-files](https://github.com/b-platform-vn/web-static-files) |
| **v3 target** | retire — Zalo domain verification moves to DNS, no static hosting needed |
| **Layer** | (retiring) |
| **Status** | retire |
| **Language** | HTML (233 bytes) + Dockerfile |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Nginx-served static files. Currently hosts Zalo domain verification HTML files and similar static assets.

## Responsibility

- Serve static verification / well-known files over HTTP.

## Dependencies

- **Runtime**: nginx (`nginx.conf` + `html/`).

## Public API surface

- Static HTML files (e.g. Zalo verifier).

## Owner

- DevOps (static hosting). Architecture: B-Platform architect owns this doc only.
