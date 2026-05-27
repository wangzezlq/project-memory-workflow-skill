# Project Memory Workflow for Claude Code

Use this adapter when the user asks in English or Chinese to initialize project memory, recover context, summarize current status, list projects, or wrap up/收工.

Canonical workflow files in this package:

- `PROJECT_MEMORY_WORKFLOW.md`
- `templates/PROJECTS.md`
- `templates/PROJECT_CONTEXT.md`
- `templates/TASKS.md`
- `adapters/prompt-pack/`

## Intent routing

- "恢复项目" / "恢复一下项目" / "recover this project" → run Recovery.
- "说一下当前项目现状" / "current project status" → run Status Summary.
- "帮我初始化项目" / "initialize project memory" → run Bootstrap.
- "收工" / "record stop-work notes" → run Stop-work.
- "我有哪些项目" / "list my projects" → read or update global `PROJECTS.md`.

## Recovery

Before editing code, read available project memory and verify git truth:

```bash
git status --short --branch
git log --oneline -5
git diff --stat
```

If `git diff --stat` is non-empty, inspect the key diff content. Summarize current state, recent work, dirty changes, unfinished work by priority, and the next 30-minute executable step.

## Bootstrap

Prefer agent-neutral files unless this project already uses a convention:

- stable context: `PROJECT_CONTEXT.md`, or existing `CLAUDE.md` / `AGENTS.md`.
- task ledger: `.project/TASKS.md`, or existing `.claude/TASKS.md`.

Use templates from `templates/`.

## Stop-work

1. Check `git status --short --branch`, `git log --oneline -3`, and `git diff --stat`.
2. Update the project task ledger Snapshot and add a stop-work note.
3. Update global `PROJECTS.md` if configured.
4. Commit/push only when requested or confirmed.
