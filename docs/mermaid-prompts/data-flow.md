# 数据/消息流提示词（flowchart 或 sequence）

把 `print`/`monitor`/日志输出中的关键数据流转成 Mermaid，适合 D-Bus、REST、驱动调用链等。

## 准备上下文
- 相关 GDB 命令输出：`print <var>`、`p *(struct*)ptr`、`monitor`、或调试日志片段。
- 说明数据从哪个模块流向哪里，以及触发条件（断点、命令）。

## 提示词模板
```
请将以下 gdb mcp 输出描述的数据流，转换成 Mermaid flowchart（或 sequenceDiagram，如果更适合），突出数据在组件间的传递顺序。
节点名用模块/对象，边上注明字段或调用。

数据/日志：
<粘贴 print/monitor/日志摘要>
```

## 期望输出
- Mermaid 代码块，推荐 flowchart LR/TB，必要时可改用 sequenceDiagram。
- 边上标注字段/函数名，帮助定位数据来源与去向。
