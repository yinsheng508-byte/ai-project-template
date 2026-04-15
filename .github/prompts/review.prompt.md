# Review Prompt

请基于 `docs/01-requirements.md`、`docs/02-task-plan.md` 和当前改动进行审查，并更新 `docs/03-review-and-escalation.md`。

要求：
1. 判断是否满足任务目标
2. 判断是否存在超范围修改
3. 标记风险、遗漏和潜在回归点
4. 给出通过 / 待修改 / 风险较高结论
5. Gemini 在 review 阶段只负责审查、总结、指出问题，不直接修改代码
6. 如发现同一问题连续失败 2 次，或连续 2 次得到相似错误结果，必须记录 escalation
7. 如需要跨模块重设计、影响范围不清或任务超出授权范围，也必须记录 escalation

记录 escalation 时至少包含：
- 当前任务简称
- 当前 agent
- 问题描述
- 已尝试方案
- 当前错误信息
- 涉及文件
- 升级给谁
- 希望接手方完成什么
