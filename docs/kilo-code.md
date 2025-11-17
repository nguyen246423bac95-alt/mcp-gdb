# 在 Kilo Code 中配置与使用 mcp-gdb

本文档说明如何在 Kilo Code（支持 MCP 的编辑器/IDE）中集成并使用本仓库提供的 GDB 服务器，包括常见的本地调试、远程 gdbserver 以及 NFS 根文件系统场景（如 OpenBMC 在 ASPEED AST2600 上的开发板）。

## 先决条件

- **Node.js**：与 Kilo Code 同机安装（与项目 `package.json` 中的版本要求一致）。
- **GDB**：
  - 本地调试使用宿主机 GDB。
  - 远程/交叉调试准备对应架构的交叉 GDB（示例：`arm-openbmc-linux-gnueabi-gdb` 或 `gdb-multiarch`）。
- **构建 mcp-gdb**：在项目根目录执行：

```bash
npm install
npm run build
```

## 在 Kilo Code 中启用 MCP GDB 服务器

Kilo Code 通过 MCP 启动外部工具。将本项目的构建产物添加到 Kilo Code 的 MCP 配置（示例路径需替换为你的实际目录）：

```json
{
  "mcpServers": {
    "gdb": {
      "command": "node",
      "args": ["/path/to/mcp-gdb/build/index.js"],
      "disabled": false
    }
  }
}
```

> 如果需要默认使用交叉 GDB，可在调用 `gdb_start` 时传入 `gdbPath`（绝对路径），例如 `"gdbPath": "/opt/toolchains/bin/arm-openbmc-linux-gnueabi-gdb"`。

保存配置后重启 Kilo Code 或重新加载工作区，使 MCP 服务器列表刷新。随后可在 MCP 面板/命令面板中调用下文的工具方法。

## 基本调试流程（本地/通用）

1. **启动会话**：调用 `gdb_start`（可选 `gdbPath`）。
2. **加载目标**：
   - 可执行文件：`gdb_load` 传入文件路径。
   - 核心转储：`gdb_load_core` 提供可执行与 core 文件。
3. **常用控制与查询**：
   - 运行/单步：`gdb_continue`、`gdb_step`、`gdb_next`、`gdb_finish`。
   - 断点：`gdb_set_breakpoint`（或用 `gdb_command` 透传 `break`）。
   - 检查：`gdb_backtrace`、`gdb_print`、`gdb_examine`、`gdb_info_registers`。
4. **执行任意 GDB 指令**：使用 `gdb_command` 透传原生命令（加载符号、修改 sysroot、远程连接等）。
5. **结束会话**：`gdb_terminate` 关闭对应 GDB 进程。

## 远程 gdbserver + NFS 根文件系统示例（OpenBMC / ASPEED AST2600）

mcp-gdb 不限制目标运行环境，通过 `gdb_command` 可直接复用终端里的调试流程。

1. **目标板启动 gdbserver**（在 BMC 上）：
   ```bash
   gdbserver :1234 /path/to/program
   ```
2. **宿主机启动会话**：在 Kilo Code 中运行 `gdb_start`，如需交叉调试传入 `gdbPath` 指向交叉 GDB。
3. **连接远程目标**：
   ```
   gdb_command: "target remote <bmc_ip>:1234"
   ```
4. **指向 NFS 根文件系统（如已挂载到宿主机 /nfsroot/obmc-rofs）**：
   ```
   gdb_command: "set sysroot /nfsroot/obmc-rofs"
   gdb_command: "set substitute-path /usr/src/debug /nfsroot/obmc-rofs/usr/src/debug"
   gdb_command: "file /nfsroot/obmc-rofs/usr/bin/your_app"
   ```
5. **调试与检查**：
   - 断点：`gdb_set_breakpoint`（或 `gdb_command: "break main"`）。
   - 运行：`gdb_continue`。
   - 状态查看：`gdb_backtrace`、`gdb_print`、`gdb_examine`、`gdb_info_registers`。
6. **结束会话**：`gdb_terminate`。

### 说明与提示

- 只要宿主机能运行 Node.js 与（交叉）GDB，并能访问目标板 gdbserver 端口，就可以在 Kilo Code 中使用 mcp-gdb。目标板无需直接运行 Node.js。
- NFS 场景下使用 `set sysroot`/`set substitute-path` 指向挂载目录，确保符号与源码能被解析。
- 早期引导/内核调试同理，先用合适的 GDB（如 `gdb-multiarch`）启动会话，再用 `gdb_command` 发送 `target remote`、`add-symbol-file` 等命令。

完成上述配置后，你可以在 Kilo Code 中通过 MCP 工具链直接驱动本地或远程的 gdbserver 调试，同时保留编辑器侧的命令记录与上下文能力。
