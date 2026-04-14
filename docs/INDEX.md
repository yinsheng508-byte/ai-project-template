# INDEX

这是项目的索引文件，所有 agent 与使用者都应优先查看本文件来决定“接下来该读什么”。

## 默认阅读顺序
1. `README.md`
2. `docs/INDEX.md`
3. `docs/00-workflow.md`
4. 再按当前阶段按需读取其他文件

## 按阶段读取

### 阶段 1：需求澄清
优先读取：
- `docs/01-requirements.md`

### 阶段 2：任务编排
优先读取：
- `docs/01-requirements.md`
- `docs/02-task-plan.md`
- `docs/05-task-template.md`

### 阶段 3：执行开发
优先读取：
- `docs/02-task-plan.md`
- `docs/05-task-template.md`
- `docs/03-review-and-escalation.md`（若任务有阻塞）

### 阶段 4：Review / Escalation
优先读取：
- `docs/02-task-plan.md`
- `docs/03-review-and-escalation.md`
- `docs/06-review-checklist.md`

### 阶段 5：最终验收
优先读取：
- `docs/01-requirements.md`
- `docs/02-task-plan.md`
- `docs/03-review-and-escalation.md`
- `docs/04-acceptance.md`
- `docs/07-acceptance-checklist.md`

## 按问题类型读取

### 需求不清
- `docs/01-requirements.md`
- `CLAUDE.md`
- `GEMINI.md`
- `CODEX.md`

### 任务不会拆
- `docs/02-task-plan.md`
- `docs/05-task-template.md`
- `CODEX.md`

### 代码卡住 / bug 修不掉
- `docs/03-review-and-escalation.md`
- `CLAUDE.md`
- `GEMINI.md`
- `AGENTS.md`

### 想知道能不能合并
- `docs/06-review-checklist.md`
- `docs/07-acceptance-checklist.md`
- `docs/04-acceptance.md`

## 原则
- 不要一次读完整个仓库所有文档
- 先看索引，再按当前任务按需读取
- 当前阶段只读最相关文档，避免上下文污染
