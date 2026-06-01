# AI 项目模板仓库

面向长期迭代项目的 AI 协作开发模板，支持多 agent 协同、多需求并行、Git 自动化与文档自动同步。

## 核心设计

- **context.md**：项目的长期记忆。永不归档，跨轮次积累领域语言和重大决策
- **Grill**：Claude 在填写需求文档前逐步追问用户关键决策，避免在错误理解上高速执行
- **垂直 Slice + AFK/HITL**：任务按端到端用户路径切分，每个 slice 完成后可独立演示
- **多线程并行**：多个需求可同时在不同阶段推进，互不阻塞
- **文档即状态**：每一步决策、代码变更、阶段推进都立即写回文档，流程结束后自动归档

## 最重要的规则

**整个项目运行所需的代码和文档，全部统一放在 `code/` 文件夹中。**

`docs/` 只用于记录协作过程，不承载运行必需内容。

## Agent 角色

| Agent | 定位 | 主要职责 |
|-------|------|---------|
| Claude | 需求 + 验收 | Grill 需求、生成文档、escalation、最终验收、merge 授权与归档 |
| Codex | 规划 + 执行 | 任务拆分、分支创建、开发实现、文档写回 |
| Gemini | 分析 + review | 只读审查、需求落地分析、输出格式化 review 结论 |
| OPS | 部署（可选） | **仅线上项目启用**，负责构建验证、部署、健康检查 |

## 多线程架构

每个需求对应一个 **slug**（如 `user-login`），slug 贯穿：
- 文档目录：`docs/current/<slug>/`
- Git 分支：`feat/<slug>` + `feat/<slug>/T<n>`
- 历史归档：`docs/archive/<date>-<slug>/`

全局状态由 `docs/current/_dashboard.md` 统一追踪，所有 agent 开工前必读。

## Git 自动化

```
Planner 确认任务 → 创建 feat/<slug>
Builder 开始任务 → 创建 feat/<slug>/T<n>
Gemini review 通过 → Builder merge T<n> → feat/<slug>
Claude 验收通过 → merge feat/<slug> → main → 自动归档
（线上项目）用户 HITL 确认 → OPS 执行部署 → 归档
```

分支命名：`feat/<slug>` / `feat/<slug>/T<n>`（n 从 1 开始）

## 工作流概览

0. Claude 注册 slug，选类型，初始化 `docs/current/<slug>/`
1. Claude Grill → 需求文档定稿 → 更新 `context.md`
2. Codex Planner 拆任务 → 用户确认 → 创建 `feat/<slug>` 分支
3. Codex Builder 逐任务开发 → 每任务一个 `feat/<slug>/T<n>` 分支
4. Gemini review → Builder 将结论写入 `review.md` → 合并任务分支
5. Claude 最终验收 → merge `feat/<slug>` → main → 自动归档

## 目录结构

```
├── README.md
├── CLAUDE.md               # Claude 职责与操作规范
├── AGENTS.md               # 全局协作原则（所有 agent 必读）
├── CODEX.md                # Codex 职责与操作规范
├── GEMINI.md               # Gemini 职责与操作规范
├── docs/OPS.md             # 可选：仅线上项目启用
├── code/                   # 项目运行所需的全部代码和文档
└── docs/
    ├── context.md          # 永久记忆：术语 + 重大决策
    ├── INDEX.md            # 导航入口
    ├── workflow.md         # 标准工作流
    ├── OPS.md              # 可选：仅线上项目启用
    ├── current/
    │   ├── _dashboard.md           # 全局工作流看板（所有 agent 必读）
    │   └── <slug>/                 # 每个需求一个独立目录
    │       ├── requirements.md
    │       ├── tasks.md
    │       ├── review.md
    │       └── acceptance.md
    ├── archive/            # 已完成轮次，按 <date>-<slug> 归档
    └── templates/          # 各类文档模板
        ├── requirements/
        │   ├── feature.md
        │   ├── bugfix.md
        │   ├── refactor.md
        │   └── ops.md
        ├── tasks.md
        ├── review.md
        ├── acceptance.md
        ├── task-template.md
        ├── review-checklist.md
        └── acceptance-checklist.md
```
