# 快捷键

使用键盘和鼠标快捷键可以在 scrcpy 窗口上执行操作。

在以下列表中，<kbd>MOD</kbd> 是快捷键修饰符。默认情况下，它是（左）<kbd>Alt</kbd> 或者（左）<kbd>Super</kbd> 键。

可以使用 `--shortcut-mod` 来更改修饰键。可能的键有 `lctrl`、`rctrl`、`lalt`、`ralt`、`lsuper` 和 `rsuper`。例如：

```bash
# 使用 RCtrl 作为快捷键修饰键
scrcpy --shortcut-mod=rctrl

# 使用 LCtrl 或 LSuper 作为快捷键修饰键
scrcpy --shortcut-mod=lctrl,lsuper
```

_<kbd>[Super]</kbd> 通常是 <kbd>Windows</kbd> 键或 <kbd>Cmd</kbd> 键。_

| 动作                                          |   快捷键
 | ------------------------------------------- |:-----------------------------
| 切换全屏模式                                 | <kbd>MOD</kbd>+<kbd>f</kbd>
| 向左旋转显示                                 | <kbd>MOD</kbd>+<kbd>←</kbd> _(左)_
| 向右旋转显示                                 | <kbd>MOD</kbd>+<kbd>→</kbd> _(右)_
| 水平翻转显示                                 | <kbd>MOD</kbd>+<kbd>Shift</kbd>+<kbd>←</kbd> _(左)_ \| <kbd>MOD</kbd>+<kbd>Shift</kbd>+<kbd>→</kbd> _(右)_
| 垂直翻转显示                                 | <kbd>MOD</kbd>+<kbd>Shift</kbd>+<kbd>↑</kbd> _(上)_ \| <kbd>MOD</kbd>+<kbd>Shift</kbd>+<kbd>↓</kbd> _(下)_
| 暂停或重新暂停显示                           | <kbd>MOD</kbd>+<kbd>z</kbd>
| 恢复显示                                     | <kbd>MOD</kbd>+<kbd>Shift</kbd>+<kbd>z</kbd>
| 调整窗口大小为1:1（像素完美）                 | <kbd>MOD</kbd>+<kbd>g</kbd>
| 调整窗口大小以去除黑边                       | <kbd>MOD</kbd>+<kbd>w</kbd> \| _双击¹_
| 点击 `HOME`                                  | <kbd>MOD</kbd>+<kbd>h</kbd> \| _中键点击_
| 点击 `BACK`                                  | <kbd>MOD</kbd>+<kbd>b</kbd> \| <kbd>MOD</kbd>+<kbd>Backspace</kbd> \| _右键²_
| 点击 `APP_SWITCH`                            | <kbd>MOD</kbd>+<kbd>s</kbd> \| _第四键³_
| 点击 `MENU`（解锁屏幕）⁴                     | <kbd>MOD</kbd>+<kbd>m</kbd>
| 点击 `VOLUME_UP`                             | <kbd>MOD</kbd>+<kbd>↑</kbd> _(上)_
| 点击 `VOLUME_DOWN`                           | <kbd>MOD</kbd>+<kbd>↓</kbd> _(下)_
| 点击 `POWER`                                 | <kbd>MOD</kbd>+<kbd>p</kbd>
| 开机                                        | _右键²_
| 关闭设备屏幕（保持镜像）                     | <kbd>MOD</kbd>+<kbd>o</kbd>
| 打开设备屏幕                                 | <kbd>MOD</kbd>+<kbd>Shift</kbd>+<kbd>o</kbd>
| 旋转设备屏幕                                 | <kbd>MOD</kbd>+<kbd>r</kbd>
| 展开通知面板                                 | <kbd>MOD</kbd>+<kbd>n</kbd> \| _第五键³_
| 展开设置面板                                 | <kbd>MOD</kbd>+<kbd>n</kbd>+<kbd>n</kbd> \| _双击第五键³_
| 折叠面板                                     | <kbd>MOD</kbd>+<kbd>Shift</kbd>+<kbd>n</kbd>
| 复制到剪贴板⁵                                | <kbd>MOD</kbd>+<kbd>c</kbd>
| 剪切到剪贴板⁵                                | <kbd>MOD</kbd>+<kbd>x</kbd>
| 同步剪贴板并粘贴⁵                            | <kbd>MOD</kbd>+<kbd>v</kbd>
| 注入计算机剪贴板文本                          | <kbd>MOD</kbd>+<kbd>Shift</kbd>+<kbd>v</kbd>
| 打开键盘设置（仅限 HID 键盘）                 | <kbd>MOD</kbd>+<kbd>k</kbd>
| 启用/禁用 FPS 计数器（在标准输出上）          | <kbd>MOD</kbd>+<kbd>i</kbd>
| 捏合缩放/旋转                                | <kbd>Ctrl</kbd>+_点击并移动_
| 倾斜（用两根手指垂直滑动）                    | <kbd>Shift</kbd>+_点击并移动_
| 拖放 APK 文件                               | 从计算机安装 APK
| 拖放非 APK 文件                             | [将文件推送到设备](control.md#push-file-to-device)

_¹双击黑边以去除它们。_  
_²右键会在屏幕关闭时打开它，否则按 BACK。_  
_³如果您的鼠标有第四和第五个按钮。_  
_⁴对于开发中的 React Native 应用程序，`MENU` 触发开发菜单。_  
_⁵仅在 Android >= 7 上可用。_

重复按键的快捷键通过释放并再次按下该键来执行。例如，要执行“展开设置面板”：

1. 按住 <kbd>MOD</kbd> 键。
2. 然后双击 <kbd>n</kbd> 键。
3. 最后释放 <kbd>MOD</kbd> 键。

所有 <kbd>Ctrl</kbd>+_键_ 的快捷键都将转发到设备，因此它们由活动应用程序处理。
