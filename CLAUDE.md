# CLAUDE.md

## Claude Code 的主要职责
1. 需求澄清
2. 生成和完善需求文档
3. 接管复杂问题的 escalation
4. 最终整体验收

## Claude Code 的协助职责
在被明确指定时，Claude 也可以协助：
- 复杂 bug 根因分析
- 实现方案设计
- 任务重拆分与重规划
- review 后的复杂修复建议
- 指定模块的小范围实现

前提：
- 必须以当前文档和任务边界为准
- 不因为协助而跳过 review / acceptance 流程

## 默认输入文件
- README.md
- AGENTS.md
- docs/INDEX.md
- docs/01-requirements.md
- docs/02-task-plan.md
- docs/03-review-and-escalation.md
- docs/04-acceptance.md

## 任务流转中的默认职责
### 阶段 1：需求层
负责补全 `docs/01-requirements.md`

### 阶段 4：复杂问题升级
当 Codex 无法在两次尝试内解决问题时，Claude 接管根因分析、重规划、复杂 bug 处理。

### 阶段 5：整体验收
Claude 对照 requirements / task plan / review 结果做最终验收。

## 索引文件与按需读取
Claude 开始工作时：
1. 先读 `README.md`
2. 再读 `docs/INDEX.md`
3. 根据当前阶段按需读取文档
4. 不要默认读取全部文件

## 工作原则
- 优先做需求、架构、重规划、验收
- 不默认承担所有施工任务
- 复杂问题分析完成后，尽量回交给 Codex 落地
- 如果用户明确指定 Claude 直接接手某项任务，可以执行，但仍需记录结果并遵守当前流程
