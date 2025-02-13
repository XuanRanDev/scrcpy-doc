# 设备

在运行 `scrcpy` 时，某些命令行参数可以对设备本身执行操作。

## 保持唤醒

为了防止设备在插入电源后经过一段时间后进入睡眠状态：

```bash
scrcpy --stay-awake
scrcpy -w
```

当关闭 `scrcpy` 时，初始状态会被恢复。

如果设备未插入电源（即仅通过 TCP/IP 连接），`--stay-awake` 不会产生任何效果（这是 Android 的行为）。

## 关闭屏幕

可以在启动镜像时通过命令行选项关闭设备屏幕：

```bash
scrcpy --turn-screen-off
scrcpy -S   # 简短版本
```

或者随时按下 <kbd>MOD</kbd>+<kbd>o</kbd>（参见 [快捷键](shortcuts.md)）。

要重新打开屏幕，按下 <kbd>MOD</kbd>+<kbd>Shift</kbd>+<kbd>o</kbd>。

在 Android 上，`POWER` 按钮始终会打开屏幕。为方便起见，如果通过 `scrcpy` 发送 `POWER`（通过右键或 <kbd>MOD</kbd>+<kbd>p</kbd>），它会尝试在一段小延迟后关闭屏幕（尽力而为）。物理 `POWER` 按钮仍会打开屏幕。

这也可以防止设备进入睡眠状态：

```bash
scrcpy --turn-screen-off --stay-awake
scrcpy -Sw   # 简短版本
```

## 显示触摸点

对于演示文稿，显示物理触摸点（在物理设备上）可能很有用。Android 在 _开发者选项_ 中提供了此功能。

`scrcpy` 在启动时提供了一个选项来启用此功能，并在退出时恢复初始值：

```bash
scrcpy --show-touches
scrcpy -t   # 简短版本
```

请注意，它只显示物理触摸点（由设备上的手指触摸）。

## 关闭设备电源

在关闭 `scrcpy` 时关闭设备屏幕：

```bash
scrcpy --power-off-on-close
```

## 启动时不开启设备电源

默认情况下，启动时会开启设备电源。为了阻止这种行为：

```bash
scrcpy --no-power-on
```
