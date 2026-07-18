---
description: "Use when working with Multica documentation, Multica CLI workflows, agents, daemon runtimes, issues, tasks, providers, skills, squads, autopilots, or troubleshooting Multica agent execution."
name: "Multica Instructions"
---

# Multica Instructions

Use these instructions when answering questions, writing docs, or preparing operational steps related to Multica.

## Product model

- Multica is a task collaboration platform where humans and AI agents work together in the same workspace.
- Issues are the core unit of work. An issue can be assigned to a person, agent, or squad.
- Agents are first-class workspace members: they can be assigned issues, be mentioned in comments, post comments, create issues, and lead projects.
- Agents differ from humans because they start work automatically when triggered and do not receive inbox notifications.

## Runtime architecture

- Do not describe agents as running on Multica servers unless cloud runtimes are explicitly in scope.
- Current default model: agents run locally through the Multica daemon on the user's machine.
- The Multica server coordinates workspaces, issues, comments, tasks, agent definitions, and realtime updates; it does not execute agent work.
- The daemon is part of the Multica CLI. It detects local AI coding tools, registers runtimes, polls for tasks every 3 seconds, and sends heartbeats every 15 seconds.
- A runtime means `daemon × one AI coding tool`. One daemon can register multiple runtimes across tools and workspaces.
- User code directories, toolchain, API keys, and local credentials stay on the user's machine.
- Cloud runtimes are planned / waitlist-only unless newer docs say otherwise.

## Agent triggers

- Main trigger paths:
	- Assign an issue to an agent or squad.
	- Mention an agent in an issue comment.
	- Chat with an agent directly.
	- Run an autopilot manually or on schedule.
- Assigning a non-Backlog issue to an agent enqueues a task immediately.
- Backlog issues do not trigger the agent until moved to Todo or In Progress.
- Reassigning or unassigning cancels active queued, dispatched, or running tasks for that issue.

## Task lifecycle

- Typical lifecycle: `queued` → `dispatched` → `running` → `completed` or `failed`.
- When assigned, the daemon picks up the task on the next poll, creates an isolated local working directory, invokes the selected AI coding tool, and reports progress / results back to Multica.
- Infrastructure failures such as offline runtimes or timeouts may be auto-retried for retryable sources such as issue assignment, mentions, and chat. Business/model errors are not automatically retried.

## CLI guidance

- Prefer official CLI commands when documenting operations:
	- `multica login` for authentication.
	- `multica auth status` to verify login and workspace.
	- `multica daemon start`, `stop`, `restart`, `status`, and `logs` for daemon operations.
	- `multica issue list`, `get`, `create`, `update`, `assign`, `status`, `runs`, `run-messages`, `rerun`, and `cancel-task` for issue workflows.
	- `multica agent list`, `get`, `create`, `update`, `archive`, `restore`, `tasks`, `env`, and `skills` for agent management.
	- `multica runtime list`, `usage`, `activity`, `update`, `delete`, and `profile` for runtime management.
- For exact flags, instruct users to run `multica <command> --help`.
- Avoid exposing or asking for secrets. Personal access tokens are prefixed with `mul_` and should be entered by the user directly when needed.

## Providers and skills

- Multica supports multiple local AI coding tools with provider-specific capabilities.
- Check provider compatibility before claiming support for MCP config, session resumption, model selection, or skill discovery.
- Built-in providers include Antigravity, Claude Code, CodeBuddy, Codex, Copilot, Cursor, Hermes, Kimi, Kiro CLI, OpenCode, DevEco Code, OpenClaw, Pi, Qoder, Trae, and Grok.
- MCP configuration is provider-specific; it has no effect for providers that ignore it.
- Skill file paths are provider-specific. Examples:
	- Copilot: `.github/skills/`
	- Claude Code: `.claude/skills/`
	- Codex: `$CODEX_HOME/skills/`
	- Cursor: `.cursor/skills/`
	- OpenCode: `.opencode/skills/`
	- Antigravity: `.agents/skills/`

## Troubleshooting checklist

When an agent is not working, check local runtime health before assuming server failure:

1. Verify the daemon is running with `multica daemon status`.
2. Inspect daemon output with `multica daemon logs`.
3. Confirm the relevant runtime is online in Multica or with runtime CLI commands.
4. Confirm the underlying AI coding tool is installed, on `PATH`, authenticated, and usable in the same environment as the daemon.
5. Check concurrency limits if tasks stay queued.

## Writing style

- Be precise about server vs daemon responsibilities.
- Prefer short, operational explanations with concrete status names and command names.
- Link users back to the relevant Multica docs page when details may change.
