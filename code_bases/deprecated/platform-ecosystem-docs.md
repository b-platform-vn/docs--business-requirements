# platform-ecosystem-docs

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/platform-ecosystem-docs](https://github.com/b-platform-vn/platform-ecosystem-docs) |
| **v3 target** | → `docs-platform` (rename + absorb cross-cutting docs) |
| **Layer** | Documentation |
| **Status** | active |
| **Language** | HTML (Docsify) |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

This living documentation site. Docsify-powered static site that mirrors the B-Platform product specs (`products/`) and code-base inventory (`code_bases/`). This very file lives in this repo.

## Responsibility

- Product specs under `products/<product>/` (MDFoods, LFarm, Odeli, UniGate, B-Platform General, CRM).
- Architecture docs under `products/*/architecture/`.
- Code-base mirror under `code_bases/<repo>.md` (this directory).
- Docsify config: `_coverpage.md`, `_navbar.md`, `_sidebar.md`, `index.html`, `CNAME`.

## Dependencies

- **Runtime**: Docsify (served from `index.html`).
- **Build**: `package.json` (local dev scripts).

## Public API surface

- Public website (CNAME-configured).

## Manifest contract

- Docsify layout: `_coverpage.md` + `_navbar.md` + `_sidebar.md` + `index.html` + `CNAME` + `README.md` + `package.json`.

## Owner

- **B-Platform architect** owns this repo's `code_bases/` directory and the architecture docs. Product specs are owned by their respective product teams.
