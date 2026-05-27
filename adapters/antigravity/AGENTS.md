# Project Memory Workflow for Antigravity

Use this adapter when Antigravity is asked to initialize project tracking, restore context, summarize status, or record stop-work notes.

Chinese triggers include: 初始化项目, 恢复项目, 恢复一下项目, 当前状态, 项目现状, 收工, 记录进展.

## Required behavior

1. Read project memory files if present.
2. Verify git truth before editing:
   - `git status --short --branch`
   - `git log --oneline -5`
   - `git diff --stat`
3. Use `PROJECT_CONTEXT.md` and `.project/TASKS.md` for new projects unless existing project conventions differ.
4. Keep next actions concrete and executable within 5-30 minutes.
5. During stop-work, update task ledger and global index, then report commit/push state.

See `PROJECT_MEMORY_WORKFLOW.md` for the complete methodology.
