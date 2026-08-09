# platform-workflows

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/platform-workflows](https://github.com/b-platform-vn/platform-workflows) |
| **v3 target** | keep (DevOps-owned infra, outside layer convention) |
| **Layer** | Platform infrastructure (DevOps-owned) |
| **Status** | active |
| **Language** | none (YAML workflows only) |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Reusable GitHub Actions workflows for the `b-platform-vn` organization.

## Responsibility

- `docker_container.yml` — reusable workflow that builds and pushes Docker images to GitHub Container Registry (`ghcr.io`).

## Dependencies

- **Consumers**: every B-Platform repo that builds a Docker image (the `api-*`, `web-*`, `app-mcm` services).

## Public API surface

- Reusable workflow: `b-platform-vn/platform-workflows/.github/workflows/docker_container.yml@main`.
- Inputs: `package_name`, `package_version`.
- Caller job must grant `contents: read`, `packages: write`, `attestations: write`, `id-token: write`.

## Manifest contract

- `.github/` + `README.md` only.

## Owner

- **`[B-Platform] DevOps`**. Architecture: B-Platform architect owns this doc only.
