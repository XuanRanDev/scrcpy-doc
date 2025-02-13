# 视频

## 来源

默认情况下，scrcpy会镜像设备屏幕。

也可以捕获设备的摄像头。

请参阅专门的[摄像头](camera.md)页面。

## 尺寸

默认情况下，scrcpy会尝试以安卓设备的分辨率进行镜像。

可以选择以较低的分辨率进行镜像以提高性能。要限制宽度和高度的最大值（此处为1024）：

```bash
scrcpy --max-size=1024
scrcpy -m 1024   # 简写
```

另一维度会根据安卓设备的纵横比进行计算。这样，分辨率为1920×1080的设备会以1024×576进行镜像。

如果编码失败，scrcpy会自动尝试以较低的分辨率进行镜像（除非启用了`--no-downsize-on-error`）。

## 比特率

默认的视频比特率为8 Mbps。要更改它：

```bash
scrcpy --video-bit-rate=2M
scrcpy --video-bit-rate=2000000  # 等效
scrcpy -b 2M                     # 简写
```

## 帧率

可以限制捕获帧率：

```bash
scrcpy --max-fps=15
```

实际的捕获帧率可以打印到控制台：

```bash
scrcpy --print-fps
```

可以随时通过<kbd>MOD</kbd>+<kbd>i</kbd>启用或禁用它（参见[快捷键](shortcuts.md)）。

帧率本质上是可变的：仅当屏幕内容发生变化时才会生成新帧。例如，如果设备上播放全屏视频为24fps，则在scrcpy中不应超过24帧每秒。

## 编解码器

可以选择视频编解码器。可能的值为 `h264`（默认）、`h265`和 `av1`：

```bash
scrcpy --video-codec=h264  # 默认
scrcpy --video-codec=h265
scrcpy --video-codec=av1
```

H265可能提供更好的质量，但H264应提供较低的延迟。AV1编解码器在当前的安卓设备上不常见。

对于高级用法，要传递任意参数给[`MediaFormat`]，请查看man页中的`--video-codec-options`或运行`scrcpy --help`。

[`MediaFormat`]: https://developer.android.com/reference/android/media/MediaFormat

## 编码器

设备上可能有多个编码器可用。可以通过以下命令列出它们：

```bash
scrcpy --list-encoders
```

有时，默认编码器可能会出现问题甚至崩溃，因此尝试另一个编码器是有用的：

```bash
scrcpy --video-codec=h264 --video-encoder='OMX.qcom.video.encoder.avc'
```

## 方向

方向可以在3个不同的层级应用：
- 快捷键<kbd>MOD</kbd>+<kbd>r</kbd>请求设备在纵向和横向之间切换（当前运行的应用可能会拒绝请求的方向，如果它不支持该方向）。
- `--lock-video-orientation`更改镜像方向（从设备发送到电脑的视频方向）。这会影响录制。
- `--orientation`应用于客户端侧，影响显示和录制。对于显示，可以使用[快捷键](shortcuts.md)动态更改。

要锁定镜像方向（在捕获端）：

```bash
scrcpy --lock-video-orientation      # 初始（当前）方向
scrcpy --lock-video-orientation=0    # 自然方向
scrcpy --lock-video-orientation=90   # 顺时针90°
scrcpy --lock-video-orientation=180  # 180°
scrcpy --lock-video-orientation=270  # 顺时针270°
```

要设置视频方向（在渲染端）：

```bash
scrcpy --orientation=0
scrcpy --orientation=90       # 顺时针90°
scrcpy --orientation=180      # 180°
scrcpy --orientation=270      # 顺时针270°
scrcpy --orientation=flip0    # 水平翻转
scrcpy --orientation=flip90   # 水平翻转+顺时针90°
scrcpy --orientation=flip180  # 垂直翻转（水平翻转+180°）
scrcpy --orientation=flip270  # 水平翻转+顺时针270°
```

如果需要，可以分别设置显示和录制的方向，通过`--display-orientation`和`--record-orientation`。

旋转通过在MP4或MKV目标文件中写入显示转换来应用于录制文件。翻转不支持，因此在录制时只能使用前四个值。

## 裁剪

可以裁剪设备屏幕以仅镜像屏幕的一部分。

例如，这对于仅镜像Oculus Go的一只眼睛是有用的：

```bash
scrcpy --crop=1224:1440:0:0   # 1224x1440，偏移（0,0）
```

这些值以设备的自然方向表示（手机为纵向，平板为横向）。

如果还指定了`--max-size`，则在裁剪之后应用调整大小。

## 显示器

如果安卓设备上有多个显示器，可以选择要镜像的显示器：

```bash
scrcpy --display-id=1
```

可以通过以下命令检索显示器ID列表：

```bash
scrcpy --list-displays
```

如果设备运行至少Android 10，则可以控制辅助显示器（否则只能作为只读镜像）。

## 缓冲

默认情况下，没有视频缓冲，以获得尽可能低的延迟。

可以添加缓冲来延迟视频流并补偿抖动，以获得更流畅的播放（参见[#2464]）。

[#2464]: https://github.com/Genymobile/scrcpy/issues/2464

可以独立配置显示、[v4l2 sinks](video.md#video4linux)和[audio](audio.md#buffering)播放的缓冲。

```bash
scrcpy --display-buffer=50   # 为显示添加50ms缓冲
scrcpy --v4l2-buffer=300     # 为v4l2 sink添加300ms缓冲
scrcpy --audio-buffer=200    # 为音频播放设置200ms缓冲
```

可以同时应用：

```bash
scrcpy --display-buffer=50 --v4l2-buffer=300
```

## 无播放

可以捕获安卓设备而不在电脑上播放视频或音频。当[录制](recording.md)或[v4l2](#video4linux)启用时，此选项非常有用：

```bash
scrcpy --v4l2-sink=/dev/video2 --no-playback
scrcpy --record=file.mkv --no-playback
# 用Ctrl+C中断
```

也可以分别禁用视频和音频播放：

```bash
# 将视频发送到V4L2 sink而不播放，但保留音频播放
scrcpy --v4l2-sink=/dev/video2 --no-video-playback

# 录制视频和音频，但只播放视频
scrcpy --record=file.mkv --no-audio-playback
```

## 无视频

要完全禁用视频转发，仅转发音频：

```
scrcpy --no-video
```

## Video4Linux

请参阅专门的[Video4Linux](v4l2.md)页面。
