# Project Memory Workflow for OpenClaw

Use this workflow for durable project memory across OpenClaw sessions.

Trigger on English or Chinese requests such as:

- recover context / 恢复一下项目
- initialize project memory / 帮我初始化项目
- current project status / 说一下当前项目现状
- wrap up / 收工

Follow the canonical workflow in `PROJECT_MEMORY_WORKFLOW.md` and use templates from `templates/`.

Always verify git truth before continuing work:

```bash
git status --short --branch
git log --oneline -5
git diff --stat
```

Prefer `.project/TASKS.md` and `PROJECT_CONTEXT.md` for new projects; respect existing `CLAUDE.md`, `AGENTS.md`, or `.claude/TASKS.md` when present.
