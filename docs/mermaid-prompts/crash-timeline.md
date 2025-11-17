# 断点/异常时间线提示词（timeline/flowchart）

用于把崩溃或关键事件的发生顺序与调用栈摘录映射成 Mermaid 时间线或流程图。

## 准备上下文
- 命中断点/信号的输出：`Breakpoint N`、`Program received signal` 等。
- 对应时间戳（如果有日志或 `printf`），或事件顺序编号。
- 关键 backtrace 片段、`print` 结果，用于标注节点。

## 提示词模板
```
请根据以下 mcp-gdb 调试记录，生成 Mermaid timeline（如不支持可用 flowchart）来展示事件顺序。
每个节点标注时间/顺序号、事件名称、可选的调用栈摘录。

记录：
<粘贴断点/信号输出、时间戳、bt 摘要、print 结果>
```

## 期望输出
- Mermaid timeline 代码块；若渲染不支持，可退化为 flowchart TB。
- 节点包含事件描述，必要时附加行号/函数名，帮助复现。
