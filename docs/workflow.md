# WORKFLOW

> 每个阶段的完成条件：**代码（或决策）已完成 + 对应文档已写回**，两者缺一不可。

---

## Phase 0：工作流注册（Claude）

**触发**：用户提出新需求

1. 与用户确认 slug（kebab-case，如 `user-login`）和需求类型（feature / bugfix / refactor / ops）
2. 在 `docs/current/_dashboard.md` 新增一行，状态=Requirements
3. 从 `docs/templates/requirements/<类型>.md` 复制到 `docs/current/<slug>/requirements.md`
4. 从对应模板复制创建 `docs/current/<slug>/tasks.md`、`review.md`、`acceptance.md`

**完成条件**：`_dashboard.md` 中 slug 行已存在；`docs/current/<slug>/` 目录及四个文件已创建

---

## Phase 1：需求澄清（Claude）

1. 读 `docs/context.md` + `<slug>/requirements.md`
2. Grill 用户关键决策（一次一问，给推荐答案；能自查代码的不问）
   停止条件：关键决策已确认 / 用户说够了 / 连续 3 问都直接接受推荐答案
3. **每次决策确认后立即写回** `<slug>/requirements.md` Grill 记录区，不攒到最后
4. 需求定稿后更新 `docs/context.md`（本轮新术语/决策）
5. 更新 `_dashboard.md` 状态→Planning

**完成条件**：`requirements.md` 已完整填写；`context.md` 已更新；`_dashboard.md` 状态=Planning

---

## Phase 2：任务编排（Codex Planner）

1. 读 `_dashboard.md` 确认 slug → 读 `<slug>/requirements.md`
2. 按垂直 slice 拆任务（每个 slice = 一条端到端用户路径，不是单一技术层）
3. 标记并行 / 串行依赖、AFK / HITL，填写 `<slug>/tasks.md`
4. 用户确认 tasks.md 后：
   - 执行 `git checkout -b feat/<slug>`
   - 更新 `<slug>/tasks.md` 分支字段
   - 更新 `_dashboard.md` 状态→Development（填入分支名）

**完成条件**：`tasks.md` 已定稿；`feat/<slug>` 分支已创建；`_dashboard.md` 已更新

---

## Phase 3：开发执行（Codex Builder）

每个任务独立执行：
1. 执行 `git checkout -b feat/<slug>/T<n>`
2. 立即更新 `<slug>/tasks.md` 任务状态=进行中
3. 仅修改授权范围内的文件
4. 执行可演示路径验证
5. 更新 `<slug>/tasks.md` 任务状态=待review

所有任务均完成后：
6. 更新 `_dashboard.md` 状态→Review

**完成条件**：所有任务状态=待review；`tasks.md` 已实时更新；`_dashboard.md` 状态=Review

---

## Phase 4：Review / Escalation（Gemini 主导，Claude 接管升级）

1. Gemini 读 `_dashboard.md` → 找 Review 状态 slug
2. 读 `<slug>/tasks.md` + `<slug>/requirements.md` + `code/` 相关代码
3. 在对话框输出格式化 review 结论（见 GEMINI.md 中的输出格式）
4. Codex 将 Gemini 结论原样写入 `<slug>/review.md`

**review 通过时**：
- Builder 执行 `git merge feat/<slug>/T<n>` 合并到 feature 分支
- 更新 `<slug>/tasks.md` 合并状态=已合并
- 全部任务通过后，更新 `_dashboard.md` 状态→Acceptance

**review 不通过时**：
- Codex 修复问题，重走 Phase 3 → Phase 4
- 同一问题失败 2 次：写入 `<slug>/review.md` escalation 区，升级给 Claude
- Claude 完成分析后，修复动作回到 Codex

**完成条件**：所有任务 review 通过；`review.md` 已填写；合并状态已更新；`_dashboard.md` 状态=Acceptance

---

## Phase 5：最终验收（Claude）

1. 读 `<slug>/requirements.md` + `<slug>/tasks.md` + `<slug>/review.md`
2. 核查：功能完成、范围合规、无未解决严重问题、机器检查通过
3. 写入 `<slug>/acceptance.md` 验收结论（含明确的 merge 授权）
4. 更新 `_dashboard.md` 状态→Merging

**完成条件**：`acceptance.md` 已填写 merge 授权；`_dashboard.md` 状态=Merging

---

## Phase 6：合并与归档（自动执行，无需外部指令）

**本地项目**：
```bash
git merge feat/<slug>    # Claude 执行，合并到 main
```
随后立即：
1. `docs/current/<slug>/` → `docs/archive/<YYYY-MM-DD>-<slug>/`
2. `docs/context.md` 补录本轮新决策
3. `_dashboard.md` 移除该 slug 行
4. 流程结束

**线上项目**（启用 OPS.md 时）：
1. 执行 merge → main，更新 `_dashboard.md` 状态→Deploying
2. OPS 准备部署，HITL 等待用户确认
3. 用户确认 → OPS 执行部署 → 健康检查
4. 健康检查通过 → OPS 写回 `<slug>/acceptance.md` 部署区
5. 执行归档（同上）

---

## 两击规则

同一 agent 在同一问题连续失败 2 次：
- 停止继续尝试
- 立即写入 `<slug>/review.md` escalation 区
- 复杂 bug / 重规划 → Claude
- 大上下文 / 影响面分析 → Gemini

## 三条底线

1. 不要直接让 agent 修改整个项目
2. 不要把多个目标塞进同一任务
3. 不要因为 agent 说"完成了"就直接相信，必须看验证和 review
