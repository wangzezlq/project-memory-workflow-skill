请恢复 <project-name> 项目上下文。

项目路径：<project-path>

请依次读取/检查：
1. <global PROJECTS.md>
2. <project context file>
3. <project task ledger>
4. git status --short --branch
5. git log --oneline -5
6. git diff --stat
7. 当前未提交 diff 的关键内容

然后只输出：
1) 当前项目状态
2) 最近完成了什么
3) 当前未提交改动是什么
4) 未完成事项，按优先级排序
5) 接下来 30 分钟最小可执行步骤

先不要改代码，等我确认。


English equivalent:

```text
Recover <project-name> project context. Read the global project index, project context file, task ledger, git status/log/diff, then summarize current status, recent completion, dirty diff, prioritized unfinished work, and the next 30-minute executable step. Do not edit code until confirmed.
```
