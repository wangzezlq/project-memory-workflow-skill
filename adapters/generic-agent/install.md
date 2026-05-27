# Install for generic AGENTS.md systems

Many agents read an `AGENTS.md` file in the repository root.

```bash
cp adapters/generic-agent/AGENTS.md /path/to/project/AGENTS.md
cp PROJECT_MEMORY_WORKFLOW.md /path/to/project/PROJECT_MEMORY_WORKFLOW.md
cp -R templates /path/to/project/.project-memory-templates
```

If `AGENTS.md` already exists, merge the adapter into it instead of overwriting.
