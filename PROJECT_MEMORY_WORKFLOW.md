# Project Memory Workflow：长期项目上下文管理方法论

> 目标：让一个长期开发项目在多次 session、不同 AI agent、不同电脑之间，都能快速恢复上下文、确认真实状态、稳定推进、可靠收工。

## 1. 核心原则

1. **Git 是事实源**：项目记录可能过期，恢复上下文时必须检查 `git status`、`git log`、`git diff`。
2. **全局索引只做导航**：跨项目看板只记录项目路径、状态、下一步，不承载细节。
3. **项目内记录承载细节**：每个项目维护稳定上下文和动态任务账本。
4. **下一步必须可执行**：`Next first action` 应该是 5–30 分钟内能开始的具体动作。
5. **收工要形成闭环**：更新项目内任务账本、更新全局索引、验证测试状态、提交/推送或明确未提交原因。
6. **用 Markdown 保持跨 agent 可读**：所有关键信息都放在普通 Markdown 文件中，避免绑定单一 agent。

## 2. 推荐文件结构

### 2.1 全局项目索引

推荐位置：

```text
$PROJECTS_HOME/PROJECTS.md
~/Projects/PROJECTS.md
```

用途：跨项目导航和状态总览。

建议表格：

```markdown
| Project | Path | Stack | Status | Next step | Last observed |
| --- | --- | --- | --- | --- | --- |
| my-service | `/path/to/my-service` | Go, MySQL | Paused | Push local commit, then validate staging API | 2026-05-27 |
```

### 2.2 单项目稳定上下文

推荐 agent-neutral 文件：

```text
PROJECT_CONTEXT.md
```

如果项目已有 agent 约定，也可以沿用：

```text
CLAUDE.md
AGENTS.md
GEMINI.md
```

用途：项目说明书，放稳定信息：

- 项目是什么；
- 技术栈；
- 常用命令；
- 目录结构；
- 恢复流程；
- 服务入口和主要模块。

### 2.3 单项目动态任务账本

推荐 agent-neutral 文件：

```text
.project/TASKS.md
```

如果项目已有历史约定，也可以沿用：

```text
.claude/TASKS.md
```

用途：动态状态、任务列表、验证记录、收工记录、关键调查结论。

## 3. 标准恢复流程

新 session 开始时，先不要改代码。按顺序读取/检查：

```text
1. 全局 PROJECTS.md
2. 项目上下文文件：PROJECT_CONTEXT.md / CLAUDE.md / AGENTS.md
3. 项目任务账本：.project/TASKS.md / .claude/TASKS.md
4. git status --short --branch
5. git log --oneline -5
6. git diff --stat
7. 当前未提交 diff 的关键内容
```

然后输出：

```text
1) 当前项目状态
2) 最近完成了什么
3) 当前未提交改动是什么
4) 未完成事项，按优先级排序
5) 接下来 30 分钟最小可执行步骤
```

## 4. 标准工作流程

### 4.1 开工

1. 恢复上下文。
2. 确认下一步。
3. 如果状态从 `Paused` 继续，必要时把任务账本改为 `In Progress`。
4. 小步执行，小步验证。

### 4.2 开发中

优先保持每个变更可以解释为一个小目标：

- 修一个 bug；
- 接一个接口；
- 更新一组字段映射；
- 补一个测试；
- 清理一段文档。

关键外部事实要写入任务账本，例如：

- 权限工单 ID；
- API endpoint ID；
- 样本 ID；
- 真实响应 shape；
- 失败接口和错误信息；
- 控制面/区域/环境参数；
- 验证命令和结果。

### 4.3 收工

收工时执行：

```text
1. git status --short --branch
2. git log --oneline -3
3. git diff --stat
4. 更新项目任务账本 Snapshot / Current state / Stop-work note
5. 更新全局 PROJECTS.md 对应项目行
6. 如有代码或记录改动，commit
7. 需要共享时 push
8. 输出最终状态和下次第一步
```

## 5. 状态字段约定

常用 `Status`：

| Status | 含义 |
| --- | --- |
| In Progress | 当前正在推进 |
| Paused | 已收工，可恢复 |
| Blocked | 被外部条件阻塞 |
| Done | 当前目标已完成 |
| Parking | 暂无明确任务，只占位 |
| Researching | 正在调研，尚未落地 |

## 6. Commit 策略

推荐拆成三类：

### 功能 commit

```text
Switch service client to new history endpoint
```

包含代码、测试和必要文档。

### 收工记录 commit

```text
Update service wrap-up notes
```

只更新任务账本和项目索引等记录。

### 文档清理 commit

```text
Clean up service demo plan
```

整理过期文档，不改变逻辑。

## 7. 推荐模板

### 7.1 恢复 prompt

```text
请恢复 <项目名> 项目上下文。

项目路径：<path>

请依次读取/检查：
1. <global-projects-md>
2. <project-context-md>
3. <project-tasks-md>
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
```

### 7.2 收工 prompt

```text
收工，记录当前进展。

请：
1. 检查 git status、git log -3、git diff --stat
2. 更新项目任务账本的 Snapshot、Current state、Stop-work note
3. 更新全局 PROJECTS.md 对应项目行
4. 如果只有收工记录改动，单独 commit，message 用 "Update <topic> wrap-up notes"
5. 最后输出当前状态、最新 commit、是否已 push、下次第一步
```

## 8. 跨 agent / 跨平台 / 跨机器实践

1. **文件名可适配，但语义要固定**：没有 `.claude/TASKS.md` 时用 `.project/TASKS.md`；没有 `CLAUDE.md` 时用 `PROJECT_CONTEXT.md`。
2. **路径不要写死**：文档中使用 `$PROJECTS_HOME`、`$HOME`、相对路径；只在具体项目记录中写绝对路径。
3. **关键状态落在 repo 内**：项目任务账本应该尽量在项目 repo 中，这样换机器后随 git 同步。
4. **全局索引可单独同步**：`PROJECTS.md` 可放在 `~/Projects`、dotfiles repo、或团队共享 repo。
5. **agent-specific 文件可以作为适配层**：Claude 用 `CLAUDE.md`，Codex/GPT 可读 `PROJECT_CONTEXT.md`，但动态状态保持一份 canonical task ledger。
6. **恢复时永远检查 git diff**：这是跨 session 成功率最高的防线。

## 9. 最小落地清单

新项目至少创建：

```text
PROJECT_CONTEXT.md
.project/TASKS.md
```

全局至少维护：

```text
PROJECTS.md
```

每次开工：恢复上下文。

每次收工：更新记录、检查 git、必要时 commit/push。


## 10. 中英文自然语言用法

这套 workflow 不要求用户说固定命令，应该兼容自然语言。

| 意图 | 中文说法 | English examples | 行为 |
| --- | --- | --- | --- |
| 初始化 | "帮我初始化项目"、"给这个项目建管理文档"、"建立项目记忆" | "initialize project memory", "set up project tracking" | 创建/更新 `PROJECTS.md`、项目上下文文件、任务账本 |
| 恢复 | "恢复一下项目"、"恢复项目上下文"、"我之前做到哪了" | "recover this project", "resume context" | 读取项目记忆并检查 git truth |
| 看状态 | "说一下当前项目现状"、"现在项目什么状态"、"总结一下进度" | "current project status", "summarize progress" | 输出状态、最近完成、未提交 diff、下一步 |
| 收工 | "收工"、"记录一下当前进展"、"保存一下项目状态" | "wrap up", "record stop-work notes" | 更新任务账本和全局索引，必要时提交/推送 |
| 项目看板 | "我有哪些项目"、"项目列表"、"项目看板" | "list my projects", "project dashboard" | 读取/维护全局 `PROJECTS.md` |

原则：如果用户说法能明确映射到意图，就直接执行对应 workflow；只有在项目路径或目标项目无法判断时，才问一个简短问题。
