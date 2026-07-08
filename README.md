# Agent Triad Loop

Languages: [English](#english) | [简体中文](#简体中文)

## English

`agent-triad-loop` is a portable agent skill for running a long-lived development loop with three explicit roles:

- **Planner** turns a compact user goal into scope, artifacts, and acceptance criteria.
- **Generator** implements one bounded sprint at a time without changing the agreed criteria.
- **Evaluator** verifies the running product like a real user, records evidence, and either fails the sprint with actionable critique or marks criteria as passed.

The protocol is designed to work across multiple agent runtimes. It supports separate Planner / Generator / Evaluator agents, a coordinator-driven multi-agent setup, or a single agent that rotates roles when subagents are unavailable.

### Contents

- `skill/agent-triad-loop/SKILL.md` - the installable skill instructions.
- `skill/agent-triad-loop/references/long-running-harness-notes.md` - background notes summarizing long-running harness patterns and tradeoffs.

### Compatibility

The skill uses file-based handoff instead of chat-memory handoff, so it can run in:

- Codex or other systems with real subagents.
- Cursor or other single-agent environments through role rotation.
- CLI agent workflows where each role is a separate process.
- Hybrid flows where one coordinator assigns work to external agents and verifies the resulting artifacts.

### Installation

Copy or symlink `skill/agent-triad-loop` into your agent's skill directory:

```bash
cp -R skill/agent-triad-loop ~/.codex/skills/
cp -R skill/agent-triad-loop ~/.cursor/skills/
```

### Usage

Start a new agent session and explicitly ask it to use the skill:

```text
Use agent-triad-loop for this project.
Start as Planner, create docs/agent-triad-loop/plan.md, feature_list.json, and the first sprint contract.
Then run the Generator -> Evaluator loop until the agreed acceptance criteria pass or the iteration cap is reached.
```

For environments with separate agents, assign Planner, Generator, and Evaluator to separate contexts. For single-agent environments, ask the agent to rotate roles and write each role's output to disk before switching.

### Core Artifact Contract

Use ordinary project files as the shared interface between agents:

- `docs/agent-triad-loop/plan.md`
- `docs/agent-triad-loop/feature_list.json`
- `docs/agent-triad-loop/contracts/<sprint>.md`
- `docs/agent-triad-loop/evaluations/<sprint>-round-<n>.md`
- `docs/agent-triad-loop/progress.md`

Any runtime can implement the loop as long as roles honor those files and do not weaken acceptance criteria without evidence.

## 简体中文

`agent-triad-loop` 是一个可移植的 agent skill，用于运行长时迭代开发循环。它把开发流程拆成三个明确角色：

- **Planner（规划者）**：把简短目标转成范围、产物、验收标准和冲刺顺序。
- **Generator（构建者）**：一次实现一个有边界的 sprint，不修改已经约定的验收标准。
- **Evaluator（评估者）**：像真实用户一样验证运行中的产物，记录证据；不通过就给出可执行 critique，通过才标记验收项。

这个协议面向多种 agent 运行环境：既支持 Planner / Generator / Evaluator 三个独立 agent，也支持 coordinator 调度的多 agent 流程；如果环境不能启动子 agent，也可以由单个 agent 串行切换角色。

### 内容

- `skill/agent-triad-loop/SKILL.md`：可安装的 skill 指令。
- `skill/agent-triad-loop/references/long-running-harness-notes.md`：长时 harness 模式、权衡和评估方法的背景笔记。

### 兼容性

这个 skill 使用文件交接，而不是依赖聊天上下文记忆，因此可以运行在：

- Codex 或其他支持真实子 agent 的系统。
- Cursor 或其他单 agent 环境，通过角色轮换执行。
- CLI agent 工作流，其中每个角色都是独立进程。
- 混合模式：一个 coordinator 分配任务给外部 agent，并最终验证结果。

### 安装

把 `skill/agent-triad-loop` 复制或软链接到你的 agent skill 目录：

```bash
cp -R skill/agent-triad-loop ~/.codex/skills/
cp -R skill/agent-triad-loop ~/.cursor/skills/
```

### 启动方式

开启一个新的 agent 会话，并明确要求它使用这个 skill：

```text
使用 agent-triad-loop 来开发这个项目。
先以 Planner 身份创建 docs/agent-triad-loop/plan.md、feature_list.json 和第一个 sprint contract。
然后按 Generator -> Evaluator 循环迭代，直到约定的验收标准通过，或达到迭代上限。
```

如果环境支持多个 agent，就把 Planner、Generator、Evaluator 分配到独立上下文。如果环境只有单 agent，就要求它进行角色轮换，并在每次切换角色前把当前角色产物写入文件。

### 核心交接文件

使用普通项目文件作为 agent 之间的共享接口：

- `docs/agent-triad-loop/plan.md`
- `docs/agent-triad-loop/feature_list.json`
- `docs/agent-triad-loop/contracts/<sprint>.md`
- `docs/agent-triad-loop/evaluations/<sprint>-round-<n>.md`
- `docs/agent-triad-loop/progress.md`

只要各个角色遵守这些文件约定，并且不在缺少证据的情况下削弱验收标准，任何 agent 运行时都可以实现这个循环。
