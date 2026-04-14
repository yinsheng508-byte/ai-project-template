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
2. Claude 负责需求梳理，填写 `docs/01-requirements.md`
3. Codex Planner 负责任务拆分，填写 `docs/02-task-plan.md`
4. Codex Builders 按任务卡在独立 worktree 中开发
5. Gemini 负责 review 和风险标记，更新 `docs/03-review-and-escalation.md`
6. Claude 最终验收，更新 `docs/04-acceptance.md`

## 推荐阅读顺序
1. `README.md`
2. `docs/INDEX.md`
3. `docs/00-workflow.md`
4. `docs/01-requirements.md`
5. `docs/02-task-plan.md`
6. `docs/06-review-checklist.md`
7. `docs/07-acceptance-checklist.md`

## 核心原则
- 一任务一分支一 worktree 一 agent
- 同一问题同一 agent 失败两次，必须 escalation
- 修改前先读文件，修改后必须验证
- main 只做集成，不做日常功能开发
- 每一步都必须有对应文档产物，不靠聊天记录管理项目
- 先看索引文件，再按需读取文档
