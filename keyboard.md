# 键盘

提供了几种键盘输入模式：

- `--keyboard=sdk`（默认）
- `--keyboard=uhid`（或 `-K`）：使用设备上的 UHID 内核模块模拟物理 HID 键盘
- `--keyboard=aoa`：使用 AOAv2 协议模拟物理 HID 键盘
- `--keyboard=disabled`

默认情况下使用 `sdk`，但如果你经常使用 scrcpy，推荐使用 [`uhid`](#uhid) 并配置键盘布局一次并永久使用。

## SDK 键盘

在这种模式下（`--keyboard=sdk`，或如果省略参数），键盘输入事件在 Android API 级别注入。它在任何地方都能工作，但仅限于 ASCII 和一些其他字符。

请注意，在某些设备上，必须在开发者选项中启用一个额外的选项才能使此键盘模式工作。参见[先决条件](README.md#prerequisites)。

下面描述的额外参数（特定于 `--keyboard=sdk`）允许自定义行为。

### 文本注入优先

输入文本时会生成两种类型的[事件][textevents]：
- _按键事件_，表示按键被按下或释放；
- _文本事件_，表示输入了一个文本。

默认情况下，数字和“特殊字符”使用文本事件插入，但字母使用按键事件注入，以便在游戏中键盘行为如预期（通常用于 WASD 键）。

但这可能会[引起问题][prefertext]。如果你遇到这样的问题，可以将字母注入为文本（或只是切换到 [UHID](#uhid)）：

```bash
scrcpy --prefer-text
```

（但这会破坏游戏中的键盘行为）

相反，你可以强制始终注入原始按键事件：

```bash
scrcpy --raw-key-events
```

[textevents]: https://blog.rom1v.com/2018/03/introducing-scrcpy/#handle-text-input
[prefertext]: https://github.com/Genymobile/scrcpy/issues/650#issuecomment-512945343

### 按键重复

默认情况下，按住按键会生成重复的按键事件。这在一些游戏中可能会导致性能问题，这些事件在游戏中无用。

要避免转发重复的按键事件：

```bash
scrcpy --no-key-repeat
```

## 物理键盘模拟

两种模式允许在设备上模拟物理 HID 键盘。

要正常工作，必须配置（一次并永久）设备上的键盘布局以匹配计算机的键盘布局。

可以通过以下方式之一打开配置页面：
- 从 scrcpy 窗口（使用 `uhid` 或 `aoa` 时），按 <kbd>MOD</kbd>+<kbd>k</kbd>（参见[快捷键](shortcuts.md)）
- 从设备上，进入设置 → 系统 → 语言和输入法 → 物理设备
- 从计算机终端，执行 `adb shell am start -a android.settings.HARD_KEYBOARD_SETTINGS`

在这个配置页面上，也可以启用或禁用屏幕键盘。

### UHID

这种模式使用设备上的 [UHID] 内核模块模拟物理 HID 键盘。

[UHID]: https://kernel.org/doc/Documentation/hid/uhid.txt

要启用 UHID 键盘，使用：

```bash
scrcpy --keyboard=uhid
scrcpy -K  # 简短版本
```

一旦配置好键盘布局（见上文），它是镜像时使用键盘的最佳模式：

- 它适用于所有字符和输入法（与 `--keyboard=sdk` 相反）
- 可以禁用屏幕键盘（与 `--keyboard=sdk` 相反）
- 它可以通过 TCP/IP（无线）工作（与 `--keyboard=aoa` 相反）
- 在 Windows 上没有问题（与 `--keyboard=aoa` 相反）

一个缺点是由于权限错误，它可能不适用于旧的 Android 版本。

### AOA

这种模式使用 [AOAv2] 协议模拟物理 HID 键盘。

[AOAv2]: https://source.android.com/devices/accessories/aoa2#hid-support

要启用 AOA 键盘，使用：

```bash
scrcpy --keyboard=aoa
```

与其他模式相反，它直接在 USB 级别工作（因此它只适用于 USB）。

它不使用 scrcpy 服务器，也不需要 `adb`（USB 调试）。因此，即使禁用 USB 调试也可以控制设备（但不能镜像）（参见[OTG](otg.md)）。

注意：在 Windows 上，它可能仅在[OTG 模式](otg.md)下工作，而在镜像时则不能（如果已经被另一个进程如 _adb 守护进程_ 打开，则无法打开 USB 设备）。
