# AI 项目模板仓库

这是一个最小可用的 AI 编程项目模板。

目标：
- 保持结构简单
- 支持多 agent 协作
- 支持 worktree 并行开发
- 支持 review / escalation
- 支持 prompt 文件复用

适用工具栈：
- Claude Code
- Codex
- Gemini
- Superset
- VS Code
- Git / Git worktree

## 最小流程
1. 先看 `docs/00-workflow.md` 了解完整步骤
2. 先确认 `main` 已提交、基线稳定、目录结构已定
3. Claude 负责需求梳理，填写 `docs/01-requirements.md`
4. Codex Planner 负责任务拆分，填写 `docs/02-task-plan.md`
5. 根据任务规模控制 agent 数：小任务 1–2 个，中任务 2–3 个，大任务 3–5 个，总数不超过 5 个
6. Codex Builders 按任务卡在独立 worktree 中开发；有依赖关系的任务必须串行
7. Gemini 负责 review 和风险标记，只审查不直接改代码，更新 `docs/03-review-and-escalation.md`
8. Claude 最终验收，更新 `docs/04-acceptance.md`
9. 人工确认后再合并到 `main`

## 推荐阅读顺序
1. `README.md`
2. `docs/INDEX.md`
3. `docs/00-workflow.md`
4. `docs/01-requirements.md`
5. `docs/02-task-plan.md`
6. `docs/06-review-checklist.md`
7. `docs/07-acceptance-checklist.md`

## 核心原则
- `main` 必须先提交干净，再开始并行开发
- 目录结构一旦确认，agent 不得擅自重构或改目录
- 每个任务都必须明确允许修改范围与禁止修改范围
- 小任务不要过度拆分；一个 agent 可以承担多个高度相关的子任务
- 有依赖关系的任务必须串行，独立任务才并行
- 同一问题同一 agent 失败两次，必须 escalation
- 修改前先读文件，修改后必须执行最相关验证
- Gemini 在 review 阶段只负责审查，不直接修改代码
- `main` 只做集成，不做日常功能开发
- 每一步都必须有对应文档产物，不靠聊天记录管理项目
- 先看索引文件，再按需读取文档
