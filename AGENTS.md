# AGENTS.md

> 唯一权威入口。所有 agent 开工前必读。所有协作规则以本文件为准。
> CODEX.md / CLAUDE.md / GEMINI.md 仅描述各自角色的具体操作细节，不重复定义规则。

## 开工必读顺序

**每次开工必读（按顺序）：**
1. `AGENTS.md`（本文件）
2. `docs/current/WORKING_CONTEXT.md`（当前开工地图）
3. `docs/architecture/map.md`（系统模块地图）

**动手前追加读（按任务类型）：**
- 新增组件 / UI 元素 → `docs/architecture/components.md`
- 新增功能 / 模块 → `docs/architecture/capabilities.md`
- 修改已有代码 → `docs/architecture/do-not-break.md`
- 涉及用户流程 / 权限 / 计费 → `docs/architecture/gates.md`

**按需读（有背景需求时）：**
- `docs/context.md`（术语、长期决策、计费语义）
- `docs/current/tasks.md`（Standard 模式任务列表）
- `docs/current/<slug>/requirements.md`

## 任务模式声明

**开工第一步：锁定当前任务模式。** 不声明模式就开始执行是禁止的。

| 模式 | 说明 |
|------|------|
| 只梳理，不修改 | 分析现状、梳理问题、给出结论，不改任何文件 |
| 深度分析，给方案 | 调研、分析、输出方案建议，不实现 |
| 写任务卡，不开发 | 拆解任务，输出 tasks.md，等用户确认后再执行 |
| 直接开发，逐项验收 | 按任务卡串行执行，完成一项验收一项 |
| 生产变更，必须完整备份 | 涉及生产数据 / 配置 / 部署，必须先备份、有回滚路径 |
| review，先找问题 | 只读审查，输出问题列表和修改建议 |
| 复盘，总结经验 | 回顾任务，输出经验总结，更新 do-not-break.md |
| Quick Fix，直接修复 | 小改动，直接修复，写 session-log |

## 两档工作流

### Quick Fix（快速通道）

**适用：** 小 bug、样式调整、文案修改、单字段、小范围逻辑修复

**判断标准（满足所有条件才走 Quick Fix）：**
- 影响范围限于 1-3 个文件
- 不涉及支付、积分、权限、登录流程
- 不需要 Grill 需求澄清
- 改动可在 30 分钟内完成

**必须做的四件事：**
1. 读 AGENTS.md + WORKING_CONTEXT.md + 当前相关源码
2. 修改前说明影响范围（哪些文件、哪些用户路径）
3. 修改后跑最小验证
4. 写一条到 `docs/current/session-log.md`，原子提交 main

**不需要：** slug / 分支 / requirements / review / acceptance

**session-log 记录格式：**
```
## YYYY-MM-DD
- 修复：[一句话描述]
- 修改文件：[文件路径]
- 验证：[做了什么验证，结果如何]
- 风险：[无 / 有（描述）]
```

### Standard（标准流程）

**适用：** 完整功能、跨模块改动、多轮持续迭代的需求

**流程：**
```
Claude Grill 需求 → slug 注册 → tasks 拆分 → 执行 → review → acceptance → 归档
```

**文档：**
- `docs/current/WORKING_CONTEXT.md`（持续更新当前状态）
- `docs/current/tasks.md`（任务列表）
- `docs/current/acceptance.md`（验收标准）
- `docs/current/<slug>/`（仅多轮持续需求建独立目录，单次需求用公用文件）

详见 `docs/workflows/standard.md`。

## Git 规范

### Main 线工作纪律（默认模式）

在 main 上开发时，必须：
1. 开工前确认 git 状态干净（无未提交改动）
2. 每次只处理一个目标
3. 大改前先 commit 一个稳定基线
4. 改完跑最小验证再提交
5. commit message 写业务意图，不写"fix bug" / "update"

### 开分支的触发条件（满足任一才开）

- 改动预计超过 1 天
- 同时有两个方向并行
- 涉及支付、扣费、登录、数据迁移、部署
- 需要独立 review 后才能合并
- 改动可能推翻现有架构
- 想保留 main 随时可发布

### 分支命名（Standard 需要分支时）
- feature 分支：`feat/<slug>`
- 合并：`feat/<slug>` → `main`，Claude 验收通过后执行

## AI 常见错误（必须避免）

1. **凭记忆开发** — 没读真实代码就动手，在老项目里极危险
2. **局部通过当整体通过** — 单测过了 ≠ 功能好了，本地能跑 ≠ 线上没问题
3. **忽略生产语义** — 失败不扣费、重试不结算、人工操作不伪造为充值
4. **顺手扩大范围** — 只改 A 结果碰了 B，只改文案结果改了逻辑
5. **不尊重已有工作区** — 把现有改动一起格式化、覆盖、提交
6. **只看命令退出码** — exit code 0 ≠ 业务正确，要看响应体、DB、日志、账务
7. **用文案判断业务状态** — 文案会变，应靠 code / status / type / metadata 判断
8. **忘记历史数据** — 新逻辑没问题，但旧数据、重试任务、历史 snapshot 可能崩
9. **任务完成不写回文档** — 下一轮 Codex 重新失忆，等于任务没完成
10. **生产操作太自信** — 没备份、没回滚路径、没 gate 检查就直接改

## 文档写回责任矩阵

| 情况 | 写回位置 | 负责 |
|------|---------|------|
| Quick Fix 完成 | `docs/current/session-log.md` | Codex |
| 新增 UI 组件 / 弹窗 / 表单 | `docs/architecture/components.md` | Codex |
| 新增模块 / 功能入口 | `docs/architecture/map.md` | Codex |
| 非常规实现 / 看起来奇怪但不能改的逻辑 | `docs/architecture/do-not-break.md` | Codex / Claude |
| 新增业务闸口 | `docs/architecture/gates.md` | Codex |
| 新增系统能力 | `docs/architecture/capabilities.md` | Codex |
| 新术语 / 计费规则 / 长期决策 | `docs/context.md` | Claude |
| Standard 任务推进 | `docs/current/tasks.md` 状态 + `WORKING_CONTEXT.md` | Codex |
| 需求澄清决策 | `docs/current/<slug>/requirements.md` | Claude |
| 验收结论 | `docs/current/acceptance.md` 或 `<slug>/acceptance.md` | Claude |
| 归档完成 | `_dashboard.md` 移除行 + `context.md` 补录 | Claude |

## Agent 角色与职责

| Agent | 定位 | 核心职责 |
|-------|------|---------|
| Claude | 需求 + 验收 | Grill 需求、需求文档、escalation 接管、最终验收、归档授权 |
| Codex | 规划 + 执行 | 任务拆分、开发实现、文档写回、验证、架构记忆维护 |
| Gemini | 分析 + review | 只读审查、调研分析、输出 review 结论（严禁修改任何文件）|

在被明确指定时，各 agent 可协助其他合理任务，但必须遵守当前边界与流程。

## 升级规则（Escalation）

满足任一条件必须升级，不尝试第三次：
- 同一问题连续失败 2 次
- 需要跨模块重新设计
- 影响范围超出当前 slug

升级路径：
- 架构 / 复杂 bug / 重规划 → Claude
- 大上下文分析 / 影响面评估 → Gemini
- 分析完成后，修复动作默认回到 Codex

升级时必须立即在 `review.md` 或 `WORKING_CONTEXT.md` 写入：当前问题、已尝试方案、错误信息、涉及文件、升级给谁、希望完成什么。

## 归档规则

Standard 任务验收通过后自动触发：
- `docs/current/<slug>/` → `docs/archive/<YYYY-MM-DD>-<slug>/`
- 从 `docs/current/_dashboard.md` 移除该行
- 补录 `docs/context.md`（本轮新决策）
- 不覆盖历史归档

## 禁止事项

- 不声明任务模式就开始执行
- 不读架构文档就新增组件或功能
- 未明确 slug 就创建分支或修改文件
- 跳过 review 直接合并（Standard 模式需要 review 时）
- 让文档状态滞后于实际进展
- 生产操作前没有备份和回滚路径
- 顺手修改任务边界外的文件
- Gemini 修改任何文件（严禁）
- 连续失败 2 次仍继续尝试（必须升级）
