# FFmpeg Android 支持分析

> 覆盖 FFmpeg 中对 Android 平台的所有支持：编译选项、MediaCodec 编解码、Camera 输入、content:// 协议、Vulkan 互操作、HardwareBuffer 现状

---

## 1. Android 编译选项总览

### 1.1 核心选项

| 编译选项 | 类型 | 说明 |
|---------|------|------|
| `--enable-mediacodec` | MediaCodec 框架 | 启用 MediaCodec 编解码器 + HWDeviceContext |
| `--enable-jni` | JNI 框架 | 启用 JNI 支持 (MediaCodec 依赖) |
| `android_camera` indev | 设备输入 (自动检测) | Android Camera2 NDK 摄像头输入，依赖 `camera2ndk` lib |

### 1.2 依赖链

```
--enable-mediacodec
  ├── --enable-jni
  │     └── android (-landroid, <android/native_window.h>)
  ├── android (-landroid, <android/native_window.h>)
  ├── mediandk (-lmediandk, <media/NdkMediaFormat.h>)
  └── pthreads

android_camera indev (自动检测 camera2ndk)
  ├── android
  ├── camera2ndk (-lcamera2ndk, <camera/NdkCameraManager.h>)
  ├── mediandk
  └── pthreads
```

### 1.3 `--enable-opengl` — 已删除的死选项

> 该选项与 Android 无关，此处记录以厘清疑问。

**设计目标:** 原为 `libavdevice/opengl_enc.c`（FFmpeg 7.1 前），一个将 raw video 渲染到 OpenGL 窗口的输出设备，类似 `ffmpeg -f opengl ...` 使用。它通过 SDL2（优先）或 GLX/WGL 创建 OpenGL 上下文并显示视频帧。

**已被删除:** 该输出设备于 2024-02 弃用、2025-02 完全移除（commit `9283c5251f`）。当前 `--enable-opengl` 开启后不编译任何文件，`CONFIG_OPENGL` 宏已不在任何源文件中使用。

**对 Android 端无意义:**
- 原实现的 GL 上下文获取只有桌面路径：GLX（X11）、WGL（Windows）、SDL2（桌面 SDL），**没有 EGL**，**没有 ANativeWindow**。
- configure 检测 `ES2/gl.h + -framework OpenGLES` 仅针对 iOS，NDK 环境下会检测失败并 `die("ERROR: opengl not found.")` 中断构建。
- 即使通过，原代码也只绑定了桌面 GL 扩展（`GL_ARB_multitexture`, `GL_ARB_vertex_buffer_object` 等），Android GLES 路径不可用。

---

## 2. MediaCodec 编解码器

### 2.1 解码器 (mediacodecdec.c)

| 解码器 | MIME | Bitstream Filter |
|--------|------|-----------------|
| `h264_mediacodec` | `video/avc` | `h264_mp4toannexb` |
| `hevc_mediacodec` | `video/hevc` | `hevc_mp4toannexb` |
| `mpeg2_mediacodec` | `video/mpeg2` | 无 |
| `mpeg4_mediacodec` | `video/mp4v-es` | 无 |
| `vp8_mediacodec` | `video/x-vnd.on2.vp8` | 无 |
| `vp9_mediacodec` | `video/x-vnd.on2.vp9` | 无 |
| `av1_mediacodec` | `video/av01` | 无 |
| `aac_mediacodec` | `audio/mp4a-latm` | `aac_adtstoasc` |
| `amrnb_mediacodec` | `audio/3gpp` | 无 |
| `amrwb_mediacodec` | `audio/amr-wb` | 无 |
| `mp3_mediacodec` | `audio/mpeg` | 无 |

所有解码器标记为 `AV_CODEC_CAP_HARDWARE`。

### 2.2 编码器 (mediacodecenc.c)

| 编码器 | MIME |
|--------|------|
| `h264_mediacodec` | `video/avc` |
| `hevc_mediacodec` | `video/hevc` |
| `vp8_mediacodec` | `video/x-vnd.on2.vp8` |
| `vp9_mediacodec` | `video/x-vnd.on2.vp9` |
| `mpeg4_mediacodec` | `video/mp4v-es` |
| `av1_mediacodec` | `video/av01` |

支持输入像素格式: `AV_PIX_FMT_MEDIACODEC`, `AV_PIX_FMT_YUV420P`, `AV_PIX_FMT_NV12`。

### 2.3 公开 API (mediacodec.h)

面向用户的 MediaCodec 解码 API，通过 `AVMediaCodecContext` 与 Surface 交互:

```c
// 分配/释放上下文
AVMediaCodecContext *av_mediacodec_alloc_context(void);
void av_mediacodec_default_free(AVCodecContext *avctx);

// 初始化（传入 Java Surface）
int av_mediacodec_default_init(AVCodecContext *avctx, AVMediaCodecContext *ctx, void *surface);

// 解码输出 buffer 操作
int av_mediacodec_release_buffer(AVMediaCodecBuffer *buffer, int render);      // 释放并渲染到 Surface
int av_mediacodec_render_buffer_at_time(AVMediaCodecBuffer *buffer, int64_t time); // 指定时间渲染
```

**结构体:**
```c
typedef struct AVMediaCodecContext {
    void *surface;  // Java android/view/Surface
} AVMediaCodecContext;

typedef struct MediaCodecBuffer AVMediaCodecBuffer;  // 不透明，存储在 frame->data[3]
```

### 2.4 重要编解码选项

| 选项 | 适用 | 说明 |
|------|------|------|
| `ndk_codec` | 解码器/编码器 | -1=auto, 0=强制JNI, 1=强制NDK |
| `delay_flush` | 解码器 | 1=延迟 flush 直到所有 HW buffer 返回 |
| `operating_rate` | 解码器/编码器 | 目标码率（0=未指定） |
| `ndk_async` | 编码器 | 1=启用 NDK MediaCodec 异步模式 |
| `bitrate_mode` | 编码器 | cq/vbr/cbr/cbr_fd |

---

## 3. MediaCodec 实现架构

### 3.1 双实现路径

`mediacodec_wrapper.c` 提供 JNI / NDK 两套实现，`ndk_codec` 选项控制:

- `-1` (auto): 无 JVM → NDK
- `0`: 强制 JNI
- `1`: 强制 NDK

NDK 路径额外支持异步回调、编码器输入 Surface、EOS 信号。

### 3.2 两种解码模式

```ascii
模式 A: 无 Surface (软件回读)

  encoded bitstream
      ↓
  OMX/Codec2 硬件解码器
      ↓
  硬件解码器写入 CPU-mapped ByteBuffer
      ↓
  AMediaCodec_getOutputBuffer() → CPU 指针
      ↓
  ff_get_buffer 分配 FFmpeg frame
      ↓
  按 color_format 复制 YUV → AV_PIX_FMT_NV12/YUV420P
      ↓
  AMediaCodec_releaseOutputBuffer() 释放 ByteBuffer
      ↓
  frame 在 CPU 内存, 可用于 filter/encode ✅


模式 B: 有 Surface (opaque)

  encoded bitstream
      ↓
  OMX/Codec2 硬件解码器
      ↓
  解码到 Surface / GPU
      ↓
  AV_PIX_FMT_MEDIACODEC
      ↓
  frame->data[3] 存 AVMediaCodecBuffer (opaque handle)
      ↓
  无法 filter/encode, 只能渲染到 Surface ❌
```

### 3.3 SW Buffer 复制

`mediacodec_sw_buffer.c` 处理各厂商压缩格式到标准 YUV:

| 函数 | 对应 ColorFormat |
|------|-----------------|
| `ff_mediacodec_sw_buffer_copy_yuv420_planar` | `COLOR_FormatYUV420Planar` (0x13) |
| `ff_mediacodec_sw_buffer_copy_yuv420_semi_planar` | `COLOR_FormatYUV420SemiPlanar` (0x15) |
| `ff_mediacodec_sw_buffer_copy_yuv420_packed_semi_planar` | `COLOR_FormatYUV420PackedSemiPlanar` (通用) |
| `ff_mediacodec_sw_buffer_copy_yuv420_packed_semi_planar_64x32Tile2m8ka` | `COLOR_QCOM_FormatYUV420PackedSemiPlanar64x32Tile2m8ka` (0x7fa30c03) |

### 3.4 FFAMediaCodecList — 编解码器查询

**文件:** `mediacodec_wrapper.h` / `mediacodec_wrapper.c`

```c
char *ff_AMediaCodecList_getCodecNameByType(const char *mime, int profile,
                                             int encoder, void *log_ctx);
```

根据 MIME 类型和 profile 查询系统中可用的 MediaCodec 名称。用于:
- 绕过有问题的厂商 decoder 实现
- 获取 codec 名称进行 workaround（如 `OMX.amlogic.mpeg2.decoder.awesome` 的特殊处理）

---

## 4. 其他 Android 功能

### 4.1 Camera 输入 (android_camera indev)

**编译: 自动检测, 依赖 `camera2ndk` lib**

使用 Camera2 NDK API，通过 `AImageReader` 捕获相机帧:

```ascii
ACameraManager → ACameraDevice → ACaptureSession
                                      ↓
                                 AImageReader
                                      ↓
                            AImage_getPlaneData() → YUV420P/NV12/NV21
```

支持分辨率/帧率选择、前后摄像头、旋转、镜像。

### 4.2 JNI 集成

#### 4.2.1 公开 API (jni.h)

```c
int av_jni_set_java_vm(void *vm, void *log_ctx);         // 设置 JavaVM
void *av_jni_get_java_vm(void *log_ctx);

// Android only:
int av_jni_set_android_app_ctx(void *app_ctx, void *log_ctx);  // 设置 Application Context
void *av_jni_get_android_app_ctx(void);
```

#### 4.2.2 内部 JNI 工具 (ffjni.c / ffjni.h)

FFmpeg Android 代码的通用 JNI 工具库，被 MediaCodec、Surface、content:// 等多处依赖:

| 函数 | 用途 |
|------|------|
| `ff_jni_get_env()` | 获取当前线程 JNIEnv，自动处理 Attach/Detach |
| `ff_jni_jstring_to_utf_chars()` | jstring → UTF-8 |
| `ff_jni_utf_chars_to_jstring()` | UTF-8 → jstring |
| `ff_jni_exception_get_summary()` | 从 jthrowable 提取 "className: message" |
| `ff_jni_exception_check()` | 检查并清除 JNI 异常 |
| `ff_jni_init_jfields()` | 通过 FFJniField 映射表批量初始化 JNI 引用 |
| `ff_jni_reset_jfields()` | 释放 JNI 引用 |

**FFJniField 映射机制:** 用结构体描述要获取的 Java 类/方法/字段，通过偏移量写入目标结构体，实现 JNI 引用的类型安全批量初始化:

```c
struct FFJniField {
    const char *name;       // "android/net/Uri"
    const char *method;     // "parse"
    const char *signature;  // "(Ljava/lang/String;)Landroid/net/Uri;"
    enum FFJniFieldType type;
    size_t offset;
    int mandatory;
};
```

### 4.3 content:// 协议 (依赖 JNI)

**编译: `--enable-jni`**。通过 JNI 解析 `content://` URI 为文件描述符 fd:

```
content:// URI
    ↓
JNI: Uri.parse() → ContentResolver.openFileDescriptor()
    ↓
ParcelFileDescriptor.detachFd()  →  获取原始 fd
    ↓
FFmpeg 通过 fd 读写内容
```

**使用方式:** `ffmpeg -i content://com.example.app/file ...`

### 4.4 Android 15+ Binder 兼容

```c
// compat/android/binder.c
// API >= 35: ABinderProcess_startThreadPool() 必须调用
// 否则 MediaCodec NDK 无法正常工作
```

### 4.5 Surface 引用管理 (mediacodec_surface.c)

**文件:** `libavcodec/mediacodec_surface.c` (78行)

管理 Java Surface (GlobalRef) 和 ANativeWindow* 之间的引用:

```c
typedef struct FFANativeWindow {
    void *surface;         // Java android/view/Surface (NewGlobalRef)
    void *native_window;   // ANativeWindow* (acquired)
} FFANativeWindow;

FFANativeWindow *ff_mediacodec_surface_ref(void *surface, void *native_window, void *log_ctx);
int ff_mediacodec_surface_unref(FFANativeWindow *window, void *log_ctx);
```

---

## 5. HWDeviceContext 体系

### 5.1 hwcontext_mediacodec — 空壳

**文件:** `libavutil/hwcontext_mediacodec.c` (121 行)

已实现:
| 回调 | 说明 |
|------|------|
| `device_create` | 解析 `create_window` 选项 |
| `device_init` | 可选创建 `PersistentInputSurface` |
| `device_uninit` | 释放 ANativeWindow |

未实现 (全部 NULL):
| 回调 | 说明 |
|------|------|
| `frames_get_buffer` | ❌ 无帧池分配 |
| `transfer_get_formats` | ❌ 无传输格式 |
| `transfer_data_to/from` | ❌ 无法上传/下载 |
| `map_to/map_from` | ❌ 无法映射到/从其他 HW context |
| `frames_derive_to/from` | ❌ 无法派生 |

```c
// hwcontext_mediacodec.h
typedef struct AVMediaCodecDeviceContext {
    void *surface;         // Java android/view/Surface
    void *native_window;   // ANativeWindow*
    int create_window;     // 自动创建 PersistentInputSurface
} AVMediaCodecDeviceContext;
```

### 5.2 hwcontext_vulkan — 无 Android 扩展

**文件:** `libavutil/hwcontext_vulkan.c`

已支持的 Vulkan 外部内存扩展:
```c
{ "VK_KHR_external_memory_fd",              "opaque fd"     }
{ "VK_EXT_external_memory_dma_buf",         "dma buf"       }
{ "VK_EXT_image_drm_format_modifier",       "drm modifier"  }
{ "VK_KHR_external_semaphore_fd",           "opaque fd sem" }
{ "VK_EXT_external_memory_host",            "host memory"   }
{ "VK_KHR_external_memory_win32",           "win32 mem"     }
{ "VK_KHR_external_semaphore_win32",        "win32 sem"     }
// ❌ 没有 VK_ANDROID_external_memory_android_hardware_buffer
```

`vulkan_map_to()` 只接受:
- `AV_PIX_FMT_VAAPI` (通过 VAAPI → DRM → Vulkan)
- `AV_PIX_FMT_DRM_PRIME`
- ❌ 不接受 `AV_PIX_FMT_MEDIACODEC`

### 5.3 互操作路径对比

```
Vulkan ↔ CUDA:
  VkDeviceMemory
      → vkGetMemoryFdKHR(OPAQUE_FD)
      → cuImportExternalMemory(OPAQUE_FD)
      → cuExternalMemoryGetMappedMipmappedArray
      → CUarray (每个 plane 一个)

Vulkan ↔ DRM:
  VkDeviceMemory
      → vkGetMemoryFdKHR(DMA_BUF)
      → DRM PRIME fd
      → AVDRMFrameDescriptor
```

---

## 6. HardwareBuffer 支持现状

### 6.1 搜索结果 (全代码库 0 匹配)

| 搜索内容 | 结果 |
|---------|:---:|
| `AHardwareBuffer` | ❌ 0 |
| `hardware_buffer` / `hardwareBuffer` | ❌ 0 |
| `VK_ANDROID_external_memory_android_hardware_buffer` | ❌ 0 |
| `AMediaCodec_getHardwareBuffer` | ❌ 0 |
| `getHardwareBuffer` / `getOutputFrame` / `getOutputImage` | ❌ 0 |
| `AImage_getHardwareBuffer` | ❌ 0 |
| `VK_ANDROID` 任何扩展 | ❌ 0 |
| git log 中 hardwarebuffer 相关提交 | ❌ 0 |

### 6.2 没有的 API 调用

当前 `mediacodec_wrapper.c` 只绑定了:

```c
// JNI: getInputBuffer / getOutputBuffer → ByteBuffer
{ "android/media/MediaCodec", "getInputBuffer",  "(I)Ljava/nio/ByteBuffer;" }
{ "android/media/MediaCodec", "getOutputBuffer", "(I)Ljava/nio/ByteBuffer;" }

// NDK: AMediaCodec_getOutputBuffer → uint8_t*
AMediaCodec_getOutputBuffer(codec, idx, out_size)
```

**从未调用过:**

- `AMediaCodec_getHardwareBuffer()` — NDK API 30+, 返回 `AHardwareBuffer*`
- `MediaCodec.OutputFrame.getHardwareBuffer()` — Java API 30+
- `AMediaCodec_getOutputFormat()` + `KEY_HARDWARE_BUFFER`
- `AImage_getHardwareBuffer()`

### 6.3 缺失链

```
MediaCodec Surface/PixelBuffer 输出
    ↓ 缺失
AMediaCodec_getHardwareBuffer() / getOutputFrame().getHardwareBuffer()
    ↓ 缺失 (mediacodecdec_common.c)
AHardwareBuffer 导出到 AVFrame.buf[]
    ↓ 缺失 (hwcontext_mediacodec.c)
AV_PIX_FMT_MEDIACODEC → AHardwareBuffer 转换
    ↓ 缺失 (hwcontext_vulkan.c)
VK_ANDROID_external_memory_android_hardware_buffer 扩展请求
    ↓ 缺失 (hwcontext_vulkan.c)
vkGetMemoryAndroidHardwareBufferANDROID() 导入为 VkImage
    ↓ 缺失 (hwcontext_vulkan.c / vulkan_map_from)
Vulkan texture / 后续 filter / encode
```

### 6.4 需要补的三个环节

| # | 位置 | 需要做的事 |
|---|------|-----------|
| 1 | `mediacodecdec_common.c` + `mediacodec_wrapper.c` | 调用 `AMediaCodec_getHardwareBuffer()` NDK API, 将 AHardwareBuffer 存入 frame 的 `AVBufferRef` |
| 2 | `hwcontext_mediacodec.c` | 实现 `transfer_get_formats` / `map_to` / `map_from`, 以 `AHardwareBuffer` 为媒介导出帧 |
| 3 | `hwcontext_vulkan.c` | 添加 `VK_ANDROID_external_memory_android_hardware_buffer` 扩展, 在 `vulkan_map_from()` 中导入 `AHardwareBuffer` 为 `VkImage` |

### 6.5 上游 FFmpeg 状态

截至 2026-05，上游 main 分支 **同样没有** Android HardwareBuffer 支持:

| 反馈 | 状态 |
|------|------|
| [FFmpeg-devel](https://lists.ffmpeg.org/) "AHardwareBuffer" 搜索 | 无实质性提案 |
| [trac.ffmpeg.org](https://trac.ffmpeg.org/) 相关 ticket | 无 |
| Gerrit patchset | 无 |

参考实现:
- [GPAC/gpac](https://github.com/gpac/gpac) — 有 `AHardwareBuffer` 导出 + Vulkan 导入的实验性支持
- [Android Codec2 参考实现](https://android.googlesource.com/platform/frameworks/av/) — MediaCodec 内部使用 `C2Buffer` + `AHardwareBuffer`

---

## 7. 总结

| 特性 | FFmpeg 现有支持 | 需要补充 |
|------|----------------|---------|
| `-hwaccel mediacodec` 解码加速 | ✅ OMX/Codec2 HW | |
| 软件回读 (CPU YUV) | ✅ | |
| Surface opaque 输出 | ✅ `AV_PIX_FMT_MEDIACODEC` | |
| AHardwareBuffer 导出 | ❌ | `mediacodecdec`, `mediacodec_wrapper` |
| hwcontext 帧池/传输 | ❌ 空壳 | `hwcontext_mediacodec.c` |
| Vulkan AHardwareBuffer 导入 | ❌ | `hwcontext_vulkan.c` |
| CUDA hwaccel (含 Vulkan 互操作) | ✅ 完整 hwcontext | |
| VAAPI/QSV/D3D11 hwaccel | ✅ 完整 hwcontext | |
| Camera 输入 (android_camera) | ✅ 自动检测 `camera2ndk` | |
| content:// 协议 | ✅ `--enable-jni` | |
| Android 15+ Binder 兼容 | ✅ `compat/android/binder.c` | |

---

## 附录: 文件清单

| 文件 | 用途 | 行数 |
|------|------|------|
| `libavcodec/mediacodec_wrapper.c` | JNI + NDK MediaCodec API 封装 | 2692 |
| `libavcodec/mediacodecdec_common.c` | 解码器核心 (init/send/receive) | 1151 |
| `libavcodec/mediacodecenc.c` | 编码器 | 1504 |
| `libavcodec/mediacodecdec.c` | 各解码器注册 | 712 |
| `libavcodec/mediacodec_sw_buffer.c` | 软件 YUV 复制 | 339 |
| `libavcodec/mediacodec.c` | 公共 API | 148 |
| `libavcodec/mediacodec_surface.c` | Surface/ANativeWindow 管理 | 78 |
| `libavcodec/jni.c` | JNI API (公开) | 124 |
| `libavcodec/ffjni.c` | JNI 工具函数 (内部) | 413 |
| `libavcodec/ffjni.h` | JNI 工具头文件 | 146 |
| `libavcodec/jni.h` | JNI API 头文件 | 68 |
| `libavdevice/android_camera.c` | Camera 输入设备 | 874 |
| `libavutil/hwcontext_mediacodec.c` | HWDeviceContext | 121 |
| `compat/android/binder.c` | Android 15+ Binder 兼容 | 114 |
| `compat/android/binder.h` | Binder 头文件 | 31 |
