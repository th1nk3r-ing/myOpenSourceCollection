# Android MediaCodec 从 Android 12 到 Android 17 新特性

> **注意**：Android 17 (API 37) 相关内容基于开发者预览版，正式发布时可能有变化。

## 参考链接

- [MediaCodec](https://developer.android.google.cn/reference/android/media/MediaCodec)
- [MediaFormat](https://developer.android.google.cn/reference/android/media/MediaFormat)
- [MediaExtractor](https://developer.android.google.cn/reference/android/media/MediaExtractor)
- [MediaMuxer](https://developer.android.google.cn/reference/android/media/MediaMuxer)
- [MediaSync](https://developer.android.google.cn/reference/android/media/MediaSync)
- [MediaCodecList](https://developer.android.google.cn/reference/android/media/MediaCodecList)
- [MediaDrm](https://developer.android.google.cn/reference/android/media/MediaDrm)
- [MediaRecorder](https://developer.android.google.cn/reference/android/media/MediaRecorder)
- [AudioManager](https://developer.android.google.cn/reference/android/media/AudioManager)
- [AudioTrack](https://developer.android.google.cn/reference/android/media/AudioTrack)
- [AudioMixerAttributes](https://developer.android.google.cn/reference/android/media/AudioMixerAttributes)
- [LoudnessCodecController](https://developer.android.google.cn/reference/android/media/audiofx/LoudnessCodecController)

本文档梳理 Android 12 (API 31) 至 Android 17 (API 37) 期间 MediaCodec 及相关编解码、图像方向的新特性，每个特性包含 **Why**（解决什么问题）和 **How**（如何使用）两部分。

---

## 附录：相关类新增 API (API 31+)

以下为 MediaCodec 及相关类新增的 public API，按 API level 标注，数据来源为官方 API reference 页面。

### MediaCodec 新增 API

| API Level | API | 说明 |
|-----------|-----|------|
| API 31 | `PARAMETER_KEY_TUNNEL_PEEK` | 控制 tunnel 模式下首帧 peek（Int，1=启用/0=禁用） |
| API 31 | `getParameterDescriptor(String)` | 获取参数描述符 |
| API 31 | `getSupportedVendorParameters()` | 获取支持的 vendor 参数列表 |
| API 31 | `subscribeToVendorParameters(List<String>)` | 订阅 vendor 参数 |
| API 31 | `unsubscribeFromVendorParameters(List<String>)` | 取消订阅 vendor 参数 |
| API 31 | `setOnFirstTunnelFrameReadyListener(Handler, OnFirstTunnelFrameReadyListener)` | 注册 tunnel 首帧就绪回调 |
| API 30 | `getQueueRequest(int)` | 获取输入槽位的 QueueRequest |
| API 30 | `mapHardwareBuffer(HardwareBuffer)` | 将 HardwareBuffer 映射为 Image |
| API 30 | `CONFIGURE_FLAG_USE_BLOCK_MODEL` | 配置 flag，支持 LinearBlock/HardwareBuffer |
| API 34 | `CONFIGURE_FLAG_USE_CRYPTO_ASYNC` | 异步加密配置 flag |
| API 35 | `queueInputBuffers(int, ArrayDeque<BufferInfo>)` | 批量提交输入 buffer |
| API 35 | `queueSecureInputBuffers(int, ArrayDeque<BufferInfo>, ArrayDeque<CryptoInfo>)` | 批量提交加密 buffer |
| API 34 | `BUFFER_FLAG_DECODE_ONLY` | 仅解码不输出 flag |
| API 36/37 | **无新增** | - |

### MediaFormat 新增 Key/Constant（API 31+）

| API Level | Key | 说明 |
|-----------|-----|------|
| API 31 | `KEY_ALLOW_FRAME_DROP` | 允许/禁止 Surface 丢帧 |
| API 31 | `KEY_COLOR_TRANSFER_REQUEST` | 请求输出视频的 color transfer |
| API 31 | `KEY_VIDEO_QP_MAX` / `KEY_VIDEO_QP_MIN` | 视频 QP 范围 |
| API 31 | `KEY_VIDEO_QP_I_MAX` / `KEY_VIDEO_QP_I_MIN` | I帧 QP 范围 |
| API 31 | `KEY_VIDEO_QP_P_MAX` / `KEY_VIDEO_QP_P_MIN` | P帧 QP 范围 |
| API 31 | `KEY_VIDEO_QP_B_MAX` / `KEY_VIDEO_QP_B_MIN` | B帧 QP 范围 |
| API 31 | `MIMETYPE_AUDIO_MPEGH_MHA1` | MPEG-H Audio (MHA1) |
| API 31 | `MIMETYPE_AUDIO_MPEGH_MHM1` | MPEG-H Audio (MHM1) |
| API 32 | `KEY_MAX_OUTPUT_CHANNEL_COUNT` | 音频解码器最大输出声道数 |
| API 33 | `KEY_CROP_BOTTOM` / `KEY_CROP_LEFT` / `KEY_CROP_RIGHT` / `KEY_CROP_TOP` | 视频裁剪坐标 |
| API 33 | `MIMETYPE_AUDIO_MPEGH_BL_L3` / `BL_L4` | MPEG-H Baseline Profile |
| API 33 | `MIMETYPE_AUDIO_MPEGH_LC_L3` / `LC_L4` | MPEG-H Low Complexity Profile |
| API 33 | `PICTURE_TYPE_B` / `PICTURE_TYPE_I` / `PICTURE_TYPE_P` | 图像帧类型常量 |
| API 34 | `MIMETYPE_IMAGE_AVIF` | AVIF 图像格式 |
| API 35 | `KEY_BUFFER_BATCH_MAX_OUTPUT_SIZE` | 批量 buffer 最大输出大小 |
| API 35 | `KEY_BUFFER_BATCH_THRESHOLD_OUTPUT_SIZE` | 批量 buffer 阈值大小 |
| API 35 | `KEY_IMPORTANCE` | Codec 优先级/重要性 |
| API 36 | `KEY_SECURITY_MODEL` | 安全模型（sandboxed/memory-safe） |
| API 36 | `KEY_PICTURE_PROFILE_ID` | 图片 profile ID |
| API 36 | `KEY_PICTURE_PROFILE_INSTANCE` | 图片 profile 实例 |
| API 36 | `MIMETYPE_AUDIO_IAMF` | IAMF 音频格式 |
| API 37 | `KEY_HDR_ST2094_50_INFO` | SMPTE ST 2094-50 HDR 元数据 |
| API 37 | `KEY_NUM_SLOTS` | Codec slot 数量 |
| API 37 | `MIMETYPE_VIDEO_VVC` | VVC (H.266) 视频格式 |

### MediaExtractor 新增 API

| API Level | API | 说明 |
|-----------|-----|------|
| API 31 | `getLogSessionId()` | 获取日志 session ID |
| API 31 | `setLogSessionId(LogSessionId)` | 设置日志 session ID |

### MediaMuxer 新增 API

**无 API 31+ 新增。** 仅有 API 26 新增 `MediaMuxer(FileDescriptor, int)` 构造方法。

### MediaSync 新增 API

| API Level | API | 说明 |
|-----------|-----|------|
| API 23 | 全部 API（MediaSync 此类为 API 23 新增） | 音视频同步控制 |

### MediaCodecList 新增 API

**无 API 31+ 新增。** 仅在 API 21 新增 `REGULAR_CODECS` / `ALL_CODECS` 常量和 `getCodecInfos()` 方法。

### MediaDrm 新增 API

| API Level | API | 说明 |
|-----------|-----|------|
| API 31 | `requiresSecureDecoder(String mime, int securityLevel)` | 在创建 session 前查询安全解码器需求 |

### 总结：各相关类 API 31+ 新增数量

| 类 | 新增数量 | 说明 |
|-----|---------|------|
| MediaCodec | 12个 | Vendor 参数订阅 + tunnel 回调 + batch buffer |
| MediaFormat | 27个 | QP 控制、HDR、批量 buffer、安全模型、VVC 等 |
| MediaExtractor | 2个 | Log session ID |
| MediaMuxer | 0个 | 无新增 |
| MediaSync | 14个 (全部为 API 23 新增) | 音视频同步 |
| MediaCodecList | 0个 | 无新增 |
| MediaDrm | 1个 | 安全解码器查询 |

---

## 1. Android 12 (API 31) - 视频编码标准化 QP 控制

### Why

此前视频编码的量化参数 (QP) 控制依赖厂商私有实现，开发者需要编写大量vendor-specific代码来处理不同设备。Android 12 引入了标准化的 QP 控制 keys，让开发者无需关心底层实现细节。

### How

```kotlin
// 使用标准化 QP 控制 keys
val format = MediaFormat.createVideoFormat(MediaFormat.MIMETYPE_VIDEO_AVC, 1920, 1080).apply {
    setInteger(MediaFormat.KEY_BIT_RATE, 5_000_000)
    setInteger(MediaFormat.KEY_VIDEO_QP_MAX, 40)      // 最大 QP
    setInteger(MediaFormat.KEY_VIDEO_QP_MIN, 20)      // 最小 QP
    setInteger(MediaFormat.KEY_VIDEO_QP_B_MAX, 42)     // B帧最大 QP
    setInteger(MediaFormat.KEY_VIDEO_QP_B_MIN, 18)     // B帧最小 QP
}
```

**关键 API**:
- `MediaFormat.KEY_VIDEO_QP_MAX` / `KEY_VIDEO_QP_MIN` - 视频 QP 范围
- `MediaFormat.KEY_VIDEO_QP_B_MAX` / `KEY_VIDEO_QP_B_MIN` - B帧 QP 范围
- 同时在 NDK Media 库中可用

---

## 2. Android 12 (API 31) - 兼容媒体转码 (Compatible Media Transcoding)

### Why

设备录制 HEVC(H.265)/HDR(HDR10, HDR10+) 视频后，旧应用可能无法播放。Android 12 支持系统自动将 HEVC/HDR 转码为 AVC(H.264)，兼顾现代 codec 效率与兼容性。

### How

系统自动处理，无需开发者介入。开发者应确保应用支持 HEVC 解码以充分利用此特性：

```kotlin
// 检查设备是否支持 HEVC 解码
val mediaCodecList = MediaCodecList(MediaCodecList.REGULAR_CODECS)
val hevcDecoderName = mediaCodecList.findDecoderForFormat(
    MediaFormat.createVideoFormat("video/hevc", 1920, 1080)
)

// 如支持，优先使用 HEVC；否则系统自动转码
```

---

## 3. Android 12 (API 31) - MediaDrm 安全解码器查询增强

### Why

此前判断安全解码器是否需要，必须先创建 session。Android 12 新增方法可在创建 session 前即可查询。

### How

```kotlin
val mediaDrm = MediaDrm(UUID.fromString(MediaDrmConfig.MEDIA_DRM_UUID))

// Android 12+ 直接查询，无需创建 session
val requiresSecure = mediaDrm.requiresSecureDecoder("video/avc")
val requiresSecureByLevel = mediaDrm.requiresSecureDecoder("video/hevc", MediaDrm.SECURITY_LEVEL_HW_SECURE_CRYPTO)
```

**关键 API**:
- `MediaDrm.requiresSecureDecoder(mime)` - 创建 session 前查询
- `MediaDrm.requiresSecureDecoder(mime, securityLevel)` - 按安全级别查询

---

## 4. Android 13 (API 33) - 空间音频 (Spatial Audio)

### Why

用户期望沉浸式音频体验。Android 13 引入空间音频支持，让媒体内容听起来更真实。

### How

```kotlin
// 查询音频设备能力
val audioManager = getSystemService(AudioManager::class.java)
val devices = audioManager.getAudioDevicesForAttributes(
    AudioAttributes.Builder()
        .setUsage(AudioAttributes.USAGE_MEDIA)
        .setContentType(AudioAttributes.CONTENT_TYPE_MUSIC)
        .build()
)

// 检查是否支持空间音频
val profiles = audioManager.getDirectProfilesForAttributes(audioAttributes)
```

**关键 API**:
- `AudioManager.getAudioDevicesForAttributes()` - 获取音频设备列表
- `AudioManager.getDirectProfilesForAttributes()` - 获取直接播放配置文件

---

## 5. Android 14 (API 34) - 无损 USB 音频 (Lossless USB Audio)

### Why

发烧友和专业用户需要通过 USB 有线耳机获得无损音频体验。Android 14 支持通过 USB 传输无损音频格式。

### How

```kotlin
val audioManager = getSystemService(AudioManager::class.java)
val descriptor = audioManager.getPreferredDeviceForStrategy(strategy)

val mixerAttributes = AudioMixerAttributes.Builder()
    .setUsage(AudioAttributes.USAGE_MEDIA)
    .setSampleRate(96000)          // 96kHz 无损采样率
    .setChannelMask(AudioFormat.CHANNEL_OUT_STEREO)
    .setGain(1.0f)
    .build()

// 注册监听器监听设备偏好变化
audioManager.addPreferredDeviceForStrategyCallback(strategy, executor, listener)
```

**关键 API**:
- `AudioMixerAttributes` - 描述音频格式、采样率、声道掩码等
- `AudioMixerAttributes.MIXER_BEHAVIOR_BIT_PERFECT` - 直通模式，不做混合/音量调整/处理

---

## 6. Android 15 (API 35) - 响度控制 (Loudness Control)

### Why

用户在不同内容间切换时音量忽大忽小，需要频繁调整。Android 15 支持 CTA-2075 标准，根据输出设备特性和音频元数据自动调整响度和动态范围压缩。

### How

```kotlin
// 通过 AudioTrack sessionId 创建 LoudnessCodecController
val mediaCodec = MediaCodec.createEncoderByType("audio/mp4a-latm")
val audioTrack = AudioTrack.Builder()
    .setSessionId(sessionId)
    .build()

val lcController = LoudnessCodecController.create(sessionId)
lcController.setOnLoudnessCodecUpdateListener(executor) { params ->
    // 可在回调中修改或过滤响度参数
}

// 开始应用响度更新
mediaCodec.start()
```

**关键 API**:
- `LoudnessCodecController.create(sessionId)` - 创建响度控制器
- `AudioTrack` 需配合 MediaCodec 使用

---

## 7. Android 15 (API 35) - AV1 软件解码器优化 (dav1d)

### Why

部分设备硬件不支持 AV1 解码，传统软件解码器性能不足。Android 15 引入 dav1d（来自 VideoLAN），性能提升最高 3 倍，让更多中低端设备也能播放 HD AV1 视频。

### How

```kotlin
// 主动调用 dav1d 解码器
val mediaCodecList = MediaCodecList(MediaCodecList.REGULAR_CODECS)
val av1DecoderName = mediaCodecList.findDecoderForFormat(
    MediaFormat.createVideoFormat("video/av01", 1920, 1080)
)

// dav1d 将成为默认 AV1 软件解码器，应用无需修改
val decoder = MediaCodec.createByCodecName(av1DecoderName)
```

**关键说明**:
- 通过名称 `"c2.android.av1-dav1d.decoder"` 调用
- 将通过 Google Play 系统更新回退至 Android 11

---

## 8. Android 17 (API 37) - HE-AAC 软件编码器

### Why

低带宽场景下传统 AAC 编码器音质与比特率难以兼顾。Android 17 引入 `c2.android.xheaac.encoder`，专为低带宽优化，支持响度元数据确保音量一致。

### How

```kotlin
val mediaCodecList = MediaCodecList(MediaCodecList.REGULAR_CODECS)
    val codecName = mediaCodecList.findEncoderForFormat(
    MediaFormat.createAudioFormat(MediaFormat.MIMETYPE_AUDIO_AAC, 44100, 2)
)

// 配置 HE-AAC 编码
val format = MediaFormat.createAudioFormat(MediaFormat.MIMETYPE_AUDIO_AAC, 44100, 2).apply {
    setInteger(MediaFormat.KEY_BIT_RATE, 64000)
    setInteger(MediaFormat.KEY_AAC_PROFILE, MediaCodecInfo.CodecProfileLevel.AACObjectLC)
}

val encoder = MediaCodec.createByCodecName(codecName)
encoder.configure(format, null, null, MediaCodec.CONFIGURE_FLAG_ENCODE)
encoder.start()
```

**关键常量**:
- `c2.android.xheaac.encoder` - 新的系统级软件编码器

---

## 9. Android 17 (API 37) - Temporal Layering 支持

### Why

Temporal Layering 是 SVC 分层编码的一种，按时间将帧分为不同层级。解码器可在带宽受限时丢弃高层帧降级帧率（如 30fps → 15fps），而不影响基本层播放。

视频流编码时帧依赖元数据缺失会导致丢帧卡顿。Android 17 修复 Issue #306222291，允许通过 `getOutputFormat` 确认 temporal layering 支持。

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

// Android 17: 配置后可获取 outputFormat 确认 temporal layering
val outputFormat = encoder.outputFormat
val temporalLayering = outputFormat.getInteger("temporal-layering", 0)

if (temporalLayering > 0) {
    // temporal layering 已启用
}
```

**关键 API**:
- `MediaCodec.getOutputFormat()` - 配置后调用
- `"temporal-layering"` - 新增 format key

---

## 10. Android 17 (API 37) - 视频录制恒定质量 (CQ) 模式

### Why

屏幕录制、视频通话等场景更关注画质稳定而非特定比特率。Android 17 在 MediaRecorder 中引入恒定质量 (Constant Quality) 模式，让编码器自动调整比特率维持目标画质。

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
- `MediaRecorder.setVideoEncodingQuality(int quality)` - 1 为最高质量

---

## 11. Android 17 (API 37) - VVC (H.266) 支持

### Why

VVC (H.266) 是新一代视频编码标准，相同画质下节省 30-50% 带宽，或相同带宽下更好画质。Android 17 正式引入平台支持，为 4K/8K/VR 应用铺路。

### How

```kotlin
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
- `MediaFormat.MIMETYPE_VIDEO_VVC` - VVC MIME 类型

---

## 12. Android 17 (API 37) - 后台音频加固 (Background Audio Hardening)

### Why

应用在后台无效生命周期状态时仍可调用音频 API，导致音频焦点混乱、电量消耗。Android 17 加固限制：无效生命周期下音频 API 静默失败。

### How

```kotlin
// Android 17 后台音频行为
// 应用在后台且无有效前台服务时
val audioManager = getSystemService(AudioManager::class.java)
val result = audioManager.requestAudioFocus(AudioFocusRequest.Builder(
    AudioManager.AUDIOFOCUS_GAIN_TRANSIENT
).build())

// Android 17+ 在无效生命周期下返回 AUDIOFOCUS_REQUEST_FAILED，静默失败不抛异常

// 正确做法：使用前台服务 + WIU capability
class AudioPlaybackService : Service() {
    override fun onCreate() {
        super.onCreate()
        val notification = Notification.Builder(this, CHANNEL_ID)
            .setContentTitle("Playing audio")
            .build()

        val wiuCapabilities = Notification.FOREGROUND_SERVICE_TYPE_MEDIA_PLAYBACK or
                              Notification.FOREGROUND_SERVICE_TYPE_USAGE_LOCATION

        startForeground(NOTIFICATION_ID, notification, wiuCapabilities)
    }
}
```

**关键变更**:
- 目标 API 37+ 需要前台服务 + WIU capability 或 exact alarm 权限
- `AUDIOFOCUS_REQUEST_FAILED` 返回码表示失败

---

## 附录：各版本 MediaCodec 相关特性总览

| 版本 | 特性 | 类型 |
|------|------|------|
| Android 12 | 标准化 QP 控制 keys | 视频编码 |
| Android 12 | 兼容媒体转码 (HEVC/HDR→AVC) | 视频转码 |
| Android 12 | MediaDrm.requiresSecureDecoder 增强 | DRM |
| Android 13 | 空间音频支持 | 音频 |
| Android 13 | 音频路由 API | 音频 |
| Android 14 | 无损 USB 音频 | 音频 |
| Android 15 | LoudnessCodecController 响度控制 | 音频 |
| Android 15 | dav1d AV1 软件解码器 | 视频解码 |
| Android 17 | HE-AAC 软件编码器 | 音频编码 |
| Android 17 | Temporal Layering 支持 | 视频编码 |
| Android 17 | 视频录制 CQ 模式 | 视频编码 |
| Android 17 | VVC (H.266) 支持 | 视频编码/解码 |
| Android 17 | 后台音频加固 | 音频安全 |