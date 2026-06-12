# FFmpeg 各版本改动记录

本文档梳理 FFmpeg 从 0.9 到 8.1 各主要版本的重大新增功能、编码器、解码器、滤镜、协议支持等变更，按发布时间倒序排列。

**参考资料：**
- [FFmpeg 官方 Changelog](https://ffmpeg.org/changelog.html)
- [FFmpeg 官方 News](https://ffmpeg.org/index.html)
- [FFmpeg 官方 Download / Release info](https://ffmpeg.org/download.html)
- [Wikipedia: FFmpeg](https://en.wikipedia.org/wiki/FFmpeg)

---

## FFmpeg 8.1 "Hoare" (2026年3月)

### 新增解码器
- xHE-AAC Mps212 解码支持（实验性）
- MPEG-H 3D Audio 解码（通过 libmpeghdec）
- JPEG-XS 编解码器（通过 libsvtjpegxs）

### 新增滤镜
- `drawvg`：矢量图形绘制滤镜（via libcairo）
- `vpp_amf`：AMD AMF 视频处理滤镜
- `scale_d3d12`、`mestimate_d3d12`、`deinterlace_d3d12`：D3D12 加速滤镜

### 新增硬件支持
- D3D12：H.264/AV1 编码器、scale/mestimate/deinterlace 滤镜
- Rockchip H.264/HEVC 硬件编码
- Vulkan：ProRes 编码/解码、DPX 解码

### 协议/封装
- `hxvs` 解封装器（HXVS/HXVT IP摄像机格式）
- LCEVC 元数据 bitstream filter
- LCEVC parser
- LCEVC enhancement layer exporting in MPEG-TS

### 其他
- IAMF：Projection mode Ambisonic Audio Elements 封装/解封装
- EXIF 元数据解析
- `gfxcapture`：Windows.Graphics.Capture 窗口/显示器捕获
- ffprobe `-codec` 选项
- ffmpeg CLI tiled HEIF 支持
- Vulkan compute codec optimizations
- swscale Vulkan support
- 移除旧的 HLS 协议处理程序

---

## FFmpeg 8.0 "Huffman" (2025年8月)

### 新增原生解码器
- **APV**（含编码器 via libopenapv、parser、原始比特流封装/解封装、MP4/ISOBMFF 封装）
- **ProRes RAW**（含 Vulkan hwaccel）
- **RealVideo 6.0**
- **Sanyo LD-ADPCM**
- **G.728**

### Vulkan Compute Codec（重大新功能）
首次基于纯 Vulkan compute 实现的新一类编解码器：
- **FFv1**（编码+解码）
- **ProRes RAW**（仅解码）
- 使用 hwaccel API，无需特殊配置
- 支持任何 Vulkan 1.3 实现

### VVC 改进
- IBC（Inter Block Copy）
- ACT（Adaptive Color Transform）
- Palette Mode
- VAAPI 解码器
- Matroska 封装支持

### 新增硬件加速
- Vulkan VP9 解码
- Vulkan AV1 编码
- OpenHarmony H264/5 编解码

### 新增封装/协议
- MCC、G.728、Whip
- Enhanced FLV v2：多轨音视频、现代编解码器支持
- CENC AV1 MP4 支持
- Animated JPEG XL 编码（via libjxl）

### 新增滤镜
- `colordetect`
- `pad_cuda`
- `scale_d3d11`
- **Whisper**（语音识别）

### 其他
- yasm 支持移除（需使用 nasm）
- OpenSSL < 1.1.0 移除支持，随后 OpenSSL < 1.1.1 也移除支持
- OpenMAX 编码器标记废弃
- `libx265` alpha layer 编码
- ADPCM IMA Xbox 解码器
- HDR10+ metadata passthrough（libaom-av1）

---

## FFmpeg 7.1 "Péter" (2024年9月)

### VVC 解码器稳定化
VVC 解码器从实验性升级为稳定版，已被广播标准化机构采用

### 新增解码器
- **xHE-AAC 原生解码器**（支持 USAC）
- **MV-HEVC 解码**（立体声编码工具）
- **LC-EVC 解码**（增强元数据层，通过外部库 LCEVCdec）

### Vulkan 编码
- **Vulkan H.264 编码器**
- **Vulkan HEVC 编码器**
- 与 VAAPI 实现功能对等

### 新增功能
- **Cropping 元数据**：Matroska/MP4 支持
- **全范围图像（Full-range images）改进**：颜色范围系统重构，YUV colorspace negotiation（废弃 YUVJ 像素格式）
- IAMF 结构配置（`-stream_group` 选项）
- 多线程 ffmpeg CLI 滤镜链
- **FFV1 parser**
- D3D12VA HEVC 编码器
- perlin video source
- RCWT 字幕解封装器
- LCEVC filter 及 H.26x/MP4 数据导出

### 新增滤镜
- `pad_vaapi`、`drawbox_vaapi`
- LC3/LC3plus 编解码（通过 liblc3）

---

## FFmpeg 7.0 "Dijkstra" (2024年4月)

### 重大变化
- **不兼容变更**：移除 6.0 之前废弃的 API
- **移除旧的位掩码通道布局 API**：替换为 `AVChannelLayout` API（支持自定义通道顺序、Ambisonics）
- **需要 C11 兼容编译器**

### 新增解码器
- **VVC 解码器（实验性）**
- **EVC 解码/编码**（通过 libxevd/libxeve）

### IAMF 支持
- `libavformat` 支持读写 IAMF（Immersive Audio）文件
- ffmpeg CLI `-stream_group` 选项配置 IAMF 结构

### CLI 多线程
- ffmpeg 命令行工具全面重构
- 解复用、解码、滤镜、编码、复用全部并行运行
- 改善吞吐量、CPU利用率、延迟

### 新增滤镜/功能
- `DXV` DXT1 编码器
- `LEAD MCMP` 解码器
- `QOA` 解码器
- IAMF 原始解封装/复用
- D3D12VA 加速 H264/HEVC/VP9/AV1/MPEG-2/VC1 解码
- `quirc` 二维码识别滤镜
- `qrencode` 二维码生成
- **DVD-Video 解封装器**（通过 libdvdnav/libdvdread）
- **HEIF/AVIF 静态图像支持**（含 tiled 图像）
- Dolby Vision profile 10（AV1）
- HDR10 metadata passthrough（libx264/libx265/libsvtav1）
- AFGS1（AOMedia Film Grain Synthesis）
- dnn filter libtorch 后端
- ffplay hwaccel 解码（Vulkan/libplacebo）
- RISC-V 和 Loongarch 优化

---

## FFmpeg 6.1 "Heaviside" (2023年11月)

### 新增解码器
- `libaribcaption` 解码器
- Playdate 视频解码器/解封装器
- RivaTuner 视频解码器
- vMix 视频解码器
- Essential Video Coding 解析器/复用器/解封装器

### Vulkan 解码（重大）
- Vulkan 硬件加速解码：H264、HEVC、AV1
- 首个跨平台、厂商通用的解码加速 API
- 首个多线程硬件解码 API

### 新增滤镜
- `color_vulkan`、`bwdif_vulkan`、`nlmeans_vulkan`
- `xfade_vulkan`
- `bwdif_cuda`

### 编码器
- VAAPI AV1 编码器
- Microsoft RLE 视频编码器
- Essential Video Coding 帧合并 bsf

### 其他
- FFT/MDCT/DCT/DST 实现完全替换为 libavutil/tx（更快的实现）
- RISC-V 优化
- VAAPI 支持扩展到 libva-win32（Windows）

---

## FFmpeg 6.0 "Von Neumann" (2023年2月)

### 发布策略变更
- 所有主版本将升级 ABI 版本
- 废弃 API 将在 3 个版本后移除
- 计划每年一个新主版本

### 新增解码器
- Bonk、RKA、Radiance、SC-4、APAC、VQC、WavArc 等多种格式

### MediaCodec 支持
- 通过 NDKMediaCodec 的 MediaCodec 解码器
- MediaCodec 编码器

### 新增滤镜
- `ddagrab`（桌面复制视频捕获）
- `showcwt`、`backgroundkey`、`ssim360`
- `adrc`、`afdelaysrc`

### 其他
- `libplacebo` 滤镜集成
- DoVi（Dolby Vision）支持，包括 tonemapping 和复用
- Radiance HDR 图像支持
- nvenc AV1 编码支持
- QSV AV1 编码器
- oneVPL 支持
- VAAPI 10/12bit 422/444 HEVC/VP9 编解码
- CrystalHD 解码器废弃

---

## FFmpeg 5.1 "Riemann" (2022年7月)

### 新增滤镜
- `dialogue enhance`（对话增强）
- `pixelize`（像素化）
- `colormap`、`colorchart`
- `blurdetect`、`chromakey_cuda`
- `bilateral_cuda`
- `SITI`、`feedback`、`multiply`

### 新增协议/格式
- IPFS/IPNS 网关支持
- QOI 图像格式
- PHM 图像格式

### 其他
- DFPWM 音频编解码器及原始复用/解封装
- VDPAU AV1 hwaccel
- PGS 字幕帧合并 bitstream filter
- Vizrt Binary Image 编解码器
- pcm-bluray 编码器
- 移除废弃的 XvMC hwaccel

---

## FFmpeg 5.0 "Lorentz" (2022年1月)

### API 重大变更
- **移除旧的编码/解码 API**：替换为 N:M API
- **移除 libavresample 库**

### 新增解码器
- **VideoToolbox VP9 硬件加速**
- **VideoToolbox ProRes** 硬件加速/编码
- AV1 低开销比特流格式复用器
- speex 解码器
- MSN Siren 解码器

### Vulkan 改进
- `libplacebo` 滤镜
- `vflip_vulkan`、`hflip_vulkan`、`flip_vulkan`

### 其他
- IMF demuxer（实验性）
- Loongarch 支持
- swscale slice threading
- bitpacked 编码器
- RTP packetizer for uncompressed video (RFC 4175)

### DoVi 支持
- DoVi RPU 解析（libavcodec/dovi_rpu.c，by Niklas Haas）
- HEVC 解码器导出 DoVi RPU side data
- Dolby Vision ISOM 配置解析/写入（libavformat/dovi_isom.c）
- Matroska DoVi 支持
- Tonemapping 和 Remuxing

---

## FFmpeg 4.4 "Rao" (2021年4月)

### 硬件解码增强
- VDPAU 加速 HEVC 10/12bit 解码
- VDPAU 加速 VP9 10/12bit 解码
- NVDEC AV1 硬件加速
- DXVA2/D3D11VA 硬件加速 AV1 解码

### AV1 支持
- **SVT-AV1** 编码支持
- **AV1 VAAPI** 解码器

### 新增滤镜
- `chromanr`、`speechnorm`、`shufflepixels`
- `tmidequalizer`、`estdif`、`epx`
- `shear`、`kirsch`、`colortemperature`

---

## FFmpeg 4.3 "4:3" (2020年6月)

### Vulkan 支持（重大）
- 首个 Vulkan 支持
- `avgblur_vulkan`、`overlay_vulkan`、`scale_vulkan`、`chromaber_vulkan` 滤镜

### AV1 进展
- **librav1e** AV1 编码支持
- AV1 帧合并 bitstream filter
- AV1 Annex B 解封装器

### 新增滤镜
- `v360`、`photosensitivity`
- `xfade`、`xfade_opencl`
- `scdet`、`gradients`

### QSV 改进
- QSV 加速 VP9 解码/编码
- QSV 加速 MJPEG 解码

---

## FFmpeg 4.2 "Ada" (2019年8月)

### AV1 重大进展
- **libdav1d AV1 解码支持**

### 新增滤镜
- `tpad`、`dedot`
- `chromashift`、`rgbashift`
- `freezedetect`、`lagfun`

### 其他
- HEVC 4:4:4 内容解码支持（nvdec/cuviddec/vdpau）
- libaribb24 ARIB STD-B24 字幕支持

---

## FFmpeg 4.1 "al-Khwarizmi" (2018年11月)

### AVS2 支持
- **AVS2 视频解码**（通过 libdavs2）
- **AVS2 视频编码**（通过 libxavs2）

### 新增滤镜
- `deblock`、`tmix`、`amplify`
- `fftdnoiz`、`bm3d`（Block-Matching 3D 去噪）
- `yadif_cuda`

### 机器学习
- **libtensorflow** DNN 后端（srcnn 等）

---

## FFmpeg 4.0 "Wu" (2018年4月)

### 重大删除
- **移除 ffserver 程序**
- 移除 ffmenc/ffmdec 复用器/解封装器
- 移除 VDA（使用 VideoToolbox 替代）
- 移除 Windows XP 支持

### AV1 支持
- **libaom AV1 支持**

### 新增硬件加速
- NVIDIA NVDEC 加速 H264/HEVC/MJPEG/MPEG-1/2/4/VC1/VP8/9 解码
- AMD AMF H.264/HEVC 编码器
- VAAPI MJPEG/VP8 解码
- VideoToolbox HEVC 编码器/硬件加速

### 新增编解码
- **原生 aptX/aptX HD 编解码器**
- **原生 SBC 编解码器**
- **Codec2** 编解码（通过 libcodec2）
- **MagicYUV** 编码器（实验性）

### 协议
- **SRT 协议**（通过 libsrt）

---

## FFmpeg 3.4 "Cantor" (2017年10月)

### 新增滤镜
- `deflicker`、`doubleweave`
- `lumakey`、`pixscope`
- `afir`、`crossfeed`、`surround`
- `libvmaf`

### CUDA 支持
- `scale_cuda`
- `overlay_cuda`
- `CUDA thumbnail` 滤镜

### 其他
- VP9 tile threading 支持
- KMS 屏幕捕获
- Rockchip MPP 硬件解码

---

## FFmpeg 3.3 "Hilbert" (2017年4月)

### 视频编解码
- Apple Pixlet 解码器
- NewTek SpeedHQ 解码器
- DNxHR 444 和 HQX 编码

### 音频
- **原生 Opus 编码器**

### 视频处理
- **支持球形视频**
- `premultiply` 滤镜
- `threshold` 滤镜

---

## FFmpeg 3.1 "Laplace" (2016年6月)

### 硬件加速
- DXVA2 加速 HEVC Main10 解码
- **VAAPI 加速 H.264/HEVC/MJPEG 编码**
- CUDA CUVID H264/HEVC 解码
- VideoToolbox H.264 编码器

### MediaCodec
- **MediaCodec H264 解码**

### 新增滤镜
- `fieldhint`、`loop`、`firequalizer`
- `remap`、`colorspace`
- `loudnorm`

---

## FFmpeg 3.0 "Einstein" (2016年2月)

### AAC 编码器稳定化
- **原生 FFmpeg AAC 编码器移除实验性标签，宣告稳定**

### 移除的编码器
- **移除 libvo-aacenc**
- **移除 libaacplus**

### 硬件加速
- VP9 DXVA2/VA-API 硬件加速

### 其他
- Cineform HD 解码器
- 新 DCA 解码器（支持 DTS-HD 扩展）
- 30+ 新滤镜

---

## FFmpeg 2.8 (2015年)

### 硬件加速
- Intel QSV 加速 MPEG-2/HEVC 编码
- VAAPI HEVC 硬件加速
- VDPAU HEVC 硬件加速

### 滤镜
- `colorkey`、`deband`
- `ssim`、`dynaudnorm`
- `reverse`、`deband`

---

## VVC (H.266) 支持时间线

VVC（Versatile Video Coding，又称 H.266）是 HEVC/H.265 的后续标准。

| 版本 | 代号 | VVC 支持内容 |
|------|------|------------|
| **6.1** | Heaviside | Raw VVC bitstream parser, muxer and demuxer；VVC 元数据编辑 bsf；VVC MP4→Annex B 转换 bsf |
| **7.0** | Dijkstra | **VVC 解码器（实验性）** |
| **7.1** | Péter | VVC 解码器稳定化；VVC 兼容 DVB 测试内容；VVC 编码支持（via libvvenc）；Intel QSV 加速 VVC 解码 |
| **8.0** | Huffman | VVC VAAPI 解码器；VVC in Matroska；VVC 解码器支持 SCC（Screen Content Coding）：IBC、Palette Mode、ACT |
| **8.1** | Hoare | 无 VVC 新增；其他改进见 Vulkan 章节 |

### VVC 关键技术点

- **IBC（Inter Block Copy）**：块复制，用于屏幕内容编码
- **ACT（Adaptive Color Transform）**：自适应颜色变换
- **Palette Mode**：调色板模式
- **libvvenc**：VVC 编码外部库
- **QSV 加速**：Intel Quick Sync Video 硬件加速解码
- **VAAPI 加速**：Linux VAAPI 硬件加速解码
- **Matroska 支持**：VVC 比特流封装支持

---

## Vulkan 支持时间线

Vulkan 是跨平台、开放的 GPU API 集，FFmpeg 逐步将其引入作为硬件加速的通用方案。

| 版本 | 代号 | Vulkan 新增内容 |
|------|------|----------------|
| **4.3** | 4:3 | **首个 Vulkan 支持**：`avgblur_vulkan`、`overlay_vulkan`、`scale_vulkan`、`chromaber_vulkan` 滤镜 |
| **5.0** | Lorentz | `libplacebo` 集成；`vflip_vulkan`、`hflip_vulkan`、`flip_vulkan` |
| **6.1** | Heaviside | **Vulkan 解码 hwaccel**（H264、HEVC、AV1）；`color_vulkan`、`bwdif_vulkan`、`nlmeans_vulkan`、`xfade_vulkan` 滤镜 |
| **7.0** | Dijkstra | D3D12VA 加速解码（H264/HEVC/VP9/AV1/MPEG-2/VC1）—— 注：D3D12VA 为独立 API，非 Vulkan |
| **7.1** | Péter | **Vulkan H.264 编码器**；**Vulkan HEVC 编码器**；与 VAAPI 功能对等 |
| **8.0** | Huffman | **Vulkan Compute Codec 新类**：FFv1（编码+解码）、ProRes RAW（仅解码）；**Vulkan VP9 解码**；**Vulkan AV1 编码**；OpenHarmony H264/5 编解码 |
| **8.1** | Hoare | **ProRes Vulkan 编码**；DPX Vulkan 解码；**Vulkan Compute Codec 优化**（移除运行时 GLSL 编译，加速初始化）；swscale Vulkan 支持；`v360_vulkan` 滤镜；`drawvg` via libcairo |

### Vulkan Compute Codec（8.0 重大新功能）

8.0 引入了**基于纯 Vulkan compute 的编解码器**，这是全新的技术方向：

- **与传统 hwaccel 的区别**：传统方案依赖特定厂商 ASIC 加速器；Vulkan Compute 使用计算着色器，在任何 Vulkan 1.3 实现上都能运行
- **使用的 API**：与现有 hwaccel API 相同，用户无需特殊配置
- **支持状态**（截至 8.0）：

| Codec | 编码 | 解码 |
|-------|------|------|
| FFv1 | ✅ `ffv1_vulkan` | ✅ |
| ProRes RAW | 即将推出 | ✅ |
| ProRes | ✅（8.1） | ✅（8.1） |
| VC-2 | 即将推出 | 即将推出 |

### Vulkan 解码加速支持

| Codec | Vulkan 解码 | 最低版本 |
|-------|------------|---------|
| H.264 | ✅ | 6.1 |
| HEVC | ✅ | 6.1 |
| AV1 | ✅ | 6.1 |
| VP9 | ✅ | 8.0 |
| MPEG-2 | ✅（D3D12VA）| 7.0 |
| VC-1 | ✅（D3D12VA）| 7.0 |

### Vulkan 滤镜一览

| 滤镜 | 版本 | 说明 |
|------|------|------|
| `avgblur_vulkan` | 4.3 | 平均模糊 |
| `overlay_vulkan` | 4.3 | Vulkan 叠加 |
| `scale_vulkan` | 4.3 | Vulkan 缩放 |
| `chromaber_vulkan` | 4.3 | 色度混合 |
| `vflip_vulkan` | 5.0 | 垂直翻转 |
| `hflip_vulkan` | 5.0 | 水平翻转 |
| `flip_vulkan` | 5.0 | 翻转 |
| `color_vulkan` | 6.1 | 颜色调整 |
| `bwdif_vulkan` | 6.1 | BWDIF 去隔行 |
| `nlmeans_vulkan` | 6.1 | 非局部均值去噪 |
| `xfade_vulkan` | 6.1 | 交叉淡化 |
| `v360_vulkan` | next | 360° 视频处理 |
| `drawvg` | 8.1 | 矢量图形绘制（via libcairo） |

> 注：`yadif_videotoolbox` 是 VideoToolbox（非 Vulkan）滤镜，已从此表移除。

---

## 版本对照表

| 版本 | 代号 | 发布年份 | 重大变化 |
|------|------|----------|----------|
| **next** | - | ~2026H2 | swscale 重写, VC-2 Vulkan 编码, APV Vulkan hwaccel |
| 8.1 | Hoare | 2026Q1 | Vulkan ProRes 编码/DPX, D3D12 AV1/H264, Rockchip |
| 8.0 | Huffman | 2025Q3 | Vulkan Compute Codec, VVC 改进 |
| 7.1 | Péter | 2024Q4 | VVC 稳定, xHE-AAC, Vulkan H.264/HEVC 编码 |
| 7.0 | Dijkstra | 2024Q2 | VVC 实验性, IAMF, CLI 多线程, EVC |
| 6.1 | Heaviside | 2023Q4 | Vulkan 解码, FFT 重写 |
| 6.0 | Von Neumann | 2023Q1 | MediaCodec, 发布策略变更 |
| 5.1 | Riemann | 2022Q3 | IPFS, DFPWM |
| 5.0 | Lorentz | 2022Q1 | API 重构, libplacebo |
| 4.4 | Rao | 2021Q2 | AV1 SVT, VDPAU 10bit |
| 4.3 | 4:3 | 2020Q2 | Vulkan 首个支持, librav1e AV1 |
| 4.2 | Ada | 2019Q3 | dav1d AV1 |
| 4.1 | al-Khwarizmi | 2018Q4 | AVS2, libtensorflow |
| 4.0 | Wu | 2018Q2 | AV1, 移除 ffserver, SRT |
| 3.4 | Cantor | 2017Q4 | libvmaf, CUDA 滤镜 |
| 3.3 | Hilbert | 2017Q2 | Opus 编码, 球形视频 |
| 3.1 | Laplace | 2016Q2 | VAAPI 编码, MediaCodec |
| 3.0 | Einstein | 2016Q1 | AAC 稳定, VP9 hwaccel |
| 2.8 | - | 2015 | QSV, VAAPI |

---

| 编解码器支持总览 | 版本 | 代号 | 备注 |
|-----------------|------|------|------|
| **VVC/H.266 解码** | 7.0 | Dijkstra | 实验性；7.1 稳定；8.0 支持 IBC/Palette/ACT |
| **VVC/H.266 编码** | 7.1 | Péter | via libvvenc |
| **VVC 封装** | 6.1 | Heaviside | Raw bitstream parser, muxer, demuxer |
| **VVC Matroska** | 8.0 | Huffman | VVC in Matroska |
| **VVC QSV 解码** | 7.1 | Péter | Intel QSV 硬件加速 |
| **VVC VAAPI 解码** | 8.0 | Huffman | Linux VAAPI 硬件加速 |
| **Vulkan 解码** | 6.1 | Heaviside | H264/HEVC/AV1；8.0 新增 VP9 |
| **Vulkan H.264 编码** | 7.1 | Péter | Vulkan 编码首个支持 |
| **Vulkan HEVC 编码** | 7.1 | Péter | 与 VAAPI 功能对等 |
| **Vulkan AV1 编码** | 8.0 | Huffman | |
| **Vulkan Compute Codec** | 8.0 | Huffman | FFv1、ProRes RAW；8.1 ProRes 编码 |
| **AV1** | 4.0 | Wu | libaom；4.2 dav1d |
| **HEVC/H.265** | 2.1 | - | 原生解码器 2.1；8.0 Vulkan 解码 |
| **VP9** | 2.0 | - | 原生解码器 2.1；8.0 Vulkan 解码 |
| **AAC 编码** | 3.0 | Einstein | 原生编码器稳定化 |
| **Opus 编码** | 3.3 | Hilbert | 原生支持 |
| **DoVi** | 5.0 | Lorentz | 复用/解码/tonemapping |
| **IAMF** | 7.0 | Dijkstra | 沉浸式音频 |

---

## 硬件加速支持

| API | 平台 | 支持版本 | 支持的 Codec |
|-----|------|---------|------------|
| VAAPI | Linux | 0.9 | H264/HEVC/VP9 → **VVC 解码** (8.0) |
| VDPAU | Linux | 0.9 | H264/HEVC/VP9 |
| NVDEC/CUVID | NVIDIA | 2.6 | H264/HEVC/MJPEG/MPEG-1/2/4/VC1/VP8/9/AV1 |
| NVENC | NVIDIA | 2.6 | H264/HEVC；**AV1** (6.0) |
| VideoToolbox | macOS/iOS | 2.6 | H264/HEVC/VP9/ProRes |
| QSV | Intel | 2.0 | MPEG-2/HEVC/VP8 → **VVC** (7.1) |
| MediaCodec | Android | 3.1 | H264 → **AAC/AMR-NB/AMR-WB/MP3** (7.1) |
| D3D11VA | Windows | 2.7 | H264/HEVC/VP9/AV1 |
| **Vulkan** | 跨平台 | 4.3 | 滤镜；6.1 解码 H264/HEVC/AV1；8.0 VP9；7.1 编码 H264/HEVC；8.0 AV1 |
| **D3D12VA** | Windows | 7.0 | H264/HEVC/VP9/AV1/MPEG-2/VC1 |

### Vulkan 版本演进

| 版本 | Vulkan 新增 |
|------|------------|
| 4.3 | 首个支持：4 个滤镜 + AMD AMF（Linux） |
| 5.0 | libplacebo 集成、flip 滤镜 |
| 6.1 | **解码 hwaccel**：H264/HEVC/AV1；4 个新滤镜 |
| 7.1 | **H264/HEVC 编码器**（功能对等 VAAPI） |
| 8.0 | **Compute Codec**：FFv1 编解码、ProRes RAW 解码；VP9 解码；AV1 编码 |
| 8.1 | ProRes 编码；DPX 解码；swscale Vulkan；初始化优化（移除 GLSL） |

### VVC 版本演进

| 版本 | VVC 新增 |
|------|---------|
| 6.1 | Raw bitstream parser/muxer/demuxer；bsf 元数据编辑；bsf MP4→Annex B |
| 7.0 | **解码器（实验性）** |
| 7.1 | **稳定化解码器**；QSV 解码；**编码（libvvenc）**；DVB compat |
| 8.0 | VAAPI 解码；Matroska 封装；SCC 支持（IBC/Palette/ACT） |

---

## 未来版本规划（`version <next>`）

> 以下为当前 master 分支正在开发、将在下一版本发布的功能，源自 FFmpeg 官方 changelog。

### 开发中的功能

| 功能 | 说明 |
|------|------|
| **AMF HDR 能力扩展** | `vf_vpp_amf` 滤镜支持更多 HDR 功能 |
| **LCEVC MP4 封装支持** | LCEVC track muxing in MP4 muxer |
| **Playdate 视频编码器/复用器** | Playdate 游戏机视频格式 |
| **v360_vulkan 滤镜** | 360° 视频处理（Vulkan） |
| **HE-AAC 960 解码（DAB+）** | 数字音频广播配置 |
| **transpose_cuda 滤镜** | CUDA 加速转置滤镜 |
| **AMF 帧率转换器** | `vf_frc_amf` 滤镜 |
| **SMPTE 2094-50 元数据** | HDR 元数据支持 |
| **ProRes RAW VideoToolbox hwaccel** | Apple 硬件加速 |
| **APV Vulkan hwaccel** | APV 视频格式 Vulkan 加速 |
| **Animated WebP 解码器/解封装器** | 动态 WebP 支持 |
| **移除 CELT 解码支持** | 与 Opus CELT 无关 |
| **移除 ogg/celt 解析** | Ogg CELT 格式移除 |

### 即将推出的 Vulkan Compute Codec（来自 Khronos Blog）

根据 FFmpeg 8.0 发布博客，以下 Vulkan Compute Codec 正在开发中：

| Codec | 状态 |
|-------|------|
| **ProRes（编码+解码）** | **已在 8.1 释出**（编码） |
| **VC-2（编码+解码）** | 实现完成，正在 review，预计近期合并 |
| **FFv1（编码+解码）** | 已在 8.0 释出 |
| **ProRes RAW（仅解码）** | 已在 8.0 释出 |

> 原文："ProRes (encode+decode) and VC-2 (encode+decode) implementations are complete and currently in review, to be merged soon and available with the next minor release."
> 
> 注：ProRes Vulkan 编码已在 8.1 合并（见 Changelog "ProRes Vulkan encoder"），解码状态待确认。

### swscale 重写（进行中）

FFmpeg 8.1 发布博客提到："The groundwork for the upcoming swscale rewrite is progressing."

swscale 是 FFmpeg 的视频缩放/颜色空间转换核心库，重写预计带来性能提升。

---

## 发布节奏与生命周期

| 项目 | 说明 |
|------|------|
| **发布周期** | 约每 6 个月一个新主版本 |
| **主版本间** | 可能有 point releases（修复 bug，不加新功能） |
| **ABI 策略** | 主版本升级时 ABI 可能不兼容；废弃 API 在 3 个版本后移除 |
| **编译器要求** | 当前需 C11；即将升级到 C17 |
| **开发者建议** | FFmpeg 官方推荐使用 master 分支（开发版），而非 release 分支 |

### 当前稳定版本（截至 2026年5月，源自 ffmpeg.org/download.html）

| 版本 | 代号 | 首次发布 | 最新补丁版本 | 补丁发布日期 |
|------|------|----------|-------------|------------|
| **8.1** | Hoare | 2026-03-16 | 8.1.1 | 2026-05-04 |
| 8.0 | Huffman | 2025-08-22 | 8.0.2 | 2026-05-04 |
| 7.1 | Péter | 2024-09-30 | 7.1.4 | 2026-05-05 |
| 7.0 | Dijkstra | 2024-04-05 | 7.0.3 | 2025-08-05 |
| 6.1 | Heaviside | 2023-11-10 | 6.1.5 | 2026-05-05 |
| 5.1 | Riemann | 2022-07-22 | 5.1.9 | 2026-05-05 |
| 4.4 | Rao | 2021-04-08 | 4.4.7 | 2026-05-05 |
| 4.3 | 4:3 | 2020-06-15 | 4.3.9 | 2025-03-12 |
| 4.2 | Ada | 2019-08-05 | 4.2.11 | 2025-05-17 |
| 3.4 | Cantor | 2017-10-15 | 3.4.14 | 2025-03-12 |
| 2.8 | Feynman | 2015-09 | 2.8.22 | 2023-10-29 |

### 废弃/移除的功能（各版本）

| 版本 | 废弃/移除 |
|------|----------|
| 8.0 | OpenMAX 编码器废弃；yasm 移除（需用 nasm）；OpenSSL < 1.1.1 移除 |
| 7.0 | 移除废弃的 ffmpeg CLI 选项（`-psnr`、`-map_channel`）；C11 编译器要求 |
| 7.1 | DEC Alpha DSP 移除 |
| 6.0 | CrystalHD 解码器废弃 |
| 5.0 | libavresample 库移除；旧编码/解码 API 移除 |
| 4.0 | ffserver 移除；ffmenc/ffmdec 移除；VDA 移除（用 VideoToolbox 替代）；Windows XP 支持移除 |
| 3.1 | libdcadec 移除 |
| 3.0 | libvo-aacenc、libaacplus 移除 |
