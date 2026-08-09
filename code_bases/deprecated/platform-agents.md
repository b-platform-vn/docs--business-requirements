# platform-agents

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/platform-agents](https://github.com/b-platform-vn/platform-agents) |
| **v3 target** | retire — Copilot skills tooling removed from code_bases topology |
| **Layer** | Platform tooling (Copilot skills) |
| **Status** | active |
| **Language** | Python + Shell |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Collection of GitHub Copilot skills for repeatable B-Platform workflows. Includes the `multica` skill and an Agno-backed evaluation harness.

## Responsibility

- Ship reusable skill definitions (`skills/`).
- Evaluate skill quality via `scripts/evaluate_skills.py` (frontmatter, discovery, required sections, reference coverage, CLI guidance, safety rules, prompt/tool-call coverage).
- Optional Agno agent evaluation (`SKILL_AGENT_EVAL=1`) against a Copilot/OpenAI endpoint.

## Dependencies

- **Runtime**: Python (`requirements.txt`), `.venv/bin/python` per user preferences.
- **Skills shipped**: `multica` (Multica task collaboration CLI).

## Public API surface

- Skill folders under `skills/` (consumed by copying into `~/.vscode/skills/`).

## Manifest contract

- `skills/`, `evals/`, `scripts/`, `setup.sh`, `requirements.txt`, `.env.example`, `README.md`.

## Notes

- Install: `./setup.sh ~/.vscode`.
- Eval reports written to `tmp/skill_eval_report.json` and `tmp/skill_agent_eval_report.json`.

## Owner

- Engineering (developer productivity). Architecture: B-Platform architect owns this doc only.
