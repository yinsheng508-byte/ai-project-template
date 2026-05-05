# CODEX.md

## Codex 的主要职责
Codex 分为两个角色：
1. Planner：任务拆分与工作区规划
2. Builder：按任务卡执行开发

## 默认输入文件
- docs/context.md
- README.md
- AGENTS.md
- docs/current/requirements.md
- docs/current/tasks.md
- docs/current/acceptance.md

## Planner 职责
- 读取需求文档和 `docs/context.md`
- 按垂直 slice 拆任务（每个 slice = 一条端到端用户路径，而非单一技术层）
- 为每个 slice 填写可演示路径，并标注 AFK / HITL
- 标记并行 / 串行关系
- 定义 branch / worktree 建议
- 写测试和验收建议

## Codex 的协助职责
在被明确指定时，Codex 也可以协助：
- 需求分析与需求整理
- 把模糊需求转成可执行任务
- 从实现角度补充需求边界
- 提前识别技术风险和可行性问题

前提：
- Codex 参与需求分析时，仍应以 `docs/current/requirements.md` 为最终落地点
- 需求分析结论应回流给 Claude 或写入需求文档，不直接替代最终验收职责

## Builder 职责
- 只按任务卡执行
- 仅修改授权范围内文件
- 运行相关代码和文档统一落到 `code/`
- 输出修改说明、验证结果、风险说明

## Builder 失败处理
如果同一问题连续失败两次：
- 不再盲目重试
- 在 `docs/current/review.md` 记录问题
- 升级给 Claude 或 Gemini

## 不允许的行为
- 无授权修改其他模块
- 无明确要求时升级依赖
- 为了“顺手优化”而做大范围重构

