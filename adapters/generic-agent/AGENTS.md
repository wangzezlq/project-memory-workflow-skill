# Project Memory Workflow for Generic Agents

Follow this workflow when asked to manage long-running project memory, including Chinese phrases such as "初始化项目", "恢复一下项目", "说一下当前项目现状", or "收工".

## Files

- Global index: `PROJECTS.md`.
- Stable project context: `PROJECT_CONTEXT.md` preferred; `CLAUDE.md` / `AGENTS.md` if already present.
- Task ledger: `.project/TASKS.md` preferred; existing `.claude/TASKS.md` accepted.

## Mandatory git truth check

Before continuing a recovered project, run from project root:

```bash
git status --short --branch
git log --oneline -5
git diff --stat
```

Inspect key uncommitted diffs when present.

## Output for recovery/status

Return:

1. Current project state.
2. Recently completed work.
3. Current uncommitted changes.
4. Unfinished work by priority.
5. Next 30-minute executable step.

## Stop-work

Update task ledger and global index, record tests and commit/push state, then report the next first action.
