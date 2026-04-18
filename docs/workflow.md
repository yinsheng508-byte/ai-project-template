# workflow

这是项目的最小工作流说明，所有 agent 和使用者都应先阅读。

## 标准步骤

### 第 1 步：需求澄清
负责人：Claude（默认）
可协助：Codex / Gemini（被明确指定时）

要做什么：
- 明确项目背景、目标、非目标、核心功能、约束
- 把结果写入 `docs/current/requirements.md`

完成标准：
- 需求文档已完整
- 关键边界和验收标准已写清楚

---

### 第 2 步：任务编排
负责人：Codex Planner（默认）
可协助：Claude / Gemini（被明确指定时）

要做什么：
- 基于需求文档拆任务
- 标记并行 / 串行关系
- 规划 branch / worktree
- 明确每个任务的修改范围和测试要求
- 把结果写入 `docs/current/tasks.md`

完成标准：
- 每个任务有明确目标、边界、branch、worktree
- 没有明显文件范围重叠

---

### 第 3 步：执行开发
负责人：Codex Builders（默认）
可协助：Claude / Gemini（被明确指定时）

要做什么：
- 每个任务在独立 worktree 中执行
- 仅修改授权范围内文件
- 运行相关代码和文档统一落到 `code/`
- 输出改动说明、验证结果、风险说明

完成标准：
- 任务目标已实现
- 已完成最相关验证
- 有可 review 的结果

---

### 第 4 步：Review / Escalation
负责人：Gemini（默认 review）
复杂问题升级：Claude / Gemini

要做什么：
- 审查当前改动是否符合目标
- 检查是否超范围修改
- 检查风险、遗漏、边界问题
- 如 builder 连续失败两次，记录 escalation
- 把结果写入 `docs/current/review.md`

完成标准：
- review 已明确通过 / 待修改 / 风险较高
- 如有升级问题，已写清交接信息

---

### 第 5 步：最终验收
负责人：Claude（默认）

要做什么：
- 对照 requirements / tasks / review 结果做最终验收
- 检查目标是否完成，风险是否收口
- 把结果写入 `docs/current/acceptance.md`

完成标准：
- 最终验收结论明确
- 明确是否允许合并 main

---

### 第 6 步：归档
当前轮次完成后：
- 将 `docs/current/` 中的文档整体归档到 `docs/archive/<日期-名称>/`
- 再开始下一轮

---

## 两击规则
如果同一 agent 在同一问题上连续失败两次：
- 停止继续盲试
- 进入 escalation
- 复杂 bug / 重规划 → Claude
- 大上下文 / 影响面分析 → Gemini

---

## 三条底线
1. 不要直接让 agent 修改整个项目
2. 不要把多个目标塞进同一任务
3. 不要因为 agent 说“完成了”就直接相信，必须看验证和 review

---

## 两条硬规则
1. 整个项目运行所需的代码和文档，全部放在 `code/`
2. `docs/` 只用于记录协作过程文档
