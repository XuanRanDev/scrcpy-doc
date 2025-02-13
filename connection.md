# 连接

## 选择

如果只有一个设备连接（即通过 `adb devices` 列出），则会自动选择该设备。

但是，如果有多个设备连接，则必须通过以下四种方式之一指定要使用的设备：
- 通过其序列号：
  ```bash
  scrcpy --serial=0123456789abcdef
  scrcpy -s 0123456789abcdef   # 简写版本

  # 如果通过 TCP/IP 连接，序列号是 ip:port（与 adb 的行为相同）
  scrcpy --serial=192.168.1.1:5555
  ```
- 通过 USB 连接的设备（如果只有一个）：
  ```bash
  scrcpy --select-usb
  scrcpy -d   # 简写版本
  ```
- 通过 TCP/IP 连接的设备（如果只有一个）：
  ```bash
  scrcpy --select-tcpip
  scrcpy -e   # 简写版本
  ```
- 已在 TCP/IP 上监听的设备（见下文 [TCP/IP（无线）]）：
  ```bash
  scrcpy --tcpip=192.168.1.1:5555
  scrcpy --tcpip=192.168.1.1        # 默认端口为 5555
  ```

也可以通过环境变量 `ANDROID_SERIAL` 提供序列号（adb 也使用此变量）：

```bash
# 在 bash 中
export ANDROID_SERIAL=0123456789abcdef
scrcpy
```

```cmd
:: 在 cmd 中
set ANDROID_SERIAL=0123456789abcdef
scrcpy
```

```powershell
# 在 PowerShell 中
$env:ANDROID_SERIAL = '0123456789abcdef'
scrcpy
```


## TCP/IP（无线）

Scrcpy 使用 `adb` 与设备通信，而 `adb` 可以通过 TCP/IP [连接] 到设备。设备必须连接到与计算机相同的网络。

[connect]: https://developer.android.com/studio/command-line/adb.html#wireless


### 自动

选项 `--tcpip` 允许自动配置连接。有两种变体。

如果设备上的 adb TCP/IP 模式被禁用（或您不知道 IP 地址），请通过 USB 连接设备，然后运行：

```bash
scrcpy --tcpip   # 不带参数
```

它将自动找到设备的 IP 地址和 adb 端口，如果需要则启用 TCP/IP 模式，然后在启动前连接到设备。

如果设备（在此示例中为 192.168.1.1）已监听端口（通常为 5555）以进行传入的 adb 连接，请运行：

```bash
scrcpy --tcpip=192.168.1.1       # 默认端口为 5555
scrcpy --tcpip=192.168.1.1:5555
```


### 手动

另外，也可以使用 `adb` 手动启用 TCP/IP 连接：

1. 将设备插入计算机的 USB 端口。
2. 将设备连接到与计算机相同的 Wi-Fi 网络。
3. 获取设备的 IP 地址，进入设置 → 关于手机 → 状态，或执行以下命令：

    ```bash
    adb shell ip route | awk '{print $9}'
    ```

4. 在设备上启用 adb over TCP/IP：`adb tcpip 5555`。
5. 拔下设备。
6. 连接到设备：`adb connect DEVICE_IP:5555`（将 `DEVICE_IP` 替换为您找到的设备 IP 地址）。
7. 像往常一样运行 `scrcpy`。
8. 完成后运行 `adb disconnect`。

自 Android 11 以来，[无线调试选项][adb-wireless] 允许绕过直接将设备物理连接到计算机的步骤。

[adb-wireless]: https://developer.android.com/studio/command-line/adb#wireless-android11-command-line


## 自动启动

一个小工具（由 scrcpy 作者开发）允许在每次连接新的 Android 设备时运行任意命令：[AutoAdb]。它可用于启动 scrcpy：

```bash
autoadb scrcpy -s '{}'
```

[AutoAdb]: https://github.com/rom1v/autoadb
