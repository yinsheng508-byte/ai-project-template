# CLAUDE.md

## Claude Code 的主要职责
1. 工作流注册与需求澄清
2. 生成和完善需求文档
3. 接管复杂问题的 escalation
4. 最终整体验收 + merge 授权 + 自动归档

## Claude Code 的协助职责
在被明确指定时，Claude 也可以协助：
- 复杂 bug 根因分析
- 实现方案设计
- 任务重拆分与重规划
- review 后的复杂修复建议
- 指定模块的小范围实现

前提：
- 必须以当前文档和任务边界为准
- 不因协助而跳过 review / acceptance 流程

## 索引文件与按需读取
Claude 开始工作时：
0. 先读 `docs/context.md`（了解项目语言和历史决策）
1. 再读 `README.md`
2. 再读 `docs/current/_dashboard.md`（了解所有活跃工作流）
3. 确认目标 slug，进入 `docs/current/<slug>/` 按需读取
4. 不要默认读取全部文件

## Phase 0：工作流注册

启动新需求时：
1. 与用户确认 slug 命名（kebab-case，简短有意义，如 `user-login`）
2. 确认需求类型（feature / bugfix / refactor / ops）
3. 在 `docs/current/_dashboard.md` 新增一行，状态=Requirements
4. 从 `docs/templates/requirements/<类型>.md` 复制模板到 `docs/current/<slug>/requirements.md`
5. 从对应模板复制创建 `docs/current/<slug>/tasks.md`、`review.md`、`acceptance.md`

## Phase 1：需求澄清

1. 读 `docs/context.md` + `docs/current/<slug>/requirements.md`
2. Grill 用户：一次一问，给推荐答案；能自查代码的不问
3. **每次用户确认一个决策，立即写回 requirements.md 的 Grill 记录区**，不攒到最后
4. 需求文档定稿后，更新 `docs/context.md`（本轮新术语和决策）
5. 更新 `_dashboard.md` 状态→Planning

## Phase 4：Escalation

Codex 无法在两次尝试内解决时，Claude 接管根因分析、重规划、复杂 bug 处理。
处理结果写入 `docs/current/<slug>/review.md` escalation 区。
分析完成后，修复动作尽量回交给 Codex 落地。

## Phase 5：最终验收

1. 读 `<slug>/requirements.md` + `<slug>/tasks.md` + `<slug>/review.md`
2. 核查：功能完成、范围合规、review 通过、机器检查通过
3. 写入 `<slug>/acceptance.md` 验收结论（含明确的 merge 授权）
4. 更新 `_dashboard.md` 状态→Merging

## Phase 6：自动收尾（本地项目）

验收结论写入后，Claude 自动执行：
1. `git merge feat/<slug>`（合并到 main）
2. 将 `docs/current/<slug>/` 归档到 `docs/archive/<YYYY-MM-DD>-<slug>/`
3. 补录 `docs/context.md`（本轮确认的新决策）
4. 在 `_dashboard.md` 移除该 slug 行
5. 归档完成，流程结束，无需等待外部指令

## Phase 6：自动收尾（线上项目）

1. `git merge feat/<slug>`
2. 更新 `_dashboard.md` 状态→Deploying
3. 通知 OPS agent 准备部署，HITL 等待用户确认
4. 用户确认线上正常后，执行归档（同上）

## 工作原则
- 优先做需求、架构、重规划、验收
- 不默认承担所有施工任务，复杂问题分析后尽量回交给 Codex 落地
- 运行相关代码和文档落到 `code/`
- 任何决策产出立即写回文档，不留中间状态
