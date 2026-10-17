# gridcraft

## 场景技能开发候选

已增加 6 个领域场景技能及 1 个内嵌编辑器入口。详见 [场景目录](docs/scene-skills.md)。本地未发布候选，不代表全部场景的创作质量已验收。

## 场景操作手册

新增 `gridcraft-scenario-playbooks`：12 条带具体命令、参数与检查的流程，见 [技能入口](skills/gridcraft-scenario-playbooks/SKILL.md)。示例数据需替换，实际运行与创作效果状态见命令证据；不将目录存在当验收通过。

## 工时、分期测算及月历模板开发候选

新增三个独立技能：`gridcraft-shift-timesheet`、`gridcraft-fixed-payment`、`gridcraft-month-calendar`，参考Apache POI固定GitHub提交的Excel示例并原创适配方法、参数、输入与检查。官方0.7.0 macOS完整21记录147次CLI通过，新模板包含跨午夜/午夜0、零利率、非整数月数、闰年、缺失/非法月份的重算，20个XLSX输出保留公式、输入规则和格式。原表除活动页选择标志外XML一致，原文件摘要不变。当前未随主市场发行，Windows、真实内容、外部Excel显示及宿主同步待验证。
