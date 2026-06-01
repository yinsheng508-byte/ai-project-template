# CODEX.md

## Codex 的两个角色

### Planner（任务规划）

**开工前读取：**
- `docs/context.md`
- `docs/current/_dashboard.md`（确认目标 slug）
- `docs/current/<slug>/requirements.md`

**执行步骤：**
1. 按垂直 slice 拆任务（每个 slice = 一条端到端用户路径，不是单一技术层）
2. 标记并行 / 串行依赖、AFK / HITL
3. 填写 `docs/current/<slug>/tasks.md`，包含可演示路径、修改范围、验收点
4. 用户确认 tasks.md 后：
   - 执行 `git checkout -b feat/<slug>` 创建 feature 分支
   - 更新 `<slug>/tasks.md` 分支字段
   - 更新 `_dashboard.md` 状态→Development（同时填入分支名）

### Builder（开发执行）

**开工前读取：**
- `docs/current/_dashboard.md`（确认 slug + 任务编号）
- `docs/current/<slug>/tasks.md`

**每个任务的执行流程：**
1. 执行 `git checkout -b feat/<slug>/T<n>`
2. 立即更新 `<slug>/tasks.md` 任务状态=进行中
3. 仅修改当前任务授权范围内的文件
4. 完成后执行可演示路径验证
5. 更新 `<slug>/tasks.md` 任务状态=待review
6. Gemini review 通过后：
   - 执行 `git merge feat/<slug>/T<n>` 合并到 feature 分支
   - 更新 `<slug>/tasks.md` 合并状态=已合并
7. 将 Gemini 输出的 review 结论原样写入 `<slug>/review.md`

## 文档写回规则

每个操作是原子单元：**操作完成，文档立即更新**，不批量攒写。

| 动作 | 立即写回 |
|------|---------|
| feature 分支创建 | `<slug>/tasks.md` 分支字段 + `_dashboard.md` 状态 |
| 开始任务 | `<slug>/tasks.md` 任务状态=进行中 |
| 完成任务 | `<slug>/tasks.md` 任务状态=待review |
| T\<n> 合并 feature | `<slug>/tasks.md` 合并状态=已合并 |
| Escalation 发生 | `<slug>/review.md` escalation 区（立即，不等解决） |

## Codex 的协助职责

在被明确指定时，Codex 也可以协助：
- 需求分析与需求整理（结论回流给 Claude 或写入需求文档）
- 把模糊需求转成可执行任务
- 从实现角度补充需求边界和技术风险

## 禁止事项
- 不在未明确 slug 的情况下创建分支或修改文件
- 不越权修改未授权模块
- 不做未经明确要求的依赖升级
- 不做仅为便利的大范围重构
- 连续失败 2 次必须升级，不尝试第 3 次

## Escalation 处理

同一问题连续失败 2 次时：
1. 立即在 `<slug>/review.md` escalation 区填写交接信息
2. 通知 Claude 或 Gemini 接手
3. 等待处理结果，不自行继续尝试
