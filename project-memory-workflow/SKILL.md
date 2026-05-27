---
name: project-memory-workflow
description: Recover, bootstrap, track, and wrap up long-running software projects with durable Markdown project memory. Use when the user asks in English or Chinese to restore/recover project context, initialize/bootstrap a project, explain current project status, summarize progress, wrap up/stop work/收工, update PROJECTS.md, manage CLAUDE.md/AGENTS.md/PROJECT_CONTEXT.md or TASKS.md, inspect git status/log/diff before continuing, or make a project workflow reusable across agents, machines, or platforms. Chinese triggers include 初始化项目, 恢复项目, 恢复一下项目, 项目现状, 当前状态, 收工, 记录进展, 总结进度, 项目管理.
---

# Project Memory Workflow

Use this skill to manage long-running projects with agent-neutral Markdown files plus git truth checks.

## Natural-language triggers / 自然语言触发

Support both English and Chinese. Treat these as equivalent intents:

| Intent | English examples | Chinese examples | Action |
| --- | --- | --- | --- |
| Recover | "recover this project", "resume context", "what was I doing?" | "恢复项目", "恢复一下项目", "帮我恢复上下文", "我之前做到哪了" | Run the recovery workflow; do not edit code unless confirmed. |
| Status | "current project status", "summarize this project" | "说一下当前项目现状", "现在项目什么状态", "总结一下进度" | Read memory + git and summarize state, recent work, dirty diff, next step. |
| Bootstrap | "initialize project memory", "set up project tracking" | "帮我初始化项目", "给这个项目建管理文档", "建立项目记忆" | Create/update index, context file, and task ledger using templates. |
| Stop work | "wrap up", "record stop-work notes" | "收工", "记录一下当前进展", "保存一下项目状态" | Run stop-work workflow, update ledgers, and ask/confirm before commit/push unless user already requested it. |
| Manage portfolio | "list my projects", "project dashboard" | "我有哪些项目", "项目列表", "项目看板" | Read/update global `PROJECTS.md`. |

When the user's wording is casual but maps clearly to one of these intents, proceed with the matching workflow. Ask only when the project path or target project cannot be inferred.

## Core model

Maintain two layers:

1. **Global project index**: `PROJECTS.md` in a user-chosen projects home, often `~/Projects/PROJECTS.md` or `$PROJECTS_HOME/PROJECTS.md`.
2. **Per-project memory**:
   - stable context: prefer existing `CLAUDE.md` / `AGENTS.md`; otherwise create `PROJECT_CONTEXT.md`.
   - dynamic task ledger: prefer existing `.claude/TASKS.md` / `.project/TASKS.md`; otherwise create `.project/TASKS.md`.

Use the existing convention if present; do not rename or duplicate files unless the user asks.

## Recovery workflow

When asked to recover or resume a project:

1. Locate the project root from the user, `PROJECTS.md`, or the current directory.
2. Read the global index if available.
3. Read stable context files in this order when present: `PROJECT_CONTEXT.md`, `CLAUDE.md`, `AGENTS.md`, `GEMINI.md`, `README.md`.
4. Read task ledgers in this order when present: `.project/TASKS.md`, `.claude/TASKS.md`, `TASKS.md`.
5. Always inspect git truth from the project root:
   - `git status --short --branch`
   - `git log --oneline -5`
   - `git diff --stat`
   - key uncommitted diff content when `git diff --stat` is non-empty.
6. Do not edit code during recovery unless the user explicitly asks.
7. Summarize: current state, recently completed work, uncommitted changes, prioritized unfinished work, and the next 30-minute executable step.

## Bootstrap workflow

When starting a new project memory system:

1. Choose file names that fit the environment:
   - cross-agent default: `PROJECT_CONTEXT.md` and `.project/TASKS.md`.
   - if an agent convention already exists, use it.
2. Create/update a global `PROJECTS.md` row.
3. Create a stable context file from `assets/templates/PROJECT_CONTEXT.md`.
4. Create a task ledger from `assets/templates/TASKS.md`.
5. Add a recovery prompt from `assets/templates/recovery-prompt.md` to the task ledger if useful.
6. Record the initial git branch, status, validation commands, blockers, and next first action.

## Stop-work workflow

When asked to wrap up, stop work, or record progress:

1. Inspect git truth:
   - `git status --short --branch`
   - `git log --oneline -3`
   - `git diff --stat`
2. Update the task ledger:
   - `Status`
   - `Last completed`
   - `Next first action`
   - `Blockers`
   - `Last updated`
   - a `Stop-work note YYYY-MM-DD` with commits, tests, sample IDs, decisions, and pending push state.
3. Update the global `PROJECTS.md` row with status, next step, and last observed date.
4. If only records changed, commit separately with a message like `Update <topic> wrap-up notes` when the user confirms or has asked to commit.
5. Push only when the user asks or the workflow explicitly requires sharing.
6. Final output must include sync state, latest commits, tests run, and next first action.

## Content rules

- Treat git as the source of truth over memory files.
- Keep `Next first action` concrete enough to begin within 5–30 minutes.
- Record negative findings that prevent future repeated mistakes.
- Record high-value external facts: ticket IDs, endpoint IDs, sample IDs, regions, control-plane parameters, response shapes, validation commands.
- Prefer small commits: functional changes, wrap-up notes, and document cleanup should usually be separate.
- Keep paths portable in reusable templates; use `$HOME`, `$PROJECTS_HOME`, and project-relative paths unless documenting a real local project.

## References and templates

- For the full methodology, read `references/handbook.md`.
- Use templates in `assets/templates/` when creating files:
  - `PROJECTS.md`
  - `PROJECT_CONTEXT.md`
  - `TASKS.md`
  - `recovery-prompt.md`
  - `stop-work-prompt.md`
