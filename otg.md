# OTG（USB On-The-Go）

默认情况下，_scrcpy_ 在 Android API 级别注入输入事件。作为替代方案，可以发送 HID 事件，使 scrcpy 表现得就像连接到 Android 设备上的 [物理键盘] 和/或 [物理鼠标] 一样（参见 [键盘](keyboard.md) 和 [鼠标](mouse.md)）。

[物理键盘]: keyboard.md#physical-keyboard-simulation
[物理鼠标]: physical-keyboard-simulation

特殊模式（OTG）允许使用 AOA（[键盘](keyboard.md#aoa) 和 [鼠标](mouse.md#aoa)）控制设备，而完全不需要使用 _adb_（因此无需开启 USB 调试）。在此模式下，视频和音频被禁用，并且 `--keyboard=aoa` 和 `--mouse=aoa` 会隐式设置。

因此，可以通过 OTG 模式仅使用物理键盘和鼠标模拟来运行 _scrcpy_，就好像计算机键盘和鼠标直接通过 OTG 线缆连接到设备上一样。

要启用 OTG 模式：

```bash
scrcpy --otg
# 如果有多个 USB 设备可用，请传递序列号
scrcpy --otg -s 0123456789abcdef
```

可以禁用键盘或鼠标：

```bash
scrcpy --otg --keyboard=disabled
scrcpy --otg --mouse=disabled
```

这仅在设备通过 USB 连接时有效。

## Windows 上的 OTG 问题

请参阅 [常见问题解答](/FAQ.md#otg-issues-on-windows)。

## 仅控制模式

请注意，OTG 的目的是在没有 USB 调试（adb）的情况下控制设备。

如果希望在启用 USB 调试的同时仅控制设备而不进行镜像，则不需要使用 OTG 模式。

相反，请禁用视频和音频，并选择 UHID（或 AOA）：

```bash
scrcpy --no-video --no-audio --keyboard=uhid --mouse=uhid
scrcpy --no-video --no-audio -KM  # 简短版本
scrcpy --no-video --no-audio --keyboard=aoa --mouse=aoa
```

UHID 的一个好处是它也可以无线工作。
