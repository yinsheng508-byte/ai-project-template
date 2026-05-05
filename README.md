# AI 项目模板仓库

这是一个面向长期迭代项目的最小 AI 协作开发模板。

目标：
- 保持结构简单
- 支持多 agent 协作
- 支持 worktree 并行开发
- 支持 review / escalation
- 支持长期迭代与历史保留

适用工具栈：
- Claude Code
- Codex
- Gemini
- Superset
- VS Code
- Git / Git worktree

## 最重要的规则
**整个项目运行所需的代码和文档，全部统一放在 `code/` 文件夹中。**

目的：
- 保证功能运行完整
- 方便后续打包
- 方便查看和交付

`docs/` 只用于记录协作过程，不承载运行必需内容。

## 最小流程
1. 先看 `docs/workflow.md` 了解流程
2. Claude 读 `docs/context.md`，grill 用户澄清需求，填写 `docs/current/requirements.md`，更新 `docs/context.md`
3. Codex Planner 按垂直 slice 拆任务，标注 AFK/HITL，填写 `docs/current/tasks.md`
4. Codex Builders 按 slice 开发，运行相关代码和文档统一落到 `code/`
5. Gemini 负责 review 和风险标记，更新 `docs/current/review.md`
6. Claude 最终验收，更新 `docs/current/acceptance.md`
7. 当前轮次完成后，将 `docs/current/` 归档到 `docs/archive/`（`docs/context.md` 不归档）
8. 人工确认后再合并到 `main`

## 推荐阅读顺序
0. `docs/context.md`（已有项目必读：先了解语言和历史）
1. `README.md`
2. `docs/INDEX.md`
3. `docs/workflow.md`
4. `docs/current/requirements.md`
5. `docs/current/tasks.md`
6. `docs/templates/review-checklist.md`
7. `docs/templates/acceptance-checklist.md`

## 目录说明
- `code/`：项目运行、部署、交付所需的全部代码和文档
- `docs/current/`：当前这一轮协作文档
- `docs/archive/`：历史轮次归档
- `docs/templates/`：复用模板与检查单

