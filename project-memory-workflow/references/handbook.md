# Project Memory Workflow Handbook

## Purpose

Use plain Markdown plus git checks to make long-running projects recoverable across sessions, agents, platforms, and machines.

## Two-layer memory

### Global project index

A single `PROJECTS.md` acts as a dashboard and router. It should contain only high-signal fields:

```markdown
| Project | Path | Stack | Status | Next step | Last observed |
| --- | --- | --- | --- | --- | --- |
| my-service | `/path/to/my-service` | Go, Postgres | Paused | Validate staging smoke test | 2026-05-27 |
```

### Per-project memory

Use two files per project:

1. Stable project context: `PROJECT_CONTEXT.md`, `CLAUDE.md`, or `AGENTS.md`.
2. Dynamic task ledger: `.project/TASKS.md` or `.claude/TASKS.md`.

Stable context should change slowly. Task ledger can change every session.

## Recommended task ledger sections

```markdown
# Project progress

## Snapshot

- Status:
- Current objective:
- Last completed:
- Next first action:
- Blockers:
- Last updated:

## Current state

- Branch:
- Latest commits:
- Remote sync:
- Latest validation:
- Known dirty worktree:

## Implemented so far

- ...

## Active tasks

- [x] ...
- [ ] ...

## Decisions

- ...

## Validation log

### YYYY-MM-DD

- Command:
- Result:

## Stop-work note YYYY-MM-DD

- Completed:
- Committed:
- Pushed:
- Tests:
- Next:
```

## Recovery output contract

After reading files and git state, answer with:

1. Current project state.
2. What was completed recently.
3. Current uncommitted changes.
4. Unfinished work sorted by priority.
5. Next 30-minute executable step.

Do not edit code during recovery unless requested.

## Stop-work output contract

After updating notes, answer with:

1. Current status.
2. Latest commits and whether pushed.
3. Tests/validation run.
4. Remaining blockers.
5. Next first action.

## Bilingual natural-language usage

The workflow should respond to casual English or Chinese phrases, not only exact command names.

### Chinese examples

- "帮我初始化项目" → bootstrap project memory.
- "恢复一下项目" / "恢复项目上下文" → recovery workflow.
- "说一下当前项目现状" / "现在项目什么状态" → status summary workflow.
- "收工" / "记录一下当前进展" → stop-work workflow.
- "我有哪些项目" / "项目看板" → read or update global `PROJECTS.md`.

### English examples

- "Initialize project memory" → bootstrap.
- "Recover this project" → recovery.
- "What's the current project status?" → status summary.
- "Wrap up and record progress" → stop-work.
- "List my projects" → global project index.

If a phrase clearly maps to an intent, execute the workflow. If the project path is missing and cannot be inferred from the current directory or global index, ask one concise question.

## Portability guidance

- Prefer repository-relative task ledgers (`.project/TASKS.md`) so state moves with git.
- Keep global `PROJECTS.md` in a synced folder or dotfiles repo if multiple machines are used.
- Use agent-specific files as adapters, not as the only source of truth.
- Avoid proprietary tool assumptions in templates; record project-specific commands only inside real project files.
- Always verify with git after reading memory files.
