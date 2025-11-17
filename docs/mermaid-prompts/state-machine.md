# 会话状态机提示词（stateDiagram-v2）

把调试会话从启动到结束的状态变化提炼成 Mermaid 状态图，便于复盘或文档化自动化逻辑。

## 准备上下文
- 记录触发状态变化的命令：`gdb_start`/`gdb_terminate`、`file`/符号加载、`continue`/`next`/`step`、命中断点或信号、`detach` 等。
- 如有异常退出或崩溃，也要保留对应输出。

## 提示词模板
```
请根据以下 mcp-gdb 会话记录生成 Mermaid stateDiagram-v2。
状态建议包含：Idle/Connected/Ready/Running/Paused/Ended，可按实际增减。
标注触发状态迁移的 GDB 命令或事件（断点、信号、崩溃）。

会话记录：
<粘贴命令顺序与关键回显>
```

## 期望输出
- Mermaid 代码块，带入口/退出节点。
- 每条转换标注触发动作（命令或事件）。
