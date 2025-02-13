Here's the translation of the `Window` section from the `scrcpy` documentation:

# 窗口

## 禁用窗口

禁用窗口（可能对录制或仅播放音频有用）：

```bash
scrcpy --no-window --record=file.mp4
# 按 Ctrl+C 中断
```

## 标题

默认情况下，窗口标题是设备型号。可以更改为：

```bash
scrcpy --window-title='我的设备'
```

## 位置和大小

可以指定初始窗口位置和大小：

```bash
scrcpy --window-x=100 --window-y=100 --window-width=800 --window-height=600
```

## 无边框

禁用窗口装饰：

```bash
scrcpy --window-borderless
```

## 始终置顶

使窗口始终置顶：

```bash
scrcpy --always-on-top
```

## 全屏

可以直接在全屏模式下启动应用：

```bash
scrcpy --fullscreen
scrcpy -f   # 简短版本
```

然后可以使用 <kbd>MOD</kbd>+<kbd>f</kbd> 动态切换全屏模式（参见 [快捷键](shortcuts.md)）。

## 禁用屏幕保护程序

默认情况下，_scrcpy_ 不会阻止计算机上的屏幕保护程序运行。要禁用它：

```bash
scrcpy --disable-screensaver
```
