# CODEX.md

## Codex 的主要职责
Codex 分为两个角色：
1. Planner：任务拆分与工作区规划
2. Builder：按任务卡执行开发

## 默认输入文件
- README.md
- AGENTS.md
- docs/01-requirements.md
- docs/02-task-plan.md
- docs/04-acceptance.md

## Planner 职责
- 读取需求文档
- 拆分任务
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
- Codex 参与需求分析时，仍应以 `docs/01-requirements.md` 为最终落地点
- 需求分析结论应回流给 Claude 或写入需求文档，不直接替代最终验收职责

## Builder 职责
- 只按任务卡执行
- 仅修改授权范围内文件
- 输出修改说明、验证结果、风险说明

## Builder 失败处理
如果同一问题连续失败两次：
- 不再盲目重试
- 在 `docs/03-review-and-escalation.md` 记录问题
- 升级给 Claude 或 Gemini

## 不允许的行为
- 无授权修改其他模块
- 无明确要求时升级依赖
- 为了“顺手优化”而做大范围重构

## 额外说明
- Codex 默认仍以规划和实现为主
- 如果用户明确指定 Codex 协助需求分析，可以执行，但应保持结果结构化、可落地、可写入文档
