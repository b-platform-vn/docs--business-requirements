# platform-fluxcd

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/platform-fluxcd](https://github.com/b-platform-vn/platform-fluxcd) |
| **v3 target** | keep (DevOps-owned infra, outside layer convention) |
| **Layer** | Platform infrastructure (DevOps-owned) |
| **Status** | active |
| **Language** | Shell + Python + Go Template |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

FluxCD GitOps repository for the B-Platform Kubernetes clusters. Owns cluster state for `k8s-dpsrv`, `k8s-dpsrv-prd`, and `k8s-local`.

## Responsibility

- FluxCD source/kustomization/helm-release manifests for all B-Platform deployments.
- Cluster secrets, image policies, and Helm charts for the deployed services.

## Dependencies

- **Tooling**: `uv` (Python via `pyproject.toml` + `uv.lock`), `scripts/` (Python helpers), `charts/` (Helm), `clusters/`, `docs/`.
- **Clusters**: `k8s-dpsrv` (staging?), `k8s-dpsrv-prd` (production), `k8s-local` (local).

## Public API surface

- None — infrastructure config only.

## Manifest contract

- `.agents/` (agent definitions for this repo), `.python-version`, `pyproject.toml`, `uv.lock`.

## Owner

- **`[B-Platform] DevOps`** — this repo is out of the architect's write scope. Architecture: B-Platform architect owns this doc only (mirrors DevOps state, never edits it).
