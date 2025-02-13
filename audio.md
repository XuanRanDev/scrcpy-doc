# 音频

音频转发支持 Android 11 或更高版本的设备，并且默认启用：

- 对于 **Android 12 或更新版本**，开箱即用。
- 对于 **Android 11**，启动 scrcpy 时需确保设备屏幕已解锁。一个假的弹出窗口会短暂出现，让系统认为 shell 应用在前台。否则，音频捕获将失败。
- 对于 **Android 10 或更早版本**，无法捕获音频，自动禁用音频。

如果音频捕获失败，镜像将继续仅传输视频（由于默认启用音频，不接受在音频不可用时让 scrcpy 失败），除非设置了 `--require-audio`。

## 无音频

要禁用音频：

```bash
scrcpy --no-audio
```

要仅禁用音频播放，请参见[无播放](video.md#no-playback)。

## 仅音频

要仅播放音频，禁用视频和控制：

```bash
scrcpy --no-video --no-control
```

要在没有窗口的情况下播放音频：

```bash
# --no-video 和 --no-control 被 --no-window 隐含
scrcpy --no-window
# 使用 Ctrl+C 终止
```

没有视频时，音频延迟通常不关键，因此可以考虑添加[缓冲](#buffering)以最小化故障：

```bash
scrcpy --no-video --audio-buffer=200
```

## 音频源

默认情况下，设备音频输出会被转发。

可以捕获设备麦克风：

```bash
scrcpy --audio-source=mic
```

例如，使用设备作为录音笔，直接在计算机上录制音频：

```bash
scrcpy --audio-source=mic --no-video --no-playback --record=file.opus
```

## 编解码器

可以选择音频编解码器。可能的值为 `opus`（默认）、`aac`、`flac` 和 `raw`（未压缩的 PCM 16 位 LE）：

```bash
scrcpy --audio-codec=opus  # 默认
scrcpy --audio-codec=aac
scrcpy --audio-codec=flac
scrcpy --audio-codec=raw
```

特别地，如果出现以下错误：

> Failed to initialize audio/opus, error 0xfffffffe

则您的设备没有 Opus 编码器：尝试 `scrcpy --audio-codec=aac`。

对于高级用法，请检查 `--audio-codec-options` 参数以传递任意参数给 [`MediaFormat`]，请参阅手册页或 `scrcpy --help`。

例如，改变 [FLAC 压缩级别]：

```bash
scrcpy --audio-codec=flac --audio-codec-options=flac-compression-level=8
```

[`MediaFormat`]: https://developer.android.com/reference/android/media/MediaFormat
[FLAC compression level]: https://developer.android.com/reference/android/media/MediaFormat#KEY_FLAC_COMPRESSION_LEVEL

## 编码器

设备上可能有多个编码器。可以列出它们：

```bash
scrcpy --list-encoders
```

要选择特定编码器：

```bash
scrcpy --audio-codec=opus --audio-encoder='c2.android.opus.encoder'
```

## 比特率

默认音频比特率为 128Kbps。更改方法：

```bash
scrcpy --audio-bit-rate=64K
scrcpy --audio-bit-rate=64000  # 等价
```

_此参数不适用于 RAW 音频编解码器（`--audio-codec=raw`）。_

## 缓冲

音频缓冲是不可避免的。它必须足够小以使延迟可接受，但足够大以最小化缓冲区不足（导致音频故障）。

默认缓冲大小设置为 50ms。可以调整：

```bash
scrcpy --audio-buffer=40   # 小于默认值
scrcpy --audio-buffer=100  # 大于默认值
```

请注意，此选项更改的是 _目标_ 缓冲。可能不会达到此目标缓冲（通常在频繁的缓冲区下溢时）。

如果您不与设备交互（例如观看视频），更高的延迟（对于 [视频](video.md#buffering) 和音频）可能更好，以避免故障并平滑播放：

```bash
scrcpy --display-buffer=200 --audio-buffer=200
```

还可以配置另一个音频缓冲区（音频输出缓冲区），默认设置为 5ms。除非出现[机器人和故障音][#3793]，否则不要更改：

```bash
# 仅在绝对必要时
scrcpy --audio-output-buffer=10
```

[#3793]: https://github.com/Genymobile/scrcpy/issues/3793
