# Install for Claude Code

Claude Code commonly reads project-local `CLAUDE.md`. Use one of these patterns.

## Per-project install

Copy this adapter into your project root:

```bash
cp adapters/claude-code/CLAUDE.md /path/to/project/CLAUDE.md
cp -R templates /path/to/project/.project-memory-templates
```

If the project already has `CLAUDE.md`, merge the adapter section instead of overwriting it.

## Prompt-only use

Paste one of the prompts from `adapters/prompt-pack/` into Claude Code and point it at this repository or copied templates.
