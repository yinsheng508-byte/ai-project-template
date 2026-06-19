# AI 项目模板仓库

面向单人 / 小团队长期迭代项目的 AI 协作开发模板。
核心目标：让 Codex 每次开工都能"看见全局"，而不是在黑暗中摸索。

## 设计重心

**架构记忆注入 > 流程治理**

AI 最大的问题不是不勤奋，而是每次启动都失忆。本模板的核心是让 AI 始终处在正确的事实、边界、验收、生产语义里。

## 核心机制

| 机制 | 文件 | 作用 |
|------|------|------|
| 唯一入口 | `AGENTS.md` | 所有规则在此，所有 agent 开工前必读 |
| 当前地图 | `docs/current/WORKING_CONTEXT.md` | 当前阶段、任务、停在哪、禁区 |
| 系统地图 | `docs/architecture/map.md` | 已有模块，防止重复实现 |
| 组件注册 | `docs/architecture/components.md` | 已有 UI 组件，必须复用 |
| 业务闸口 | `docs/architecture/gates.md` | 统一入口，不得绕过 |
| 能力清单 | `docs/architecture/capabilities.md` | 已有能力，防止重造 |
| 禁区记录 | `docs/architecture/do-not-break.md` | 看起来奇怪但不能改的逻辑 |
| 稳定背景 | `docs/context.md` | 术语、长期决策、计费语义 |

## 两档工作流

### Quick Fix（快速通道）
小 bug、样式调整、文案修改、小范围逻辑修复。
- 不需要 slug，不需要分支，不需要 review
- 四步：读上下文 → 说影响范围 → 修改 → 写 session-log + 提交
- 详见 `docs/workflows/quick-fix.md`

### Standard（标准流程）
完整功能、跨模块改动、多轮迭代需求。
- Grill 需求 → slug → 任务拆分 → 执行 → review → 验收 → 归档
- 默认 main 线开发，按条件才开分支
- 详见 `docs/workflows/standard.md`

## Agent 角色

| Agent | 定位 | 核心职责 |
|-------|------|---------|
| Claude | 需求 + 验收 | Grill 需求、escalation 接管、最终验收、归档授权 |
| Codex | 规划 + 执行 | 任务拆分、开发实现、文档写回、架构记忆维护 |
| Gemini | 分析 + review | 只读审查、调研分析（严禁修改任何文件）|

## 目录结构

```
├── AGENTS.md                        # 唯一权威入口（所有规则在此）
├── CLAUDE.md                        # Claude 职责说明
├── CODEX.md                         # Codex 职责说明
├── GEMINI.md                        # Gemini 职责说明
├── README.md
├── code/                            # 所有可运行代码
└── docs/
    ├── context.md                   # 稳定背景：术语、长期决策、计费语义
    ├── INDEX.md                     # 导航入口
    ├── architecture/                # ★ AI 长期记忆（最重要）
    │   ├── map.md                   # 系统模块地图
    │   ├── components.md            # 组件注册表
    │   ├── gates.md                 # 业务闸口 / 网关
    │   ├── capabilities.md          # 已实现能力清单
    │   └── do-not-break.md          # 禁区：不能改回去的逻辑
    ├── current/                     # 当前工作状态
    │   ├── WORKING_CONTEXT.md       # 当前开工入口（每次必读）
    │   ├── session-log.md           # Quick Fix 轻量记录
    │   ├── _dashboard.md            # Standard 模式活跃任务看板
    │   ├── tasks.md                 # Standard 模式任务列表
    │   ├── acceptance.md            # Standard 模式验收文档
    │   └── <slug>/                  # 仅多轮持续需求建立
    ├── workflows/
    │   ├── quick-fix.md             # Quick Fix 操作说明
    │   └── standard.md              # Standard 操作说明
    ├── archive/                     # 已完成归档（<date>-<slug>）
    └── templates/                   # 文档模板
```

## Git 规范

- **默认：main 线直接开发，原子提交**
- **分支：风险隔离工具，按条件触发**（见 AGENTS.md）
- commit message 写业务意图，不写"fix bug" / "update"
