# 控制

## 只读模式

要禁用控制（所有与设备交互的操作：输入按键、鼠标事件、拖放文件）：

```bash
scrcpy --no-control
scrcpy -n   # 简短版本
```

## 键盘和鼠标

阅读 [键盘](keyboard.md) 和 [鼠标](mouse.md) 文档。

## 仅控制

要在不进行镜像的情况下控制设备：

```bash
scrcpy --no-video --no-audio
```

默认情况下，如果禁用视频镜像，则鼠标模式切换到 UHID（需要相对鼠标模式）。

要同时使用 UHID 键盘，请明确设置：

```bash
scrcpy --no-video --no-audio --keyboard=uhid
scrcpy --no-video --no-audio -K  # 简短版本
```

要改用 AOA（仅限 USB）：

```bash
scrcpy --no-video --no-audio --keyboard=aoa --mouse=aoa
```

## 复制粘贴

任何时候 Android 剪贴板发生变化，它会自动同步到计算机剪贴板。

任何 <kbd>Ctrl</kbd> 快捷键都会转发到设备。特别是：
- <kbd>Ctrl</kbd>+<kbd>c</kbd> 通常用于复制
- <kbd>Ctrl</kbd>+<kbd>x</kbd> 通常用于剪切
- <kbd>Ctrl</kbd>+<kbd>v</kbd> 通常用于粘贴（在计算机到设备剪贴板同步后）

这通常按预期工作。

实际行为取决于活动的应用程序。例如，_Termux_ 会在 <kbd>Ctrl</kbd>+<kbd>c</kbd> 时发送 SIGINT，而 _K-9 Mail_ 则会撰写新邮件。

在这种情况下（仅支持 Android >= 7），可以使用以下方法进行复制、剪切和粘贴：
- <kbd>MOD</kbd>+<kbd>c</kbd> 注入 `COPY`
- <kbd>MOD</kbd>+<kbd>x</kbd> 注入 `CUT`
- <kbd>MOD</kbd>+<kbd>v</kbd> 注入 `PASTE`（在计算机到设备剪贴板同步后）

此外，<kbd>MOD</kbd>+<kbd>Shift</kbd>+<kbd>v</kbd> 将计算机剪贴板文本作为一系列按键事件注入。这在组件不接受文本粘贴时非常有用（例如在 _Termux_ 中），但可能会破坏非 ASCII 内容。

**警告：** 将计算机剪贴板粘贴到设备（通过 <kbd>Ctrl</kbd>+<kbd>v</kbd> 或 <kbd>MOD</kbd>+<kbd>v</kbd>）会将内容复制到 Android 剪贴板。因此，任何 Android 应用都可以读取其内容。应避免以这种方式粘贴敏感内容（如密码）。

某些 Android 设备在通过编程方式设置设备剪贴板时行为异常。提供了一个选项 `--legacy-paste` 来更改 <kbd>Ctrl</kbd>+<kbd>v</kbd> 和 <kbd>MOD</kbd>+<kbd>v</kbd> 的行为，使它们也将计算机剪贴板文本作为按键事件序列注入（与 <kbd>MOD</kbd>+<kbd>Shift</kbd>+<kbd>v</kbd> 相同）。

要禁用自动剪贴板同步，请使用 `--no-clipboard-autosync`。

## 模拟捏合缩放、旋转和倾斜

要模拟“捏合缩放”：<kbd>Ctrl</kbd>+_单击并移动_。

更准确地说，在按下左键的同时按住 <kbd>Ctrl</kbd> 键。在释放左键之前，所有鼠标移动都会相对于屏幕中心进行缩放和旋转（如果应用程序支持）。

[视频演示](https://github.com/Genymobile/scrcpy/assets/543275/26c4a920-9805-43f1-8d4c-608752d04767)

要模拟倾斜手势：<kbd>Shift</kbd>+_单击并上下移动_。

[视频演示](https://github.com/Genymobile/scrcpy/assets/543275/1e252341-4a90-4b29-9d11-9153b324669f)

技术上，_scrcpy_ 从“虚拟手指”位置生成额外的触摸事件，该位置通过屏幕中心反转。当按下 <kbd>Ctrl</kbd> 键时，_x_ 和 _y_ 坐标反转。仅使用 <kbd>Shift</kbd> 键则仅反转 _x_ 坐标。

这仅适用于默认鼠标模式（`--mouse=sdk`）。

## 文件拖放

### 安装 APK

要安装 APK，请将 APK 文件（以 `.apk` 结尾）拖放到 _scrcpy_ 窗口。

没有视觉反馈，日志会打印到控制台。

### 推送文件到设备

要将文件推送到设备上的 `/sdcard/Download/` 目录，请将（非 APK）文件拖放到 _scrcpy_ 窗口。

没有视觉反馈，日志会打印到控制台。

可以在启动时更改目标目录：

```bash
scrcpy --push-target=/sdcard/Movies/
```
