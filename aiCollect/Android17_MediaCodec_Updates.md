# Android 17 MediaCodec & 编解码新特性

> **注意**：Android 17 (API 37) 相关内容基于开发者预览版，正式发布时可能有变化。

## 参考链接

- [MediaCodec](https://developer.android.google.cn/reference/android/media/MediaCodec)
- [MediaFormat](https://developer.android.google.cn/reference/android/media/MediaFormat)
- [MediaCodecList](https://developer.android.google.cn/reference/android/media/MediaCodecList)
- [MediaRecorder](https://developer.android.google.cn/reference/android/media/MediaRecorder)
- [AudioManager](https://developer.android.google.cn/reference/android/media/AudioManager)

本文档梳理 Android 17 中与编解码、图像相关的重点新特性，每个特性包含 **Why**（解决什么问题）和 **How**（如何使用）两部分。

---

## 1. HE-AAC 软件编码器

### Why

在低带宽场景下（如网络不稳定、流量受限环境），传统 AAC 编码器的音频质量与比特率之间的平衡难以兼顾。此外，不同内容和平台的响度差异可能导致播放时音量忽大忽小，用户体验不佳。

Android 17 引入了 `c2.android.xheaac.encoder` 软件编码器，专门针对低带宽条件优化，支持高、低两种比特率模式，并强制支持响度元数据（loudness metadata），确保不同内容在不同设备上播放音量一致。

### How

```kotlin
// 通过 MediaCodecList 查询编码器能力
val mediaCodecList = MediaCodecList(MediaCodecList.REGULAR_CODECS)
val codecName = mediaCodecList.findEncoderForFormat(
    MediaFormat.createAudioFormat(MediaFormat.MIMETYPE_AUDIO_AAC, 44100, 2)
)

// 配置 HE-AAC 编码参数
val format = MediaFormat.createAudioFormat(MediaFormat.MIMETYPE_AUDIO_AAC, 44100, 2).apply {
    setInteger(MediaFormat.KEY_BIT_RATE, 64000)  // 低比特率模式
    setInteger(MediaFormat.KEY_AAC_PROFILE, MediaCodecInfo.CodecProfileLevel.AACObjectLC)
}

val encoder = MediaCodec.createByCodecName(codecName)
encoder.configure(format, null, null, MediaCodec.CONFIGURE_FLAG_ENCODE)
encoder.start()
```

**关键 API**:
- `c2.android.xheaac.encoder` - 新的系统级软件编码器名称
- 通过标准 `MediaCodec` API 使用，无需额外依赖

---

## 2. 视频流可靠性提升 - Temporal Layering 支持

### Why

Temporal Layering 是 **SVC (Scalable Video Coding) 分层编码**的一种，专注于时间维度。视频帧按时间分为不同层级（Temporal Layers），高层帧依赖低层帧。解码器在带宽受限时可以丢弃高层帧的解码而降级到低帧率（如 30fps → 15fps），而不影响整个流的可用性。

在视频流编码过程中，如果编码器配置后无法确认 temporal layering 支持，解码端可能无法正确获取帧依赖元数据，导致丢帧、卡顿等问题。Android 17 修复了 Issue #306222291，允许开发者在编码器配置后通过 `getOutputFormat` 确认 temporal layering 支持，从而确保视频流可靠性。

**SVC 分层类型对比**：

| 分层类型 | 作用 |
|---------|------|
| Temporal layering | 丢弃高层时间帧 → 降低帧率，保持基本层完整 |
| Spatial layering | 不同分辨率分层 → 适应不同屏幕尺寸 |
| Quality layering | 不同质量/比特率分层 → 适应网络条件 |

### How

```kotlin
val encoder = MediaCodec.createEncoderByType(MediaFormat.MIMETYPE_VIDEO_AVC)
val inputFormat = MediaFormat.createVideoFormat(MediaFormat.MIMETYPE_VIDEO_AVC, 1920, 1080).apply {
    setInteger(MediaFormat.KEY_COLOR_FORMAT, MediaCodecInfo.CodecCapabilities.COLOR_FormatSurface)
    setInteger(MediaFormat.KEY_BIT_RATE, 5_000_000)
    setInteger(MediaFormat.KEY_FRAME_RATE, 30)
    setInteger(MediaFormat.KEY_I_FRAME_INTERVAL, 1)
}

encoder.configure(inputFormat, null, null, MediaCodec.CONFIGURE_FLAG_ENCODE)
encoder.start()

// Android 17: 配置后可获取 outputFormat 确认 temporal layering 支持
val outputFormat = encoder.outputFormat
val temporalLayering = outputFormat.getInteger("temporal-layering", 0)

// 确保支持后才开始输入帧
if (temporalLayering > 0) {
    // temporal layering 已启用，可正常使用
}
```

**关键 API**:
- `MediaCodec.getOutputFormat()` - 在编码器配置后调用，确认 temporal layering 元数据
- 新增 `"temporal-layering"` 键值，可获取 temporal layers 数量

---

## 3. 视频录制恒定质量（CQ）模式

### Why

传统的视频编码通常使用 CBR（恒定比特率）或 VBR（可变比特率）模式，但在某些场景下（如屏幕录制、视频通话），开发者更关注的是保持稳定的画质而非特定的比特率。Android 17 在 `MediaRecorder` 中引入了恒定质量（Constant Quality, CQ）模式，让编码器自动调整比特率以维持目标画质。

### How

```kotlin
val mediaRecorder = MediaRecorder().apply {
    setAudioSource(MediaRecorder.AudioSource.MIC)
    setVideoSource(MediaRecorder.VideoSource.SURFACE)
    setOutputFormat(MediaRecorder.OutputFormat.MPEG_4)
    setAudioEncoder(MediaRecorder.AudioEncoder.AAC)
    setVideoEncoder(MediaRecorder.VideoEncoder.H264)
    setVideoEncodingQuality(1)  // 配置恒定质量模式
    setOutputFile(outputFile)
    prepare()
}

mediaRecorder.start()
```

**关键 API**:
- `MediaRecorder.setVideoEncodingQuality(int quality)` - 配置视频编码质量级别（1 为最高质量）
- CQ 模式下编码器自动调节比特率以保持目标画质

---

## 4. VVC (Versatile Video Coding / H.266) 支持

### Why

VVC（又称 H.266）是新一代视频编码标准，相比 H.265/HEVC 在相同画质下可节省约 30-50% 的带宽，或在相同带宽下提供更好的画质。随着高清/4K 视频、AR/VR 应用的普及，对更高效视频编码的需求日益增长。Android 17 正式引入 VVC 平台支持，为未来超高清视频应用铺路。

### How

```kotlin
// 查询设备是否支持 VVC 解码
val mediaCodecList = MediaCodecList(MediaCodecList.REGULAR_CODECS)
val vvcDecoderName = mediaCodecList.findDecoderForFormat(
    MediaFormat.createVideoFormat(MediaFormat.MIMETYPE_VIDEO_VVC, 1920, 1080)
)

if (vvcDecoderName != null) {
    val decoder = MediaCodec.createByCodecName(vvcDecoderName)
    val format = MediaFormat.createVideoFormat(MediaFormat.MIMETYPE_VIDEO_VVC, 1920, 1080)
    decoder.configure(format, surface, null, 0)
    decoder.start()
}
```

**关键常量**:
- `MediaFormat.MIMETYPE_VIDEO_VVC` - VVC 视频格式 MIME 类型
- 通过 `MediaCodecList.findDecoderForFormat()` / `findEncoderForFormat()` 查询编解码支持

---

## 5. 后台音频加固（Background Audio Hardening）

### Why

以往应用在后台播放音频时，即使应用生命周期已经进入无效状态（如 onStop 后），音频 API 仍可能正常调用，导致音频焦点混乱、资源占用、电池消耗等问题。Android 17 加固了后台音频限制：当应用不在有效生命周期状态时，音频播放、音量更改 API 将静默失败，而不抛出异常或打印日志。

这一改变确保用户交互必须是主动、有意的，防止后台应用意外播放音频干扰当前使用场景。

### How

```kotlin
// Android 17 后台音频行为
// 场景1: 应用在后台且无有效前台服务
try {
    val audioManager = getSystemService(AudioManager::class.java)
    audioManager.requestAudioFocus(AudioFocusRequest.Builder(AudioManager.AUDIOFOCUS_GAIN_TRANSIENT)
        .build())
    // 在无效生命周期下会静默返回 AUDIOFOCUS_REQUEST_FAILED，不抛异常
} catch (e: Exception) {
    // Android 17+ 不会进入此处，失败时静默返回
}

// 场景2: 正确的后台音频播放方式 - 使用前台服务
class AudioPlaybackService : Service() {
    override fun onCreate() {
        super.onCreate()
        val notification = Notification.Builder(this, CHANNEL_ID)
            .setContentTitle("Playing audio")
            .build()

        val wiuCapabilities = Notification.FOREGROUND_SERVICE_TYPE_MEDIA_PLAYBACK or
                              Notification.FOREGROUND_SERVICE_TYPE_USAGE_LOCATION

        startForeground(
            NOTIFICATION_ID,
            notification,
            wiuCapabilities  // 必须声明 while-in-use 类型
        )
    }
}
```

**关键变更**:
- 音频播放/音量 API 在无效生命周期下静默失败
- 目标 API 37+ 需要前台服务 + WIU capability 或 exact alarm 权限
- 建议使用 `AudioManager.AUDIOFOCUS_REQUEST_FAILED` 返回码判断失败

---

## 附录：相关常量

| 常量/类 | 说明 |
|--------|------|
| `c2.android.xheaac.encoder` | HE-AAC 软件编码器名称 |
| `MediaFormat.MIMETYPE_VIDEO_VVC` | VVC 视频格式 MIME |
| `MediaFormat.MIMETYPE_AUDIO_AAC` | AAC 音频格式 MIME |
| `MediaCodecInfo.CodecProfileLevel.AACObjectLC` | AAC LC  profile |
| `Notification.FOREGROUND_SERVICE_TYPE_MEDIA_PLAYBACK` | 媒体播放前台服务类型 |
| `AUDIOFOCUS_REQUEST_FAILED` | 音频焦点请求失败返回码 |