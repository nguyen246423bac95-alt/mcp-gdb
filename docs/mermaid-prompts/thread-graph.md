# 线程/任务关系提示词（graph TD）

将 `info threads`、`thread apply all bt` 或关键栈帧浓缩为 Mermaid `graph TD`，说明线程职责与调度关系。

## 准备上下文
- `gdb_command "info threads"` 输出。
- 如需标注调用栈，提供 `thread apply all bt` 的摘要（挑出关键帧即可）。
- 标记线程角色（如 event-loop、worker、io），便于在图中命名。

## 提示词模板
```
请根据以下线程信息生成 Mermaid graph TD，展示线程角色与关键调用点。
节点使用 Thread N: <角色>，边上可标注调度/派发关系。

info threads:
<粘贴输出>

线程调用栈摘要（可选）：
<粘贴关键帧>
```

## 期望输出
- Mermaid graph TD 代码块，突出线程名称/ID 与核心函数。
- 派发/依赖关系可用箭头说明（如 event-loop -> worker）。
