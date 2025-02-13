# 隧道

Scrcpy 设计用于镜像本地安卓设备。隧道允许连接到远程设备（例如通过互联网）。

要连接到远程设备，可以将本地的 `adb` 客户端连接到远程的 `adb` 服务器（前提是它们使用相同版本的 _adb_ 协议）。

## 远程 ADB 服务器

要连接到远程 _adb 服务器_，使服务器监听所有接口：

```bash
adb kill-server
adb -a nodaemon server start
# 保持此窗口打开
```

**警告：客户端与 _adb 服务器_ 之间的所有通信都是未加密的。**

假设该服务器可通过 192.168.1.2 访问。然后，从另一个终端运行 `scrcpy`：

```bash
# 在 bash 中
export ADB_SERVER_SOCKET=tcp:192.168.1.2:5037
scrcpy --tunnel-host=192.168.1.2
```

```cmd
:: 在 cmd 中
set ADB_SERVER_SOCKET=tcp:192.168.1.2:5037
scrcpy --tunnel-host=192.168.1.2
```

```powershell
# 在 PowerShell 中
$env:ADB_SERVER_SOCKET = 'tcp:192.168.1.2:5037'
scrcpy --tunnel-host=192.168.1.2
```

默认情况下，`scrcpy` 使用本地端口来建立 `adb forward` 隧道（通常为 `27183`，参见 `--port`）。也可以强制使用不同的隧道端口（在涉及更多重定向的更复杂情况下可能有用）：

```
scrcpy --tunnel-port=1234
```

## SSH 隧道

为了安全地与远程 _adb 服务器_ 通信，最好使用 SSH 隧道。

首先，确保 _adb 服务器_ 在远程计算机上运行：

```bash
adb start-server
```

然后，建立一个 SSH 隧道：

```bash
# 本地  5038 --> 远程  5037
# 本地 27183 <-- 远程 27183
ssh -CN -L5038:localhost:5037 -R27183:localhost:27183 your_remote_computer
# 保持此窗口打开
```

从另一个终端运行 `scrcpy`：

```bash
# 在 bash 中
export ADB_SERVER_SOCKET=tcp:localhost:5038
scrcpy
```

```cmd
:: 在 cmd 中
set ADB_SERVER_SOCKET=tcp:localhost:5038
scrcpy
```

```powershell
# 在 PowerShell 中
$env:ADB_SERVER_SOCKET = 'tcp:localhost:5038'
scrcpy
```

为了避免启用远程端口转发，可以强制使用转发连接（注意 `-L` 而不是 `-R`）：

```bash
# 本地  5038 --> 远程  5037
# 本地 27183 --> 远程 27183
ssh -CN -L5038:localhost:5037 -L27183:localhost:27183 your_remote_computer
# 保持此窗口打开
```

从另一个终端运行 `scrcpy`：

```bash
# 在 bash 中
export ADB_SERVER_SOCKET=tcp:localhost:5038
scrcpy --force-adb-forward
```

```cmd
:: 在 cmd 中
set ADB_SERVER_SOCKET=tcp:localhost:5038
scrcpy --force-adb-forward
```

```powershell
# 在 PowerShell 中
$env:ADB_SERVER_SOCKET = 'tcp:localhost:5038'
scrcpy --force-adb-forward
```
