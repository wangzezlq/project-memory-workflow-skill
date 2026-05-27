# Project index

Use this file as the global dashboard for long-running projects.

## Project dashboard

| Project | Path | Stack | Status | Next step | Last observed |
| --- | --- | --- | --- | --- | --- |
| example-project | `/path/to/example-project` | stack summary | Paused | Restore context and choose next first action | YYYY-MM-DD |

## Update rule

When stopping work on any project:

1. Update that project's task ledger first.
2. Update one row here: `Status`, `Next step`, `Last observed`.

## Recovery flow

1. Read this file.
2. Open the selected project.
3. Read the project's stable context and task ledger.
4. Check git status/log/diff.
5. Continue from the task ledger's next first action.
