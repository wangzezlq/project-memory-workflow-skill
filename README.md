# Project Memory Workflow Skill

A portable Codex skill and methodology for managing long-running software projects with durable Markdown project memory.

It helps agents and humans:

- initialize project memory for a new repository;
- recover project context before editing code;
- summarize current project status;
- record stop-work notes;
- maintain a global `PROJECTS.md` dashboard;
- keep workflows reusable across agents, platforms, and machines.

The skill supports English and Chinese natural-language usage, including:

- "initialize project memory" / "帮我初始化项目"
- "recover this project" / "恢复一下项目"
- "current project status" / "说一下当前项目现状"
- "wrap up" / "收工"
- "list my projects" / "我有哪些项目"

## Contents

```text
project-memory-workflow/
├── SKILL.md
├── agents/openai.yaml
├── references/handbook.md
└── assets/templates/
    ├── PROJECTS.md
    ├── PROJECT_CONTEXT.md
    ├── TASKS.md
    ├── recovery-prompt.md
    └── stop-work-prompt.md
```

The full human-readable methodology is in [`PROJECT_MEMORY_WORKFLOW.md`](PROJECT_MEMORY_WORKFLOW.md).

## Install

Copy the skill folder into your Codex skills directory:

```bash
mkdir -p ~/.codex/skills
cp -R project-memory-workflow ~/.codex/skills/
```

Or from a cloned repository:

```bash
git clone <this-repo-url>
mkdir -p ~/.codex/skills
cp -R project-memory-workflow ~/.codex/skills/
```

Restart Codex or start a new session so the skill can be discovered.

## Usage examples

```text
Use $project-memory-workflow to initialize project memory for this repo.
```

```text
用 $project-memory-workflow 恢复一下这个项目，先不要改代码。
```

```text
收工，记录一下当前进展。
```

## License

No license is included yet. Add one before publishing if you want others to reuse, modify, or redistribute under explicit terms.
