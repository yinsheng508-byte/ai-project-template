# acceptance-checklist

## 最终只需要看这几件事

### 1. 任务目标是否真的完成
- 需求里说要做的功能，有没有做出来？
- 页面或接口是不是能正常用？

### 2. 是否超范围
- 这次明明只改一个功能，却顺带动了很多别的东西？
- 改动文件是否明显超出预期？

### 3. reviewer 是否放行
- reviewer 是否说"可以合并"？
- 有没有"必须修复"项？

### 4. 机器检查是否通过
- lint / test / build 是否通过？

### 5. 是否存在重大风险
- 有没有提示"可能影响旧功能"？
- 有没有说明"尚未验证"？

### 6. context.md 是否已更新
- 本轮确认的新术语是否已写入 `docs/context.md`？
- 本轮的重大决策是否已记录？

### 7. merge 授权
- [ ] Claude 已在 `acceptance.md` 中明确填写"授权合并"
- [ ] `main` 当前无冲突风险

### 8. 部署确认（仅线上项目，本地项目跳过）
- [ ] 用户已 HITL 确认部署
- [ ] OPS 健康检查通过
- [ ] 用户确认线上功能正常

## 不要合并的情况
- reviewer 明确说不建议合并
- 同一问题还没修干净
- lint / test / build 没过
- 改动超范围明显
- 风险无法判断
- `acceptance.md` 中无明确 merge 授权

## 合并后自动归档
合并完成后无需等待指令，自动执行：
1. `docs/current/<slug>/` → `docs/archive/<YYYY-MM-DD>-<slug>/`
2. `_dashboard.md` 移除该 slug 行
3. `docs/context.md` 补录本轮新决策
