# AGENTS.md

## 目标
保持 AI 协作开发过程清晰、可控、可 review、可回滚。

## 核心协作原则

1. 先把项目当前正式文件全部提交到 `main`，再开始并行开发
2. `main` 是正式基线，不允许在基线未稳定时直接派生大量任务
3. 目录结构一旦确认，就是正式结构，agent 不得擅自重构或改目录
4. 整个项目运行所需的代码和文档，全部放在 `code/`
5. `docs/` 只记录需求、任务、review、验收等过程文档
6. **文档即状态**：任何决策、代码变更、阶段推进必须立即写回对应文档，文档落后于实际状态等同于任务未完成
7. 先读 `docs/current/_dashboard.md` 了解全局，再按当前阶段按需读取文档
8. 修改后必须执行最相关验证
9. Gemini 在 review 阶段只负责审查，不直接修改代码或文档
10. 每个任务必须有可演示路径；HITL 任务在执行前需人工确认再交 agent

## 任务流转

标准流转顺序：
1. 工作流注册（Claude 在 `_dashboard.md` 登记 slug）
2. 需求澄清（Claude）
3. 任务编排（Codex Planner）
4. 执行开发（Codex Builder）
5. Review / Escalation（Gemini / Claude）
6. 最终验收（Claude）
7. 自动归档（验收通过后立即触发，无需外部指令）
8. 部署（可选，仅线上项目，用户 HITL 确认后由 OPS 执行）

默认责任人：
- 工作流注册 → Claude
- 需求澄清 → Claude
- 任务编排 → Codex Planner
- 执行开发 → Codex Builder
- Review / Escalation → Gemini / Claude
- 最终验收 → Claude
- merge + 归档 → Claude（自动执行）
- 部署 → OPS（仅线上项目，用户 HITL 确认后）

在被明确指定时，Claude、Codex、Gemini 都可以协助其他合理任务，但必须遵守当前边界与流程。

## 索引文件与按需读取

所有 agent 开始工作前：
0. 先读 `docs/context.md`（了解项目语言和历史决策）
1. 再读 `README.md`
2. 再读 `docs/current/_dashboard.md`（了解当前所有活跃工作流）
3. 确认目标 slug，进入 `docs/current/<slug>/` 按需读取
4. 不要默认一次性读完整个仓库

## 文档写回责任矩阵

| 触发动作 | 写入文档 | 负责执行 |
|---------|---------|---------|
| 用户确认 Grill 决策 | `<slug>/requirements.md` Grill 记录区 | Claude |
| 需求文档定稿 | `context.md`（新术语/决策） | Claude |
| 工作流注册 | `_dashboard.md` 新增行，状态=Requirements | Claude |
| 任务拆分定稿 + 用户确认 | `<slug>/tasks.md`；`_dashboard.md` 状态→Planning | Codex Planner |
| feature 分支创建 | `<slug>/tasks.md` 分支字段；`_dashboard.md` 状态→Development | Codex Planner |
| Builder 开始某任务 | `<slug>/tasks.md` 任务状态=进行中 | Codex Builder |
| Builder 完成某任务 | `<slug>/tasks.md` 任务状态=待review | Codex Builder |
| 任务分支合并 feature | `<slug>/tasks.md` 合并状态=已合并 | Codex Builder |
| Escalation 发生 | `<slug>/review.md` escalation 区（立即写入，不等解决） | 发起 agent |
| Gemini 完成 review | `<slug>/review.md`（Gemini 口述，Codex 写入） | Codex Builder |
| Claude 完成验收 | `<slug>/acceptance.md` 验收结论 | Claude |
| feature 分支合并 main | `_dashboard.md` 状态→Done/Deploying | Claude |
| OPS 部署完成 | `_dashboard.md` 状态→Done；`<slug>/acceptance.md` 部署区 | OPS |
| 归档完成 | `_dashboard.md` 移除该行；`context.md` 补录 | Claude / OPS |

## Git 自动化规范

### 分支命名
- feature 分支：`feat/<slug>`
- 任务分支：`feat/<slug>/T<n>`（n 从 1 开始）

### 创建时机
- `feat/<slug>`：Planner 完成 tasks.md、用户确认后创建
- `feat/<slug>/T<n>`：Builder 开始执行该任务前创建

### 合并规则
- `feat/<slug>/T<n>` → `feat/<slug>`：Gemini review 通过后，由 Builder 执行
- `feat/<slug>` → `main`：Claude 验收通过后，由 Claude 执行
- 禁止跳过中间层直接合并到 `main`

### 本地项目 vs 线上项目
- **本地项目**：merge 到 `main` 后自动归档，流程结束
- **线上项目**：merge 后由 OPS 准备部署，用户 HITL 确认，OPS 执行，用户确认线上正常后归档

## 并行开发规则

### 多工作流协调
- 不同 slug 的文件天然隔离，可自由并行
- 多个 slug 依赖同一共享核心文件时，必须在 `_dashboard.md` 标注，采用串行处理
- 同时活跃 slug 建议不超过 5 个

### agent 分配规则
- 总 agent 数不超过 5 个
- 一个 agent 可执行多个任务，但这些任务应属于同一 slug 或高度相关
- 每个 agent 有清晰的 slug 归属，不跨 slug 操作
- Gemini review 可处理多个 slug，但同时处理上限 3 个

### 分支 / worktree 规则
- 只有 `main` 已提交、基线稳定后，才创建 branch / worktree
- 有依赖关系的任务必须串行，不得盲目并行
- 多个 agent 不应同时修改同一核心共享文件

## 默认执行策略
- 如果用户已经明确指定任务，默认当前工作区就是有效工作区
- 不要每次都先检查 branch / worktree / git status
- 只有在以下情况，才进入严格预检：
  - 用户要求并行开发
  - 需要新开 branch / worktree
  - 涉及共享核心文件
  - 准备提交 / 合并 / review
  - 发现冲突风险或目录状态异常

## 升级规则（Escalation）

满足以下任一条件，必须升级：
- 同一问题连续失败 2 次
- 连续 2 次得到相似错误结果
- 需要跨模块重新设计
- 影响范围超出当前 slug

升级路径：
- 架构 / 复杂 bug / 重规划 → Claude
- 大上下文 / 影响面分析 → Gemini
- 分析完成后，修复动作默认回到 Codex

## 交接要求（Handoff）

升级时必须立即写入 `<slug>/review.md` escalation 区：
- 当前 slug + 任务编号
- 当前 agent
- 问题描述
- 已尝试方案
- 当前错误信息
- 涉及文件
- 升级给谁
- 希望完成什么

## 归档规则

- 验收通过后自动触发，无需外部指令
- `docs/current/<slug>/` → `docs/archive/<YYYY-MM-DD>-<slug>/`
- `_dashboard.md` 中移除该 slug 行
- `docs/context.md` 补录本轮新决策（保留，不归档）
- 不覆盖历史轮次文档

## 禁止事项
- 不要在 `main` 还是空或未整理好的情况下直接开大量分支
- 不要让 agent 无边界修改整个项目
- 不要让有依赖关系的任务盲目并行
- 不要跳过 review 直接合并
- 不要在当前工作区有未处理改动时随意合并别的分支
- 不要在未明确 slug 的情况下创建分支或修改文件
- 不要让文档状态滞后于实际进展
