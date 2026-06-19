# CODEX.md

> AGENTS.md 是 Codex 的权威入口，开工前必须先读 AGENTS.md。
> 本文件补充 Codex 的具体操作细节，不重复定义规则。

## Codex 的核心职责

1. **任务规划（Planner）** — 按垂直 slice 拆任务，标记依赖，填写 tasks.md
2. **开发执行（Builder）** — 串行执行，完成一项验收一项
3. **文档写回** — 每次完成必须按 AGENTS.md 写回责任矩阵更新对应文档
4. **架构记忆维护** — 新增组件 / 模块 / 能力 / 非常规决策必须同步到 `docs/architecture/`

## Planner 操作步骤

1. 读 requirements.md + `docs/architecture/map.md` + `docs/architecture/capabilities.md`
2. 按垂直 slice 拆任务（每个 slice = 一条端到端用户路径，不是单一技术层）
3. 标注并行 / 串行依赖
4. 填写 tasks.md，每项包含：可演示路径、修改范围、验收点
5. 等用户确认后再开始执行
6. 如触发 AGENTS.md 中的分支条件，创建 `feat/<slug>`

## Builder 操作步骤

每个任务执行前：
1. 查 `docs/architecture/components.md`（要用的组件是否已有）
2. 查 `docs/architecture/do-not-break.md`（要改的代码是否有约束）

每个任务执行中：
3. 更新 tasks.md 任务状态=进行中
4. 只修改当前任务授权范围内的文件
5. 完成后执行可演示路径验证

每个任务执行后：
6. 更新 tasks.md 任务状态=待review 或 已完成
7. 更新 WORKING_CONTEXT.md（当前状态、上次停在哪里）
8. 按写回矩阵更新 architecture/ 下对应文件
9. Gemini review 通过后合并（如有分支）

## 架构记忆维护规则

| 完成动作 | 立即写入 |
|---------|---------|
| 新增 UI 组件 | `docs/architecture/components.md` |
| 新增模块或功能入口 | `docs/architecture/map.md` |
| 新增系统能力（可复用的逻辑） | `docs/architecture/capabilities.md` |
| 新增业务闸口 | `docs/architecture/gates.md` |
| 某段代码看起来奇怪但有意为之 | `docs/architecture/do-not-break.md` |

**这是最重要的工作纪律：不写回 = 下一轮 Codex 失忆 = 重复造轮子。**

## 禁止事项

- 没读 `components.md` 就新增 UI 组件（先查是否已有）
- 没读 `do-not-break.md` 就修改已有代码
- 越权修改未授权模块
- 做未经明确要求的依赖升级或大范围重构
- 连续失败 2 次继续尝试（必须升级给 Claude）
- 任务完成不写回文档

## Escalation 处理

同一问题连续失败 2 次时：
1. 立即在 `review.md` escalation 区填写：问题描述、已尝试方案、错误信息、涉及文件
2. 通知 Claude 接管
3. 等待处理结果，不自行继续尝试
