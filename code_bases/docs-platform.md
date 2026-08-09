# docs-platform

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/docs-platform` (planned) |
| **Current name** | `platform-ecosystem-docs` → [old doc](./deprecated/platform-ecosystem-docs.md) |
| **v3 target** | `docs-platform` (L0) — rename + absorb cross-cutting docs |
| **Layer** | L0 — Documentation only (shared) |
| **Status** | planned (rename of `platform-ecosystem-docs`) |
| **Language** | HTML (Docsify) + Markdown |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

This living documentation site (Docsify-powered) plus all cross-cutting shared documentation. Absorbs the role of the former `platform-ecosystem-docs` repo — product specs (`products/`), code-base inventory (`code_bases/`), architecture docs, and shared conventions/ADRs. This very file lives in this repo.

## Responsibility

- Product specs under `products/<product>/` (MDFoods, LFarm, Odeli, UniGate, B-Platform General, CRM).
- Architecture docs under `products/*/architecture/`.
- Code-base mirror under `code_bases/<repo>.md` (this directory).
- Cross-cutting shared documentation, conventions, and ADRs not owned by a domain docs repo.
- Docsify config: `_coverpage.md`, `_navbar.md`, `_sidebar.md`, `index.html`, `CNAME`.

## Dependencies

- **Runtime**: Docsify (served from `index.html`).
- **Build**: `package.json` (local dev scripts).

## Public API surface

- Public website (CNAME-configured) + Markdown documentation.

## Manifest contract

- Docsify layout: `_coverpage.md` + `_navbar.md` + `_sidebar.md` + `index.html` + `CNAME` + `README.md` + `package.json`.

## Notes

- ⚠️ Current remote repo name is `platform-ecosystem-docs` — rename to `docs-platform` under the v3 convention. The repo already exists and hosts this site.

## Owner

- **B-Platform architect** owns this repo's `code_bases/` directory and architecture docs. Product specs are owned by their respective product teams.
