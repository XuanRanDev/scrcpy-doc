# 鼠标

有几种鼠标输入模式可供选择：

- `--mouse=sdk`（默认模式）
- `--mouse=uhid`（或 `-M`）：使用设备上的 UHID 内核模块模拟物理 HID 鼠标
- `--mouse=aoa`：使用 AOAv2 协议模拟物理 HID 鼠标
- `--mouse=disabled`：禁用鼠标输入

## SDK 鼠标

在此模式下（`--mouse=sdk`，或者如果省略了参数），鼠标输入事件以绝对坐标注入到 Android API 级别。

请注意，对于一些设备，必须在开发者选项中启用额外选项才能使此鼠标模式工作。请参阅 [先决条件](/README.md#prerequisites)。

### 鼠标悬停

默认情况下，鼠标悬停（鼠标移动而不进行任何点击）事件将被转发到设备。可以使用以下命令禁用：

```
scrcpy --no-mouse-hover
```

## 物理鼠标模拟

有两种模式允许在设备上模拟物理 HID 鼠标。

在这些模式下，计算机鼠标被“捕获”：鼠标指针从计算机消失，并出现在 Android 设备上。

特殊的捕获键，如 <kbd>Alt</kbd> 或 <kbd>Super</kbd>，切换（禁用或启用）鼠标捕获。使用其中一个键将鼠标控制权还给计算机。

### UHID

此模式使用设备上的 [UHID] 内核模块模拟物理 HID 鼠标。

[UHID]: https://kernel.org/doc/Documentation/hid/uhid.txt

要启用 UHID 鼠标，请使用：

```bash
scrcpy --mouse=uhid
scrcpy -M  # 简短版本
```

### AOA

此模式使用 [AOAv2] 协议模拟物理 HID 鼠标。

[AOAv2]: https://source.android.com/devices/accessories/aoa2#hid-support

要启用 AOA 鼠标，请使用：

```bash
scrcpy --mouse=aoa
```

与其他模式相反，它直接在 USB 级别工作（因此仅通过 USB 工作）。

它不使用 scrcpy 服务器，并且不需要 `adb`（USB 调试）。因此，即使禁用了 USB 调试（请参阅 [OTG](otg.md)），也可以控制设备（但不能镜像）。

注意：在 Windows 上，它可能仅在 [OTG 模式](otg.md) 下工作，而不是在镜像时（如果 USB 设备已被其他进程如 _adb 守护进程_ 打开，则无法打开 USB 设备）。

## 鼠标绑定

默认情况下，在 SDK 鼠标模式下，右键触发 BACK（或按下 POWER），中键触发 HOME。此外，第四次点击触发 APP_SWITCH，第五次点击展开通知面板。

在 AOA 和 UHID 鼠标模式下，默认情况下所有点击都会被转发。

可以使用 `--mouse-bind=xxxx` 配置鼠标快捷键绑定，适用于任何鼠标模式。参数必须完全是 4 个字符，每个字符对应一个次要点击：

```
--mouse-bind=xxxx
             ^^^^
             ||||
             ||| `- 第 5 次点击
             || `-- 第 4 次点击
             | `--- 中键点击
              `---- 右键点击
```

每个字符必须是以下之一：

- `+`：转发点击到设备
- `-`：忽略点击
- `b`：触发 BACK 快捷键（或如果屏幕关闭则打开屏幕）
- `h`：触发 HOME 快捷键
- `s`：触发 APP_SWITCH 快捷键
- `n`：触发 "展开通知面板" 快捷键

例如：

```bash
scrcpy --mouse-bind=bhsn  # 在 SDK 鼠标模式下的默认模式
scrcpy --mouse-bind=++++  # 转发所有点击（AOA/UHID 的默认模式）
scrcpy --mouse-bind=++bh  # 转发右键和中键点击，使用第四和第五次点击作为 BACK 和 HOME
```
