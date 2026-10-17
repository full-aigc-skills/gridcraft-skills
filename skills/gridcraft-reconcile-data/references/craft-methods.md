# 本场景的 Craft 方法与参数

先读取输入与当前选择。数值、路径、名称和对象编号仅为格式示例；用当前输入、实际返回ID和目标尺寸替换后执行。

| 顺序与目的 | 实际方法 | 参数例子 |
| --- | --- | --- |
| 1. 读取双方主键与金额 | `sheet.read` | `{"range":"A1:D100","formulas":true}` |
| 2. 先检查主键缺失/重复，唯一匹配才提取金额 | `cell.set` | `{"cell":"E2","input":"=IF(A2=\"\",IF(B2=\"\",\"\",\"缺少主键\"),IF(COUNTIF(Sheet2!$A$2:$A$100,A2)=0,\"未匹配\",IF(COUNTIF(Sheet2!$A$2:$A$100,A2)=1,VLOOKUP(A2,Sheet2!$A$2:$B$100,2,FALSE),\"重复键\")))"}` |
| 3. 在新列计算金额差额 | `cell.set` | `{"cell":"F2","input":"=IF(AND(ISNUMBER(B2),ISNUMBER(E2)),B2-E2,\"\")"}` |
| 4. 按实际末行替换100，填充每行且固定对方数据范围 | `edit.fillDown` | `{"range":"E2:F100"}` |
| 5. 检查计算结果 | `sheet.read` | `{"range":"A2:F10"}` |

## 如何调用

宿主内已打开编辑器时使用该同会话MCP；先等待editor_status就绪，读取实际工具schema和当前命令参数。独立CLI可用于文件任务，但新进程与已打开Web编辑器不是同一状态。

本插件MCP包装：`execute_command`，参数 `{"command": "sheet.read", "params": {"range": "A1:D100", "formulas": true}}`。后续行保持同一会话和对象选择，逐条检查实际结果。

CLI 语法（下面是展示用写法，实际跨平台执行用argv避免shell引号差异）：

gridcraft-cli run --in input.xlsx --cmd 'cell.set={"cell":"B2","input":"=A2*2"}' --out output.xlsx；再用 cat/info 新进程读取。

## 参数核对与完成标准

先查 [当前版本观察目录](craft-command-evidence.json) 中对应方法的完整参数，再按实际运行时查询更新。结构化步骤见 [craft-steps.json](craft-steps.json)。

分别列出缺失、多对一和金额差异，不通过删除未匹配行让总数相等。

边界：工作表名、键列和重复键必须先查；VLOOKUP只取首条，不适合未处理的多对多。

保存可编辑工程后由新进程或重新打开的同一宿主会话核对；导出格式单独解码/查看。方法存在和零退出不代表本场景已经完成。

工作流来源与许可见 [workflow-sources.json](workflow-sources.json)。仅借鉴事实与方法，本文与参数例子原创编写。

本轮实际边界：工作表名、键列和重复键必须先查；VLOOKUP只取首条，不适合未处理的多对多。 当前示例先COUNTIF分流重复键，不使用IFERROR把重复键隐藏成首条金额。示例100替换为真实范围，双方原始键/金额及所有未匹配行保留。
