# Codex adapter

Codex can use the native skill folder directly.

## Install

From this repository root:

```bash
mkdir -p ~/.codex/skills
cp -R project-memory-workflow ~/.codex/skills/
```

Restart Codex or start a new session.

## Example prompts

```text
Use $project-memory-workflow to recover this project context. Do not edit code until I confirm.
```

```text
用 $project-memory-workflow 帮我初始化项目。
```

```text
收工，记录一下当前进展。
```
