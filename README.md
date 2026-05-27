# Project Memory Workflow

A portable project-memory workflow for managing long-running software projects across AI agents, terminals, platforms, and machines.

This repository is **not tied to one agent runtime**. It provides one shared methodology plus optional adapters for different systems:

1. **Universal Markdown methodology** for project recovery, status tracking, and stop-work notes.
2. **Agent-neutral templates** for project indexes, context files, task ledgers, and prompts.
3. **Optional adapters** for Codex, Claude Code, OpenClaw, Antigravity, and generic `AGENTS.md`-style systems.
4. **Prompt-only workflows** for tools that do not support installable skills or adapters.

The workflow supports English and Chinese natural-language usage, including:

- "initialize project memory" / "帮我初始化项目"
- "recover this project" / "恢复一下项目"
- "current project status" / "说一下当前项目现状"
- "wrap up" / "收工"
- "list my projects" / "我有哪些项目"

## Why this exists

Long-running projects often span multiple sessions, machines, and AI agents. Without durable project memory, agents waste time rediscovering context or, worse, continue from stale assumptions.

This workflow makes project state explicit by combining:

- a global `PROJECTS.md` dashboard;
- per-project context files;
- per-project task ledgers;
- mandatory git truth checks (`status`, `log`, `diff`);
- stop-work notes with validation and next actions.

## Repository layout

```text
.
├── README.md
├── PROJECT_MEMORY_WORKFLOW.md              # full human-readable methodology
├── templates/                              # agent-neutral templates
│   ├── PROJECTS.md
│   ├── PROJECT_CONTEXT.md
│   └── TASKS.md
├── project-memory-workflow/                # optional Codex-compatible skill package
│   ├── SKILL.md
│   ├── agents/openai.yaml                  # Codex/OpenAI adapter metadata
│   ├── references/handbook.md
│   └── assets/templates/
└── adapters/                               # optional adapters for other runtimes
    ├── codex/
    ├── claude-code/
    ├── generic-agent/
    ├── openclaw/
    ├── antigravity/
    └── prompt-pack/
```

## Universal concepts

Use two layers of project memory:

1. **Global index**: `PROJECTS.md` lists projects, paths, status, next step, and last observed date.
2. **Per-project memory**:
   - stable context: `PROJECT_CONTEXT.md`, `CLAUDE.md`, or `AGENTS.md`;
   - dynamic task ledger: `.project/TASKS.md`, `.claude/TASKS.md`, or equivalent.

Before continuing work, every agent should verify git truth:

```bash
git status --short --branch
git log --oneline -5
git diff --stat
```

If uncommitted changes exist, inspect key diffs before making assumptions.

## Quick start: agent-neutral setup

Use this when you want a workflow that any agent can read.

```bash
cp templates/PROJECT_CONTEXT.md /path/to/project/PROJECT_CONTEXT.md
mkdir -p /path/to/project/.project
cp templates/TASKS.md /path/to/project/.project/TASKS.md
```

Optionally maintain a global dashboard:

```bash
mkdir -p ~/Projects
cp templates/PROJECTS.md ~/Projects/PROJECTS.md
```

Then ask any agent:

```text
Recover this project using PROJECT_CONTEXT.md and .project/TASKS.md. Check git status/log/diff before editing code.
```

Or in Chinese:

```text
恢复一下这个项目，读取 PROJECT_CONTEXT.md 和 .project/TASKS.md，先检查 git status/log/diff，不要先改代码。
```

## Adapter: Codex

Codex can optionally use the packaged skill folder directly:

```bash
mkdir -p ~/.codex/skills
cp -R project-memory-workflow ~/.codex/skills/
```

Restart Codex or start a new session.

Example:

```text
Use $project-memory-workflow to recover this project context. Do not edit code until I confirm.
```

See `adapters/codex/install.md` for details.

## Adapter: Claude Code

Claude Code commonly reads project-local `CLAUDE.md`.

For a new project:

```bash
cp adapters/claude-code/CLAUDE.md /path/to/project/CLAUDE.md
cp PROJECT_MEMORY_WORKFLOW.md /path/to/project/PROJECT_MEMORY_WORKFLOW.md
cp -R templates /path/to/project/.project-memory-templates
```

If the project already has `CLAUDE.md`, merge the adapter section instead of overwriting it.

## Adapter: generic AGENTS.md systems

For agents that read `AGENTS.md`:

```bash
cp adapters/generic-agent/AGENTS.md /path/to/project/AGENTS.md
cp PROJECT_MEMORY_WORKFLOW.md /path/to/project/PROJECT_MEMORY_WORKFLOW.md
cp -R templates /path/to/project/.project-memory-templates
```

If `AGENTS.md` already exists, merge the adapter into it.

## Adapter: OpenClaw

```bash
cp adapters/openclaw/OPENCLAW.md /path/to/project/OPENCLAW.md
cp PROJECT_MEMORY_WORKFLOW.md /path/to/project/PROJECT_MEMORY_WORKFLOW.md
cp -R templates /path/to/project/.project-memory-templates
```

If your OpenClaw environment uses a different project-instruction filename, copy the adapter content into that file.

## Adapter: Antigravity

```bash
cp adapters/antigravity/AGENTS.md /path/to/project/AGENTS.md
cp PROJECT_MEMORY_WORKFLOW.md /path/to/project/PROJECT_MEMORY_WORKFLOW.md
cp -R templates /path/to/project/.project-memory-templates
```

If Antigravity uses a different instruction file, copy the adapter content there.

## Prompt-only usage

If an agent has no installable skill or adapter mechanism, paste one of the prompts from `adapters/prompt-pack/`:

- `universal-bootstrap-prompt.md`
- `universal-recovery-prompt.md`
- `universal-stop-work-prompt.md`

## Examples

### Initialize / 初始化

```text
帮我初始化项目，用 project memory workflow 建立长期项目记忆。
```

```text
Initialize durable project memory for this repository.
```

### Recover / 恢复

```text
恢复一下项目，先不要改代码，告诉我当前状态和下一步。
```

```text
Recover this project context and do not edit code until I confirm.
```

### Status / 项目现状

```text
说一下当前项目现状，包括未提交 diff 和接下来 30 分钟最小可执行步骤。
```

```text
Summarize the current project status, dirty diff, and next executable step.
```

### Stop-work / 收工

```text
收工，记录一下当前进展，更新任务账本和项目看板。
```

```text
Wrap up and record stop-work notes for this project.
```

## License

No license is included yet. Add one before publishing if you want others to reuse, modify, or redistribute under explicit terms.
