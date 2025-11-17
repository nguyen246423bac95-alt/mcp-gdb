# 源码/符号映射提示词（flowchart）

用于把 `set sysroot`、`set substitute-path` 等路径映射关系可视化，突出宿主机与目标根文件系统的对应关系。

## 准备上下文
- 通过 `gdb_command "show sysroot"` 获取当前 sysroot。
- 通过 `gdb_command "show substitute-path"` 获取路径映射。
- 需要的话，附上 `file <binary>` 的加载路径和断点位置。

## 提示词模板
```
请根据以下 GDB 输出生成 Mermaid flowchart，展示宿主机路径 -> 目标路径 -> 调试动作的映射。
节点请写明实际路径，边上注明使用的命令（set sysroot/substitute-path/file/break）。

GDB 输出：
- show sysroot:
<粘贴结果>
- show substitute-path:
<粘贴结果>
- 其他补充（如断点位置、file 路径）
```

## 期望输出
- Mermaid flowchart 代码块，方向可选 TB/LR。
- 边上标注对应的 GDB 命令，便于快速还原配置。
