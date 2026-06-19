# Dashboard

> Standard 模式活跃任务看板。Quick Fix 不在此登记，记录在 session-log.md。
> 阶段变更必须立即更新，不允许滞后。

## 活跃工作流

| slug | 类型 | 当前阶段 | 负责 agent | 分支（可选） | 创建日期 | 阻塞 |
|------|------|---------|-----------|------------|---------|------|
| _(暂无活跃工作流)_ | | | | | | |

## 阶段状态机

```
Requirements → Planning → Development → Review → Acceptance → Done（自动归档）

异常状态：
  Blocked     —— 标注阻塞原因
  Escalating  —— 标注升级去向（Claude / Gemini）
```

## 多工作流协调

- 不同 slug 的文件天然隔离，可自由并行
- 多个 slug 依赖同一共享核心文件时，在下方标注，采用串行处理

**共享文件冲突记录：**
（无）

## 规则

- 新 Standard 工作流启动前，先在活跃工作流表新增一行
- 每次阶段变更必须更新"当前阶段"字段
- 工作流归档后从表中删除对应行（历史记录在 `docs/archive/` 中查找）
