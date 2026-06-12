# FFmpeg 支持的三方库

> 基于 `configure` 中 `EXTERNAL_LIBRARY_LIST` / `HWACCEL_LIBRARY_LIST` / `EXTERNAL_AUTODETECT_LIBRARY_LIST` 整理。
> ~~删除线~~ 表示 Android 不可用。

## 视频编解码库

| 库名 | 用途 | 许可证 | 类型 | Android | GitHub URL |
|------|------|:---:|------|:---:|------------|
| **libaom** | AV1 编解码 | BSD | *参考实现* | ✅ | <https://github.com/AOM-AV1/aom> |
| **libdav1d** | AV1 快速解码器 | BSD | 优化实现 | ✅ | <https://code.videolan.org/videolan/dav1d> |
| **libsvtav1** | AV1 高性能编码器 | BSD | 优化实现 | ✅ | <https://gitlab.com/AOMediaCodec/SVT-AV1> |
| **librav1e** | AV1 编码器 (Rust) | BSD | 优化实现 | ✅ | <https://github.com/xiph/rav1e> |
| **libx264** | H.264 编码 | GPL | 优化实现 | ✅ | <https://code.videolan.org/videolan/x264> |
| **libx265** | H.265/HEVC 编码 | GPL | 优化实现 | ✅ | <https://bitbucket.org/multicoreware/x265_git> |
| **libopenh264** | H.264 编码 | BSD | 优化实现 | ✅ | <https://github.com/cisco/openh264> |
| **libkvazaar** | HEVC 编码 | BSD | 优化实现 | ✅ | <https://github.com/ultravideo/kvazaar> |
| **libvvenc** | H.266/VVC 编码 | BSD | *参考实现* | ✅ | <https://github.com/fraunhoferhhi/vvenc> |
| **libvvdec** | H.266/VVC 解码 | BSD | *参考实现* | ✅ | <https://github.com/fraunhoferhhi/vvdec> |
| **libvpx** | VP8/VP9 编解码 | BSD | *参考实现* | ✅ | <https://github.com/AOM-AV1/libvpx> |
| **libtheora** | Theora 编码 | BSD | *参考实现* | ✅ | <https://github.com/xiph/theora> |
| **libxvid** | MPEG-4 编码 | GPL | 优化实现 | ✅ | <https://labs.xvid.com/source/> |
| **libxavs** | AVS 编码 | GPL | *参考实现* | ✅ | <https://github.com/nicxzhang/xavs> |
| **libxavs2** | AVS2 编码 | GPL | *参考实现* | ✅ | <https://github.com/pkuvcl/xavs2> |
| **libdavs2** | AVS2 解码 | GPL | *参考实现* | ✅ | <https://github.com/pkuvcl/davs2> |
| **libuavs3d** | AVS3 解码 | BSD | *参考实现* | ✅ | <https://github.com/nicxzhang/uavs3d> |
| **libsvtjpegxs** | JPEG-XS 编解码 | BSD | 优化实现 | ✅ | <https://github.com/OpenVisualCloud/SVT-JPEG-XS> |
| **libxeve** | EVC 编码 | BSD | *参考实现* | ✅ | <https://github.com/mpeg5/xeve> |
| **libxeveb** | EVC Baseline 编码 | BSD | *参考实现* | ✅ | <https://github.com/mpeg5/xeve> |
| **libxevd** | EVC 解码 | BSD | *参考实现* | ✅ | <https://github.com/mpeg5/xevd> |
| **libxevdb** | EVC Baseline 解码 | BSD | *参考实现* | ✅ | <https://github.com/mpeg5/xevd> |
| **libjxl** | JPEG XL 编解码 | BSD | *参考实现* | ✅ | <https://github.com/libjxl/libjxl> |
| **libwebp** | WebP 编解码 | BSD | 优化实现 | ✅ | <https://github.com/webmproject/libwebp> |
| **libopenjpeg** | JPEG 2000 编解码 | BSD | 优化实现 | ✅ | <https://github.com/uclouvain/openjpeg> |
| **liboapv** | APV 编码 | BSD | 优化实现 | ✅ | <https://github.com/Netflix/oapv> |
| **liblcevc_dec** | LCEVC 解码 | BSD | *参考实现* | ✅ | <https://github.com/v-nova/lcevc_dec> |

## 音频编解码库

| 库名 | 用途 | 许可证 | 类型 | Android | GitHub URL |
|------|------|:---:|------|:---:|------------|
| **libfdk_aac** | AAC 编解码 | Nonfree ✱ | 优化实现 | ✅ | <https://github.com/mstorsjo/fdk-aac> |
| **libopus** | Opus 编解码 | BSD | *参考实现* | ✅ | <https://github.com/xiph/opus> |
| **libmp3lame** | MP3 编码 | LGPL | 优化实现 | ✅ | <https://lame.sourceforge.io/> |
| **libshine** | 固定点 MP3 编码 | LGPL | 优化实现 | ✅ | <https://github.com/toots/shine> |
| **libvorbis** | Vorbis 编解码 | BSD | *参考实现* | ✅ | <https://github.com/xiph/vorbis> |
| **libspeex** | Speex 编解码 | BSD | *参考实现* | ✅ | <https://github.com/xiph/speex> |
| **libcodec2** | Codec2 语音编解码 | LGPL | *参考实现* | ✅ | <https://github.com/drowe67/codec2> |
| **libgsm** | GSM 编解码 | TU-Berlin | *参考实现* | ✅ | <https://www.quut.com/gsm/> |
| **libilbc** | iLBC 编解码 | BSD | *参考实现* | ✅ | <https://github.com/nicxzhang/libilbc> |
| **libopencore_amrnb** | AMR-NB 编解码 | Version3 | *参考实现* | ✅ | <https://sourceforge.net/projects/opencore-amr/> |
| **libopencore_amrwb** | AMR-WB 解码 | Version3 | *参考实现* | ✅ | <https://sourceforge.net/projects/opencore-amr/> |
| **libvo_amrwbenc** | AMR-WB 编码 | Version3 | *参考实现* | ✅ | <https://sourceforge.net/projects/opencore-amr/> |
| **libtwolame** | MP2 编码 | LGPL | 优化实现 | ✅ | <https://github.com/njh/twolame> |
| **liblc3** | LC3 编解码 | BSD | *参考实现* | ✅ | <https://github.com/nicxzhang/liblc3> |
| **libcelt** | CELT 解码 | BSD | *参考实现* | ✅ | <https://downloads.xiph.org/releases/celt/> |

> ⚠️ libcelt 将在下一版本（`<next>`）中移除，参见 FFmpeg Changelog "Remove CELT decoding support"。
| **libmpeghdec** | MPEG-H 3DA 解码 | Nonfree ✱ | *参考实现* | ✅ | <https://github.com/nicxzhang/libmpeghdec> |
| **libgme** | 游戏音乐模拟 | LGPL | 其他 | ✅ | <https://github.com/libgme/game-music-emu> |
| **libflite** | 语音合成 | MIT | 其他 | ✅ | <https://github.com/festvox/flite> |
| **libmodplug** | MOD 音乐解码 | Public | 其他 | ✅ | <https://github.com/Konstanty/libmodplug> |
| **libopenmpt** | MOD 音乐解码 | BSD | 其他 | ✅ | <https://github.com/nicxzhang/libopenmpt> |
| **chromaprint** | 音频指纹 | LGPL | 其他 | ✅ | <https://github.com/acoustid/chromaprint> |
| **pocketsphinx** | 语音识别 | BSD | 其他 | ✅ | <https://github.com/cmusphinx/pocketsphinx> |
| **whisper** | 语音识别 (OpenAI) | MIT | 其他 | ✅ | <https://github.com/openai/whisper> |
| **openal** | OpenAL 3D 音频 | LGPL | 系统库 | ✅ | <https://github.com/kcat/openal-soft> |

> ✱ Nonfree：启用后 FFmpeg 不可商用分发。

## 音频插件 API

| 库名 | 用途 | 许可证 | 类型 | Android | GitHub URL |
|------|------|:---:|------|:---:|------------|
| **ladspa** | LADSPA 音频插件 | LGPL | 其他 | ⚠️ 需交叉编译 | <https://www.ladspa.org/> |
| **lv2** | LV2 音频插件 | ISC | 其他 | ⚠️ 需交叉编译 | <https://github.com/lv2/lv2> |

## 字幕/文本处理库

| 库名 | 用途 | 许可证 | 类型 | Android | GitHub URL |
|------|------|:---:|------|:---:|------------|
| **libass** | ASS/SSA 字幕渲染 | ISC | 优化实现 | ✅ | <https://github.com/libass/libass> |
| **libaribb24** | ARIB 字幕解码 | Version3 | *参考实现* | ✅ | <https://github.com/nicxzhang/libaribb24> |
| **libaribcaption** | ARIB 字幕解码 | BSD | *参考实现* | ✅ | <https://github.com/nicxzhang/aribcaption> |
| **libfreetype** | 字体渲染 | FTL/GPL | 系统库 | ✅ | <https://gitlab.freedesktop.org/freetype/freetype> |
| **libfontconfig** | 字体配置 | MIT | 系统库 | ✅ | <https://gitlab.freedesktop.org/fontconfig/fontconfig> |
| **libfribidi** | BiDi 文本处理 | LGPL | 系统库 | ✅ | <https://github.com/fribidi/fribidi> |
| **libharfbuzz** | 文本整形 | MIT | 系统库 | ✅ | <https://github.com/harfbuzz/harfbuzz> |
| **libtesseract** | OCR 文字识别 | Apache | 其他 | ✅ | <https://github.com/tesseract-ocr/tesseract> |
| **libqrencode** | QR 码生成 | LGPL | 其他 | ✅ | <https://github.com/fukuchi/libqrencode> |
| **libquirc** | QR 码解码 | ISC | 其他 | ✅ | <https://github.com/nicxzhang/libquirc> |
| **libzvbi** | 图文电视 | LGPL | 其他 | ✅ | <https://github.com/zapping-vbi/zvbi> |

## 视频处理/滤镜库

| 库名 | 用途 | 许可证 | 类型 | Android | GitHub URL |
|------|------|:---:|------|:---:|------------|
| **libplacebo** | GPU 视频渲染 | LGPL | 硬件加速 | ✅ | <https://code.videolan.org/videolan/libplacebo> |
| **libzimg** | 图像缩放/色彩转换 | WTFPL | 优化实现 | ✅ | <https://github.com/sekrit-twc/zimg> |
| **libopencv** | 计算机视觉 | Apache | 其他 | ✅ | <https://github.com/opencv/opencv> |
| **libvidstab** | 视频稳定 | GPL | 优化实现 | ✅ | <https://github.com/georgmartius/vid.stab> |
| **librubberband** | 音频时间拉伸 | GPL | 优化实现 | ✅ | <https://github.com/breakfastquay/rubberband> |
| **libvmaf** | 视频质量评估 | BSD+Patent | 其他 | ✅ | <https://github.com/Netflix/vmaf> |
| **liblensfun** | 镜头校正 | Version3 | 其他 | ✅ | <https://github.com/lensfun/lensfun> |
| **lcms2** | 色彩管理 (Little CMS) | MIT | 系统库 | ✅ | <https://github.com/mm2/Little-CMS> |
| **libmysofa** | SOFA HRTF | BSD | 其他 | ✅ | <https://github.com/hoene/libmysofa> |
| **libbs2b** | 音频 DSP | MIT | 其他 | ✅ | <https://github.com/alexmarsev/libbs2b> |
| **libsnappy** | Snappy 压缩 (HAP) | BSD | 系统库 | ✅ | <https://github.com/google/snappy> |
| **libsoxr** | 音频重采样 | LGPL | 优化实现 | ✅ | <https://sourceforge.net/projects/soxr/> |
| **frei0r** | 视频特效插件 | GPL | 其他 | ✅ | <https://github.com/dyne/frei0r> |
| **vapoursynth** | 视频处理框架 | LGPL | 其他 | ⚠️ 需交叉编译 | <https://github.com/vapoursynth/vapoursynth> |

## 协议/网络/安全库

| 库名 | 用途 | 许可证 | 类型 | Android | GitHub URL |
|------|------|:---:|------|:---:|------------|
| **libsrt** | SRT 协议 | MPL | 优化实现 | ✅ | <https://github.com/Haivision/srt> |
| **librist** | RIST 协议 | BSD | 优化实现 | ✅ | <https://code.videolan.org/rist/librist> |
| **librtmp** | RTMP 协议 | LGPL | 其他 | ✅ | <https://rtmpdump.mplayerhq.hu/> |
| **libssh** | SFTP 协议 | LGPL | 系统库 | ✅ | <https://libssh.org/> |
| **libsmbclient** | SMB/CIFS 协议 | GPLv3 | 系统库 | ⚠️ 需交叉编译 | <https://github.com/samba-team/samba> |
| **librabbitmq** | AMQP 消息队列 | MIT | 其他 | ✅ | <https://github.com/alanxz/rabbitmq-c> |
| **libzmq** | ZeroMQ 消息传递 | MPL | 其他 | ✅ | <https://github.com/zeromq/libzmq> |
| **libxml2** | XML 解析 | MIT | 系统库 | ✅ | <https://gitlab.gnome.org/GNOME/libxml2> |
| **openssl** | TLS/加密 | Apache | 系统库 | ✅ | <https://github.com/openssl/openssl> |
| **gnutls** | TLS 传输层 | LGPL | 系统库 | ✅ | <https://gitlab.com/gnutls/gnutls> |
| **libtls** | LibreTLS | ISC | 系统库 | ✅ | <https://git.libressl.org/> |
| **mbedtls** | 嵌入式 TLS | Version3 | 系统库 | ✅ | <https://github.com/Mbed-TLS/mbedtls> |
| **gcrypt** | 加密算法库 | LGPL | 系统库 | ✅ | <https://github.com/gpg/libgcrypt> |
| **gmp** | 大数运算库 (Crypto 依赖) | Version3 | 系统库 | ✅ | <https://gmplib.org/> |
| **bzlib** | bzip2 压缩 | BSD | 系统库 | ✅ | <https://sourceware.org/git/bzip2.git> |

## 硬件/平台库

| 库名 | 用途 | 许可证 | 类型 | Android | GitHub URL |
|------|------|:---:|------|:---:|------------|
| ~~**libmfx**~~ | Intel Quick Sync Video | MIT | 硬件加速 | ❌ | <https://github.com/Intel-Media-SDK/MediaSDK> |
| ~~**libvpl**~~ | Intel oneVPL | MIT | 硬件加速 | ❌ | <https://github.com/intel/libvpl> |
| ~~**libnpp**~~ | NVIDIA NPP | Nonfree | 硬件加速 | ❌ | <https://developer.nvidia.com/cuda-toolkit> |
| ~~**ffnvcodec**~~ | NVIDIA 编解码头文件 | MIT | 硬件加速 | ❌ | <https://github.com/FFmpeg/nv-codec-headers> |
| ~~**cuda_sdk**~~ | CUDA SDK | Nonfree | 硬件加速 | ❌ | <https://developer.nvidia.com/cuda-toolkit> |
| **libglslang** | GLSL→SPIRV 编译 | BSD | 硬件加速 | ✅ | <https://github.com/KhronosGroup/glslang> |
| **libshaderc** | GLSL→SPIRV 编译 | Apache | 硬件加速 | ✅ | <https://github.com/google/shaderc> |
| **libv4l2** | Video4Linux2 | LGPL | 硬件加速 | ⚠️ 部分支持 | <https://github.com/gjasny/v4l-utils> |
| ~~**libdc1394**~~ | IEEE 1394 摄像头 | LGPL | 硬件加速 | ❌ | <https://sourceforge.net/projects/libdc1394/> |
| ~~**libiec61883**~~ | IEC 61883 | LGPL | 硬件加速 | ❌ | <https://github.com/nicxzhang/libiec61883> |
| ~~**libpulse**~~ | PulseAudio | LGPL | 系统库 | ❌ | <https://gitlab.freedesktop.org/pulseaudio/pulseaudio> |
| ~~**libjack**~~ | JACK 音频 | LGPL | 系统库 | ❌ | <https://github.com/jackaudio/jack2> |
| **libcaca** | 文本模式显示 | WTFPL | 其他 | ✅ | <https://github.com/cacalabs/libcaca> |
| ~~**libcdio**~~ | CD 抓取 | GPL | 其他 | ❌ | <https://github.com/rocky/libcdio> |
| **libdvdnav** | DVD 导航 | GPL | 其他 | ⚠️ 需交叉编译 | <https://code.videolan.org/videolan/libdvdnav> |
| **libdvdread** | DVD 读取 | GPL | 其他 | ⚠️ 需交叉编译 | <https://code.videolan.org/videolan/libdvdread> |
| **libbluray** | 蓝光读取 | LGPL | 其他 | ⚠️ 需交叉编译 | <https://code.videolan.org/videolan/libbluray> |
| **librsvg** | SVG 光栅化 | LGPL | 系统库 | ⚠️ 需交叉编译 | <https://gitlab.gnome.org/GNOME/librsvg> |
| **libcairo** | 2D 图形 | LGPL/MPL | 系统库 | ⚠️ 需交叉编译 | <https://gitlab.cairographics.org/cairo/cairo> |
| **libopenvino** | OpenVINO DNN | Apache | 硬件加速 | ✅ | <https://github.com/openvinotoolkit/openvino> |
| **libtensorflow** | TensorFlow DNN | Apache | 硬件加速 | ✅ | <https://github.com/tensorflow/tensorflow> |
| **libtorch** | PyTorch DNN | BSD | 硬件加速 | ✅ | <https://github.com/pytorch/pytorch> |
| **libopencolorio** | 色彩管理 (OCIO) | BSD | 其他 | ✅ | <https://github.com/AcademySoftwareFoundation/OpenColorIO> |
| ~~**libklvanc**~~ | VANC 处理 | BSD | 其他 | ❌ | <https://github.com/Kitchen-Registry/kvanc> |
| **opengl** | OpenGL 渲染 | — | 硬件加速 | ✅ | <https://www.opengl.org/> |
| **opencl** | OpenCL 计算 | — | 硬件加速 | ⚠️ 设备依赖 | <https://www.khronos.org/opencl/> |
| **vulkan_static** | Vulkan 静态链接 | — | 硬件加速 | ✅ | <https://www.vulkan.org/> |
| ~~**libdrm**~~ | DRM/KMS 接口 | MIT | 硬件加速 | ❌ | <https://gitlab.freedesktop.org/mesa/drm> |
| ~~**decklink**~~ | Blackmagic 采集卡 | Nonfree ✱ | 硬件加速 | ❌ | <https://www.blackmagicdesign.com/> |
| ~~**avisynth**~~ | AviSynth 脚本 (Win) | GPL | 优化实现 | ❌ | <https://github.com/AviSynth/AviSynthPlus> |
| **ohcodec** | OpenHarmony 硬件编解码 | Apache | 硬件加速 | ⚠️ HarmonyOS | — |
| **zlib** | 压缩库 | zlib | 系统库 | ✅ | <https://github.com/madler/zlib> |
| **SDL2** | 多媒体框架 | zlib | 系统库 | ✅ | <https://github.com/libsdl-org/SDL> |

> ✱ Nonfree：启用后 FFmpeg 不可商用分发。

## 许可证约束速查

启用以下任意库将强制 FFmpeg 编译为对应许可证变体：

| 许可证 | 库列表 |
|:------:|--------|
| **GPL** | libx264, libx265, libxvid, libxavs, libxavs2, libdavs2, libvidstab, librubberband, libcdio, libdvdnav, libdvdread, frei0r, avisynth |
| **Nonfree** | libfdk_aac, libmpeghdec, decklink, libnpp |
| **Version3** | libopencore_amrnb, libopencore_amrwb, libvo_amrwbenc, libaribb24, liblensfun, gmp, mbedtls, rkmpp |
| **GPLv3** | libsmbclient |

## configure 对应关系

文档中库名与 `configure` 中的 `--enable-*` 选项名一致。部分 `configure` 选项对应关系：

| 文档分类 | configure 列表归属 | 示例 |
|---------|-------------------|------|
| 视频/音频/字幕/滤镜/协议库 | `EXTERNAL_LIBRARY_LIST` | `--enable-libx264` |
| 硬件加速库 | `HWACCEL_LIBRARY_LIST` | `--enable-libmfx` |
| 自动检测平台库 | `HWACCEL_AUTODETECT_LIBRARY_LIST` | `--enable-vulkan` |
| 自动检测系统库 | `EXTERNAL_AUTODETECT_LIBRARY_LIST` | `--enable-zlib` |

## FFmpeg 内置平台特性

| 特性 | configure 选项 | 用途 | Android | 说明 |
|------|:---:|------|:---:|------|
| **MediaCodec** | `mediacodec` | Android 硬件编解码 | ✅ | Android 原生硬件编解码 API |
| **JNI** | `jni` | Java Native Interface | ✅ | Android NDK 内置 |
| **Vulkan** | `vulkan` | GPU 计算/图形 API | ✅ | 跨平台 GPU API |
| **VideoToolbox** | `videotoolbox` | Apple 硬件编解码 | ❌ | macOS/iOS 专用 |
| **VAAPI** | `vaapi` | Intel/AMD 视频加速 | ❌ | Linux 桌面专用 |
| **VDPAU** | `vdpau` | NVIDIA 视频加速 | ❌ | Linux 桌面专用 |
| **D3D11VA** | `d3d11va` | Direct3D 11 视频加速 | ❌ | Windows 专用 |
| **D3D12VA** | `d3d12va` | Direct3D 12 视频加速 | ❌ | Windows 专用 |
| **DXVA2** | `dxva2` | DirectX 视频加速 | ❌ | Windows 专用 |
| **NVDEC/NVENC** | `nvdec` / `nvenc` | NVIDIA 硬件编解码 | ❌ | 需要 NVIDIA GPU |
| **CUDA** | `cuda` / `cuda_sdk` | NVIDIA GPU 计算 | ❌ | 需要 NVIDIA GPU |
| **QSV** | `libmfx` / `libvpl` | Intel Quick Sync | ❌ | Intel CPU 专用 |
| **ALSA** | `alsa` | Linux 音频 | ❌ | Linux 桌面专用 |
| **PulseAudio** | `libpulse` | Linux 音频 | ❌ | Linux 桌面专用 |
| **X11/XCB** | `libxcb` / `xlib` | X Window 系统 | ❌ | Linux 桌面专用 |
| **MMAL** | `mmal` | Raspberry Pi 视频 | ❌ | 树莓派专用 |
| **OpenMAX IL** | `omx` | 嵌入式多媒体 | ❌ | 树莓派/嵌入式 |
| **RKMPP** | `rkmpp` | Rockchip 媒体处理 | ❌ | Rockchip SoC 专用 |
| **v4l2_m2m** | `v4l2_m2m` | V4L2 内存到内存 | ⚠️ | Linux 内核 API |
| **AMF** | `amf` | AMD GPU 编码 | ❌ | AMD GPU 专用 |

## 图例

| 符号 | 含义 |
|:---:|------|
| ✅ | 完全支持 Android |
| ⚠️ | 需要交叉编译或部分支持 |
| ❌ | 不支持 Android |

## 统计

- ***参考实现***: 25 个 (标准组织/官方参考代码)
- **优化实现**: 18 个 (针对性能优化的第三方实现)
- **硬件加速**: 14 个 (GPU/DSP/专用硬件加速)
- **系统库**: 14 个 (操作系统/平台库)
- **其他**: 29 个 (其他用途)

**总计**: 100+ 个可选三方库

### Android 兼容性统计

- ✅ 完全支持: 约 **75** 个
- ⚠️ 需交叉编译: 约 **8** 个 (smbclient, dvdnav, dvdread, bluray, librsvg, cairo, v4l2, dc1394)
- ❌ 不支持: 约 **17** 个 (Apple/Windows/NVIDIA 专属硬件库)

## 当前编译配置 (Android aarch64)

已启用 (FFmpeg vvdecSupport_8.1, NDK r29, API 31):
- **libvvdec** - H.266/VVC 参考解码器
- **libplacebo** - GPU 视频渲染 (Vulkan)
- **openssl** - TLS/HTTPS 支持
- **libfreetype** - 字体光栅化 (drawtext 滤镜)
- **libharfbuzz** - 复杂文字整形 (drawtext 滤镜)
- **libfribidi** - 双向文本处理 (drawtext/libass 依赖)
- **libass** - ASS/SSA 字幕渲染
- **Vulkan** - GPU 计算/图形 API
- **MediaCodec** - Android 硬件编解码
- **JNI** - Java Native Interface
- **SDL2** - 多媒体框架
- **zlib** - 压缩库
- **iconv** - 字符编码转换

## Termux 编译配置 lib 依赖分析

> 基于 FFmpeg 8.1.1 (Termux Android aarch64) 的 `configure` 提取，逐个分析所有 `--enable-lib*` 和 `--enable-lcms2` 选项。

| 库名 | 用途 | 版本要求 | FFmpeg 内依赖的滤镜/编解码器 | lib 之间的依赖 | 官方地址 |
|------|------|---------|---------------------------|--------------|---------|
| **lcms2** | ICC 色彩配置 | >= 2.13 | iccdetect, iccgen 滤镜; avcodec extralibs | 无 | https://github.com/mm2/Little-CMS |
| **libaom** | AV1 编解码 | >= 2.0.0 | libaom_av1_encoder, libaom_av1_decoder | 无 | https://aomedia.googlesource.com/aom |
| **libass** | ASS/SSA 字幕渲染 | >= 0.11.0 | ass 滤镜, subtitles 滤镜 | freetype2, fribidi, harfbuzz, fontconfig, libunibreak | https://github.com/libass/libass |
| **libbluray** | 蓝光播放 | (pkg-config) | bluray demuxer | 无 | https://www.videolan.org/developers/libbluray.html |
| **libbs2b** | Bauer 立体声 | (pkg-config) | bs2b 滤镜 | 无 | http://bs2b.sourceforge.net/ |
| **libdav1d** | AV1 解码 | >= 1.0.0 | libdav1d_decoder | 无 | https://code.videolan.org/videolan/dav1d |
| **libfontconfig** | 字体查找/配置 | (pkg-config) | drawtext 滤镜(间接) | freetype2, expat | https://www.freedesktop.org/wiki/Software/fontconfig/ |
| **libfreetype** | 字体光栅化 | (pkg-config) | drawtext 滤镜 | 无 | https://freetype.org/ |
| **libfribidi** | 双向文本 | (pkg-config) | drawtext 滤镜(间接) | 无 | https://github.com/fribidi/fribidi |
| **libglslang** | GLSL→SPIR-V 编译 | >= 16 (推荐) | 所有 vulkan 滤镜 | libstdc++ | https://github.com/KhronosGroup/glslang |
| **libgme** | 游戏音乐播放 | (pkg-config) | libgme_demuxer | libstdc++ | https://bitbucket.org/mpyne/game-music-emu |
| **libharfbuzz** | 复杂文字整形 | (pkg-config) | drawtext 滤镜 | freetype2(可选) | https://github.com/harfbuzz/harfbuzz |
| **libmp3lame** | MP3 编码 | >= 3.98.3 | libmp3lame_encoder | libm | https://lame.sourceforge.io/ |
| **libopencore-amrnb** | AMR-NB 编解码 | (pkg-config) | libopencore_amrnb_encoder/decoder | 无 | https://sourceforge.net/projects/opencore-amr/ |
| **libopencore-amrwb** | AMR-WB 解码 | (pkg-config) | libopencore_amrwb_decoder | 无 | https://sourceforge.net/projects/opencore-amr/ |
| **libopenmpt** | 模块音乐播放 | >= 0.2.6557 | libopenmpt_demuxer | libstdc++ | https://lib.openmpt.org/ |
| **libopus** | Opus 音频编解码 | (pkg-config) | libopus_encoder/decoder | 无 | https://opus-codec.org/ |
| **libplacebo** | GPU 视频处理 | >= 5.229.0 | libplacebo 滤镜 | vulkan | https://code.videolan.org/videolan/libplacebo |
| **librav1e** | AV1 编码(Rust) | >= 0.5.0 | librav1e_encoder | 无 | https://github.com/xiph/rav1e |
| **librubberband** | 音频变速变调 | >= 1.8.1 | rubberband 滤镜 | libstdc++, libsamplerate | https://breakfastquay.com/rubberband/ |
| **libsoxr** | 音频重采样 | (pkg-config) | soxr 滤镜(替代 swresample) | 无 | https://sourceforge.net/projects/soxr/ |
| **libsrt** | SRT 网络协议 | >= 1.3.0 | srt 输入/输出 | openssl(可选) | https://github.com/Haivision/srt |
| **libssh** | SFTP 协议 | >= 0.6.0 | sftp 输入/输出 | openssl | https://www.libssh.org/ |
| **libsvtav1** | AV1 编码(Intel) | >= 0.9.0 | libsvtav1_encoder | 无 | https://gitlab.com/AOMediaCodec/SVT-AV1 |
| **libtheora** | Theora 视频编码 | (pkg-config) | libtheora_encoder | libogg | https://www.theora.org/ |
| **libv4l2** | V4L2 设备访问 | (pkg-config) | v4l2 输入设备 | 无 | https://linuxtv.org/ |
| **libvidstab** | 视频稳定 | >= 0.98 | vidstabdetect, vidstabtransform 滤镜 | 无 | https://github.com/georgmartius/vid.stab |
| **libvmaf** | 视频质量评估 | >= 2.0.0 | libvmaf 滤镜 | 无 | https://github.com/Netflix/vmaf |
| **libvo-amrwbenc** | AMR-WB 编码 | (pkg-config) | libvo_amrwbenc_encoder | 无 | https://sourceforge.net/projects/opencore-amr/ |
| **libvorbis** | Vorbis 音频编码 | (pkg-config) | libvorbis_encoder/decoder | libogg | https://xiph.org/vorbis/ |
| **libvpx** | VP8/VP9 编解码 | >= 1.4.0 | libvpx_vp8/vp9_encoder/decoder | libm, pthreads | https://chromium.googlesource.com/webm/libvpx |
| **libwebp** | WebP 编解码 | >= 0.2.0 | libwebp_encoder, libwebp_anim_encoder | 无 | https://developers.google.com/speed/webp/ |
| **libx264** | H.264 编码 | (pkg-config) | libx264_encoder, libx264rgb_encoder | libm, pthreads | https://www.videolan.org/developers/x264.html |
| **libx265** | H.265/HEVC 编码 | (pkg-config) | libx265_encoder | libm, pthreads | https://www.videolan.org/developers/x265.html |
| **libxml2** | XML 解析 | (pkg-config) | dash_demuxer, imf_demuxer | 无 | https://gitlab.gnome.org/GNOME/libxml2 |
| **libxvid** | MPEG-4 ASP 编码 | (pkg-config) | libxvid_encoder | 无 | https://www.xvid.com/ |
| **libzimg** | 图像缩放/色彩空间 | >= 2.7.0 | zscale 滤镜 | 无 | https://github.com/sekrit-twc/zimg |
| **libzmq** | ZeroMQ 消息传递 | >= 4.2.1 | zmq, azmq 滤镜 | 无 | https://zeromq.org/ |

## 非 lib 配置选项对比分析

> 对比 Termux (FFmpeg 8.1.1, API 24) 与自定义编译 (vvdecSupport_8.1, NDK r29, API 31) 的非 lib configure 选项。

| 选项 | 分类 | 含义 | Termux | 自定义 | 说明 |
|------|------|------|:---:|:---:|------|
| `--enable-debug` | 构建模式 | 启用调试信息 | ❌ | ✅ | 保留符号便于调试 |
| `--disable-doc` | 文档 | 禁用所有文档 | ❌ | ✅ | 含 html/man/pod/txt |
| `--disable-htmlpages` | 文档 | 禁用 HTML 文档 | ❌ | ✅ | |
| `--disable-manpages` | 文档 | 禁用 man 文档 | ❌ | ✅ | |
| `--disable-podpages` | 文档 | 禁用 POD 文档 | ❌ | ✅ | |
| `--disable-txtpages` | 文档 | 禁用文本文档 | ❌ | ✅ | |
| `--enable-cross-compile` | 工具链 | 声明交叉编译 | ✅ | ✅ | 跳过本机可执行测试 |
| `--arch=aarch64` | 工具链 | ARM 64-bit 架构 | ✅ | ✅ | |
| `--cc` | 工具链 | C 编译器 | android-clang | android31-clang | API level 不同 |
| `--cxx` | 工具链 | C++ 编译器 | android-clang++ | android31-clang++ | 同上 |
| `--as` | 工具链 | 汇编器 | android-clang | (默认) | 自定义未指定 |
| `--nm` | 工具链 | 符号表工具 | llvm-nm | (默认) | 自定义未指定 |
| `--ar` | 工具链 | 归档工具 | llvm-ar | llvm-ar | |
| `--ranlib` | 工具链 | 索引生成 | llvm-ranlib | llvm-ranlib | |
| `--strip` | 工具链 | 符号剥离 | llvm-strip | llvm-strip | |
| `--cross-prefix` | 工具链 | 交叉编译前缀 | aarch64-linux-android- | ❌ | 自定义用全路径 |
| `--sysroot` | 工具链 | 交叉编译 sysroot | ❌ | ✅ NDK r29 | Termux 用 pkg-config 替代 |
| `--pkg-config` | 工具链 | pkg-config 路径 | Termux cache 路径 | (默认) | |
| `--target-os=android` | 工具链 | 目标 OS | ✅ | ✅ | |
| `--enable-gpl` | 许可证 | 允许 GPL 代码 | ✅ | ✅ | |
| `--enable-version3` | 许可证 | 升级到 (L)GPL v3 | ✅ | ✅ | |
| `--enable-nonfree` | 许可证 | 允许非自由代码 | ❌ | ✅ | 不可分发 |
| `--enable-shared` | 构建类型 | 构建 .so | ✅ | ✅ | |
| `--disable-static` | 构建类型 | 不构建 .a | ✅ | ✅ | |
| `--disable-symver` | 构建类型 | 禁用符号版本控制 | ✅ | ✅ | Bionic 限制 |
| `--enable-jni` | 平台特性 | JNI 支持 | ✅ | ✅ | Android 必须 |
| `--enable-mediacodec` | 平台特性 | 硬件编解码 | ✅ | ✅ | 依赖 mediandk |
| `--enable-vulkan` | 平台特性 | GPU API | ✅ | ✅ | 依赖 threads |
| `--enable-opencl` | 平台特性 | OpenCL GPU 计算 | ✅ | ❌ | |
| `--enable-neon` | 平台特性 | ARM NEON SIMD | ✅ | ✅ | aarch64 默认 |
| `--enable-openssl` | 网络/TLS | TLS/HTTPS 支持 | ✅ | ❌ | |
| `--disable-indevs` | 设备控制 | 禁用所有输入设备 | ✅ | ❌ | 减少体积 |
| `--disable-outdevs` | 设备控制 | 禁用所有输出设备 | ✅ | ❌ | 减少体积 |
| `--enable-indev=lavfi` | 设备控制 | 保留 lavfi 输入 | ✅ | ❌ | 滤镜图输入源 |
| `--disable-libfdk-aac` | 禁用 lib | 禁用 FDK-AAC | ✅ | ❌ | Nonfree 许可 |
| `--extra-cflags` | 编译参数 | 额外 C 编译选项 | ❌ | ✅ `-DANDROID` 等 | vulkan/libplacebo 路径 |
| `--extra-ldflags` | 链接参数 | 额外链接选项 | ❌ | ✅ 16KB 对齐等 | `-lc++_shared` |
| `--extra-libs` | 链接参数 | 额外链接库 | ✅ `-landroid-glob` | ❌ | Termux glob 补丁 |
| `--prefix` | 安装路径 | 安装根目录 | Termux usr | 项目内 build_ffmpeg | |
| `--libdir` | 安装路径 | 静态库目录 | (默认) | `lib/arm64-v8a` | |
| `--shlibdir` | 安装路径 | 动态库目录 | (默认) | `libs/arm64-v8a` | Android 打包用 |
| `--pkgconfigdir` | 安装路径 | pkg-config 目录 | (默认) | `libs/arm64-v8a/pkgconfig` | |
| `--bindir` | 安装路径 | 可执行文件目录 | (默认) | `bins/arm64-v8a` | |

### 关键差异总结

- **API level**: Termux 用 API 24，自定义用 API 31
- **许可证**: Termux 启用 GPL+Version3，自定义额外启用 nonfree（不可分发）
- **lib 数量**: Termux 启用 38 个 lib，自定义启用 7 个（vvdec/placebo/openssl/freetype/harfbuzz/fribidi/libass）
- **调试**: 自定义启用 debug，Termux 为 release 构建
- **网络**: 两者均启用 openssl；Termux 额外启用 opencl，自定义未启用
- **字幕**: 自定义启用 libass + fribidi + freetype + harfbuzz（drawtext/ass 滤镜支持），Termux 也启用但依赖更多
- **设备**: Termux 禁用 indevs/outdevs 并仅保留 lavfi，自定义未做限制