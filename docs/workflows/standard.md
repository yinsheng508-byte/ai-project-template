# Standard 工作流

适用：完整功能、跨模块改动、多轮持续迭代的需求。

## 流程概览

```
Phase 0  Claude   注册 slug → 初始化文档
Phase 1  Claude   Grill 需求 → requirements.md 定稿 → 更新 context.md
Phase 2  Codex    拆任务 → tasks.md → 用户确认
Phase 3  Codex    逐任务执行 → 逐项验收 → 文档写回
Phase 4  Gemini   review（可选）/ Claude Escalation（需要时）
Phase 5  Claude   最终验收 → acceptance.md
Phase 6  Claude   归档 → docs/archive/<YYYY-MM-DD>-<slug>/
```

---

## Phase 0：注册工作流（Claude）

1. 与用户确认 slug（kebab-case，如 `user-login`）
2. 确认需求类型（feature / bugfix / refactor）
3. 在 `docs/current/_dashboard.md` 新增一行，状态=Requirements
4. 从 `docs/templates/requirements/<类型>.md` 复制模板到 requirements.md
5. 单次需求：使用 `docs/current/` 下的公用文档（requirements.md、tasks.md、acceptance.md）
   多轮持续需求：建立 `docs/current/<slug>/` 独立目录

---

## Phase 1：需求澄清（Claude）

1. 读 context.md + requirements.md + 相关源码
2. Grill 用户：一次一问，给推荐答案，能自查代码的不问
3. 每次用户确认一个决策，立即写回 requirements.md
4. 定稿后更新 context.md（新术语和决策）
5. 更新 _dashboard.md 状态→Planning

---

## Phase 2：任务规划（Codex Planner）

1. 读 requirements.md + `docs/architecture/map.md` + `docs/architecture/capabilities.md`
2. 按垂直 slice 拆任务（每个 slice = 一条完整端到端用户路径）
3. 标注依赖关系（哪些必须串行，哪些可并行）
4. 填写 tasks.md，每项包含：可演示路径、修改范围、验收点
5. 等用户确认
6. 如触发 AGENTS.md 中的分支条件，创建 `feat/<slug>`
7. 更新 _dashboard.md 状态→Development

---

## Phase 3：开发执行（Codex Builder）

**每个任务开始前：**
- 查 `docs/architecture/components.md`（组件是否已有）
- 查 `docs/architecture/do-not-break.md`（要改的代码是否有约束）

**执行中：**
- 更新 tasks.md 任务状态=进行中
- 只修改当前任务授权范围内的文件
- 完成后执行可演示路径验证

**执行后：**
- 更新 tasks.md 任务状态=待review 或 已完成
- 更新 WORKING_CONTEXT.md
- 按写回矩阵更新 architecture/ 下对应文件
- 如有分支且 Gemini review 通过：合并任务分支

---

## Phase 4：Review / Escalation

**Gemini review（按需，非强制）：**
- 只读审查，对话框输出结论
- Codex 将结论写入 review.md

**Claude Escalation（Codex 连续失败 2 次触发）：**
- Claude 接管根因分析，结论写入 review.md escalation 区
- 修复动作回交 Codex

---

## Phase 5：最终验收（Claude）

1. 读 requirements.md + tasks.md + review.md（如有）
2. 核查：功能完成、范围合规、review 通过
3. 写入 acceptance.md 验收结论
4. 更新 _dashboard.md 状态→Done

---

## Phase 6：归档（Claude）

1. 如有分支：merge `feat/<slug>` → main
2. `docs/current/<slug>/` → `docs/archive/<YYYY-MM-DD>-<slug>/`（有独立目录时）
3. 补录 context.md（本轮新决策）
4. 从 _dashboard.md 移除该行

---

## 分支使用规则

默认 main 线开发，原子提交。以下情况才开分支（满足任一）：

- 改动预计超过 1 天
- 同时有两个方向并行
- 涉及支付、扣费、登录、数据迁移、部署
- 需要独立 review 后才能合并
- 改动可能推翻现有架构
- 想保留 main 随时可发布
