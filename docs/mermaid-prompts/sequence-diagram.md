# MCP + GDB 时序图提示词

用于把 `gdb_command` 交互（如 `target remote`、断点、继续运行）转成 Mermaid `sequenceDiagram`。

## 准备上下文
- 在 Kilo Code 里用 `gdb_start` 启动（可指定交叉 GDB）。
- 记录关键 `gdb_command` 调用及输出：`target remote`, `set sysroot`, `set substitute-path`, `break`, `continue` 等。
- 如果经过远程下载或 reset，请包含命令顺序。

## 提示词模板
```
请根据以下 mcp-gdb 交互记录生成 Mermaid sequenceDiagram，节点包含 Editor(Kilo Code)、MCP、GDB（注明主机或交叉 GDB）、Target（gdbserver/设备）。
用箭头展示命令/响应顺序，保持与日志一致。

交互日志：
<粘贴 gdb_command 调用与 GDB 回显，例如 target remote、set sysroot、break main、continue 等>
```

## 期望输出
- Mermaid 代码块，节点名参照模板；可额外标记端口/IP。
- 保留命令顺序，体现启动、连接、加载符号、运行等阶段。
