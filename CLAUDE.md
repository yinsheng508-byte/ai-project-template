# CLAUDE.md

> 协作规则以 AGENTS.md 为准。本文件仅描述 Claude 的具体职责与操作步骤。

## Claude 的核心职责

1. **需求澄清（Grill）** — 一次一问，给推荐答案，能自查代码的不问
2. **需求文档定稿** — 写 requirements.md，更新 context.md
3. **Escalation 接管** — Codex 连续失败 2 次后，Claude 接管根因分析与重规划
4. **最终验收** — 核查功能完成、范围合规、review 通过
5. **归档授权** — 验收通过后执行 merge + 归档

## 开工读取顺序

遵照 AGENTS.md 的标准读取顺序。Standard 模式下额外读：
- `docs/current/<slug>/requirements.md`
- `docs/current/<slug>/tasks.md`
- `docs/current/<slug>/review.md`（如有）

## Standard 模式操作流程

### Phase 0：注册工作流

1. 与用户确认 slug（kebab-case，如 `user-login`）
2. 确认需求类型（feature / bugfix / refactor）
3. 在 `docs/current/_dashboard.md` 新增一行，状态=Requirements
4. 从 `docs/templates/requirements/<类型>.md` 复制模板到 requirements.md
5. 单次需求：使用 `docs/current/` 下的公用文档
   多轮持续需求：建立 `docs/current/<slug>/` 独立目录

### Phase 1：需求澄清（Grill）

1. 读 context.md + requirements.md + 相关源码
2. 逐步追问，一次一问，给推荐答案，能自查代码的不问
3. 每次用户确认一个决策，立即写回 requirements.md
4. 定稿后更新 context.md（新术语和决策）
5. 更新 _dashboard.md 状态→Planning

### Phase 4：Escalation

1. 接管根因分析、重规划、复杂 bug 处理
2. 结论写入 `review.md` escalation 区
3. 修复动作尽量回交 Codex 落地

### Phase 5：最终验收

1. 读 requirements.md + tasks.md + review.md
2. 核查：功能完成、范围合规、review 通过
3. 写入 acceptance.md 验收结论（含明确的授权结论）
4. 更新 _dashboard.md 状态→Done

### Phase 6：归档

1. 如有分支：merge `feat/<slug>` → main
2. `docs/current/<slug>/` → `docs/archive/<YYYY-MM-DD>-<slug>/`
3. 补录 context.md（本轮新决策）
4. 从 _dashboard.md 移除该行

## 工作原则

- 优先做需求澄清、架构讨论、重规划、验收
- 复杂问题分析后尽量回交 Codex 落地
- 任何决策立即写回文档，不留中间状态
- 不因协助而跳过 review / acceptance 流程
