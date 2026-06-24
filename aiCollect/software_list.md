# 开源软件清单

## 背景知识

**Windows 版本**：Windows 软件存放在 `/mnt/c/ProgramFilesMy/`（WSL 挂载的 Windows C 盘自定义目录），主要通过 `exiftool` 检查 exe 文件的 Product Version 确认本地版本，部分软件使用其他方式（见表格）。

**注意**：并非所有仓库都有 GitHub Releases 页面，有些项目（如 FFmpeg）仅使用 tags 标记版本，`gh release list` 会返回空。

**平台说明**：

| 缩写 | 含义 |
| ------ | ------ |
| Win | 仅 Windows 原生 |
| WSL-1 | 仅 WSL-1 环境 |
| Win+WSL-1(软链接到Win) | Windows 原生 + WSL-1 通过软链接调用 Windows 端 exe |
| macOS | 仅 macOS |
| Android | Android 设备（通过 adb 查询本地版本，仅关注 arm64-v8a 架构） |

**检查时间 📅**：

| 平台 | 最近检查 |
| ------ | --------- |
| Win | 2026-06-10 |
| WSL | 2026-06-10 |
| macOS | 2026-06-23 |
| Android | 2026-06-23 |

**表格列说明**：

| 取值 | 含义 |
| ------ | ------ |
| ✅ | 已是最新正式版 |
| ❌ | 有更新可用 |
| 🔄 | daily/rolling build |
| ⏸️ | 暂不关注/跳过某个版本 |
| - | 不适用 |


使用 GitHub CLI (`gh`) 查看仓库的 release 发布情况，需先 `gh auth login` 认证。

```bash
# 列出仓库最近的 release
gh release list -R <owner/repo> --limit 10

# 查看指定 tag 的 release 详情（发布说明、assets 等）
gh release view <tag> -R <owner/repo>

# 下载指定 release 的 assets
gh release download <tag> -R <owner/repo>

# 部分仓库不使用 GitHub Releases，改用 tags 查看版本
gh api repos/<owner/repo>/tags -q '.[].name'
```


## 软件清单

| 本地文件/文件夹 | 平台 | 参考 URL | 本地版本 | 查本地版本方法 | 最新版本 | 是否最新 |
| ---------------- | ------ | -------- | --------- | -------------- | -------- | ------ |
| **Windows** | | | | | | |
| ffmpeg | Win+WSL-1(ffplay软链接到Win⁵) | <https://github.com/BtbN/FFmpeg-Builds> | n8.1.1 | `ffmpeg -version` / `exiftool ffmpeg.exe` | daily build (2026-06-11)³ | 🔄 |
| RenderDoc_1.44_64 | Win | <https://github.com/baldurk/renderdoc> | v1.44 | `exiftool renderdocui.exe \| grep "Product Version"` | v1.44 | ✅ |
| scrcpy | Win+WSL-1(软链接到Win) | <https://github.com/Genymobile/scrcpy> | 4.0 | `scrcpy --version` | v4.0 | ✅ |
| jadx-gui-1.5.5 | Win+WSL-1(软链接到Win) | <https://github.com/skylot/jadx> | 1.5.5 | 文件夹名推断 | v1.5.5 | ✅ |
| draw.io | Win | <https://github.com/jgraph/drawio-desktop> | 30.0.4 | `exiftool draw.io.exe \| grep "Product Version"` | v30.0.4 | ✅ |
| mpv-x86_64-v3 | Win+WSL-1(软链接到Win) | <https://github.com/zhongfly/mpv-winbuild> | v0.41.0 | `mpv --version` | daily build (2026-06-11)³ | 🔄 |
| YUView-Win | Win | <https://github.com/IENT/YUView> | v2.14-322 | `strings YUView.exe \| grep` | v2.14 | ✅ |
| MediaInfo_GUI | Win | <https://github.com/MediaArea/MediaInfo> | 26.01.0 | `exiftool MediaInfo.exe \| grep "Product Version"` | v26.05 | ❌ |
| ImageGlass_x64 | Win | <https://github.com/d2phap/ImageGlass> | 9.4.1.15 | `exiftool ImageGlass.exe \| grep "Product Version"` | 9.5.0.515 | ❌ |
| WinDirStat.exe | Win | <https://github.com/windirstat/windirstat> | 2.6.2 | `exiftool WinDirStat.exe \| grep "Product Version"` | v2.6.2 | ✅ |
| ContextMenuManager.NET.4.0.exe | Win | <https://github.com/BluePointLilac/ContextMenuManager> | 3.3.3.1 | `exiftool ContextMenuManager.NET.4.0.exe \| grep "Product Version"` | 3.3.3.1 | ✅ |
| Snipaste-2.10.6-x64 | Win | <https://www.snipaste.com/download.html> | 2.11.2⁴ | `exiftool Snipaste.exe \| grep "Product Version"` | 无 release | - |
| WSL | Win | <https://github.com/microsoft/WSL> | 2.7.8 | `cmd.exe /c wsl --version` | 2.7.8 | ✅ |
| git | Win | <https://github.com/git-for-windows/git> | 2.54.0 | `cmd.exe /c git --version` | v2.54.0.windows.1 | ✅ |
| Android Studio | Win | <https://developer.android.google.cn/studio/releases> | 2025.3.4 (AI-253.32098.37.2534.15336583) | `grep '"version"' "/mnt/c/Program Files/Android/Android Studio/product-info.json"` | 2026.1.1 Patch 2 (2026-04-28) | ❌ |
| window-switcher | Win | <https://github.com/sigoden/window-switcher> | 1.18.0 | `exiftool window-switcher.exe \| grep "Product Version"` | v1.18.0 (2026-06-03) | ✅ |
| Notepad++ | Win | <https://github.com/notepad-plus-plus/notepad-plus-plus> | 8.9.6.4 | `exiftool notepad++.exe \| grep "Product Version"` | v8.9.6.4 (2026-06-03) | ✅ |
| Dism++x64 | Win | <https://github.com/Chuyu-Team/Dism-Multi-language> | 10.1.1002.1 | `exiftool Dism++x64.exe \| grep "Product Version"` | v10.1.1002.2 (2023-03-26) | ❌ |
| ProcessExplorer | Win | <https://learn.microsoft.com/en-us/sysinternals/downloads/process-explorer> | 17.12 | `exiftool procexp64.exe \| grep "Product Version"` | v17.12 (2026-05-07) | ✅ |
| **WSL** | | | | | | |
| git | WSL-1 | <https://github.com/git/git> | 2.54.0 | `git --version` | v2.54.0 | ✅ |
| yt-dlp | WSL-1 | <https://github.com/yt-dlp/yt-dlp> | 2026.06.09 | `yt-dlp --version` | 2026.06.09 | ✅ |
| glslang | WSL-1 | <https://github.com/KhronosGroup/glslang> | 16.3.0 | `glslang --version` | 16.3.0 | ✅ |
| bloaty | WSL-1 | <https://github.com/google/bloaty> | 1.1 | `bloaty --version` | v1.1 | ✅ |
| tldr | WSL-1 | <https://github.com/tldr-pages/tlrc> | v1.11.1 | `tldr --version` | v1.13.1 | ❌ |
| axel-2.17.14 | WSL-1 | <https://github.com/axel-download-accelerator/axel> | 2.17.14 | `axel --version` | v2.17.14 | ✅ |
| exiftool | WSL-1 | <https://github.com/exiftool/exiftool> | 13.59 | `exiftool -ver` | 13.59 | ✅ |
| fastfetch | WSL-1 | <https://github.com/fastfetch-cli/fastfetch> | 2.64.2 | `fastfetch --version` | 2.64.2 | ✅ |
| **macOS** | | | | | | |
| draw.io | macOS | <https://github.com/jgraph/drawio-desktop> | 30.2.4 | `defaults read /Applications/draw.io.app/Contents/Info.plist CFBundleShortVersionString` | v30.2.4 | ✅ |
| IINA | macOS | <https://github.com/iina/iina> | 1.4.3 | `defaults read /Applications/IINA.app/Contents/Info.plist CFBundleShortVersionString` | v1.4.3 | ✅ |
| Stats | macOS | <https://github.com/exelban/stats> | 3.0.4 | `defaults read /Applications/Stats.app/Contents/Info.plist CFBundleShortVersionString` | v3.0.4 | ✅ |
| KeyCastr | macOS | <https://github.com/keycastr/keycastr> | 0.10.5 | `defaults read /Applications/KeyCastr.app/Contents/Info.plist CFBundleShortVersionString` | v0.10.5 | ✅ |
| macos | macOS | <https://support.apple.com/zh-cn/109033> | 15.7.7 | `sw_vers` | 15.7.7 (Sequoia)¹ | ✅ |
| Android Studio | macOS | <https://developer.android.google.cn/studio/releases> | 2025.3 (AI-253.32098.37.2534.15232325) | `python3 -c "import json;print(json.load(open('/Applications/Android Studio.app/Contents/Resources/product-info.json'))['version'])"` | 2026.1.1 Patch 2 (2026-04-28) | ❌ |
| CC Switch | macOS | <https://github.com/farion1231/cc-switch> | 3.16.3 | `defaults read "/Applications/CC Switch.app/Contents/Info.plist" CFBundleShortVersionString` | v3.16.3 (2026-06-14) | ✅ |
| AltTab | macOS | <https://github.com/lwouis/alt-tab-macos> | 11.3.1 | `defaults read /Applications/AltTab.app/Contents/Info.plist CFBundleShortVersionString` | v11.3.1 | ✅ |
| Clash Verge | macOS | <https://github.com/clash-verge-rev/clash-verge-rev> | 2.5.1 | `defaults read /Applications/Clash\ Verge.app/Contents/Info.plist CFBundleShortVersionString` | v2.5.1 | ✅ |
| iTerm2 | macOS | <https://github.com/gnachman/iTerm2> | 3.6.11 | `defaults read /Applications/iTerm.app/Contents/Info.plist CFBundleShortVersionString` | 3.6.11² | ✅ |
| Notepad-- | macOS | <https://github.com/cxasm/notepad--> | 3.7.3 | `defaults read /Applications/Notepad--.app/Contents/Info.plist CFBundleShortVersionString` | v3.7.3 | ✅ |
| Ollama | macOS | <https://github.com/ollama/ollama> | 0.30.10 | `defaults read /Applications/Ollama.app/Contents/Info.plist CFBundleShortVersionString` | v0.30.10 | ✅ |
| **Android** | | | | | | |
| Termux | Android | <https://github.com/termux/termux-app> | 0.118.3 | `adb shell dumpsys package com.termux \| grep versionName` | v0.118.3 (2025-05-22) | ✅ |
| LibChecker | Android | <https://github.com/LibChecker/LibChecker> | 2.5.4.dev.4049e7c | `adb shell dumpsys package com.absinthe.libchecker \| grep versionName` | 2.5.4 (2026-06-17) | ✅ |
| Clash Meta | Android | <https://github.com/MetaCubeX/ClashMetaForAndroid> | 2.11.30.Meta | `adb shell dumpsys package com.github.metacubex.clash.meta \| grep versionName` | v2.11.30 (2026-06-06) | ✅ |
| NewPipe | Android | <https://github.com/TeamNewPipe/NewPipe> | 0.28.8 | `adb shell dumpsys package org.schabi.newpipe \| grep versionName` | v0.28.8 (2026-06-09) | ✅ |

> ¹ macOS 当前关注 Sequoia (15.x) 分支，暂不关注 Tahoe (26) 版本 ⏸️，后续大版本（如 27）仍会关注。 <br/>
> ² iTerm2 无 GitHub Releases，最新版本来自 [官网](https://iterm2.com/downloads.html)。 <br/>
> ³ FFmpeg (BtbN) 和 mpv (zhongfly/mpv-winbuild) 无正式 Release tag，`daily build` 为每日构建包，🔄 表示 rolling/daily build 项目，无固定版本对齐。 <br/>
> ⁴ Snipaste 无开源源码仓库，参考 URL 为官网下载页；exe 有可能已被原地替换升级，文件夹名仍为旧版本。 <br/>
> ⁵ WSL-1 无法启动图形窗口，因此 ffplay 通过软链接调用 Windows 端的 exe 运行。 <br/>
> ⁶ Android Studio 无开源仓库 / GitHub Releases，版本信息来自 [官网 releases 页](https://developer.android.com/studio/releases)；本地安装路径为 `/mnt/c/Program Files/Android/`（非 `ProgramFilesMy/`），版本号通过 `product-info.json` 的 `version` 字段获取（如 `AI-253.x.x.2534.x` → `2025.3.4`）。 <br/>
> ⁷ exiftool 仓库不使用 GitHub Releases，仅用 tags 标记版本，需用 `gh api repos/exiftool/exiftool/tags -q '.[].name'` 查看（与 FFmpeg 同类）。本地从源码构建安装：`perl Makefile.PL && make -j && sudo make install`。 <br/>
> ⁸ Android 应用本地版本通过 `adb shell dumpsys package <包名> | grep versionName` 查询；LibChecker 本地为开发构建快照（`2.5.4.dev.4049e7c`），与正式 release `2.5.4` 同版本号，标记为 ✅。 <br/>
> ⁹ Clash Meta（原 Clash for Android）Android 客户端仓库为 `MetaCubeX/ClashMetaForAndroid`，与核心项目 `MetaCubeX/mihomo` 版本号独立；本地 versionName 带 `.Meta` 后缀。 <br/>
> ¹⁰ Dism++ 仓库 `Chuyu-Team/Dism-Multi-language` 自 2023-03-26 后无新 release，项目处于维护停滞状态，最新版 v10.1.1002.2。 <br/>
> ¹¹ Process Explorer 为微软 Sysinternals 闭源免费软件，无开源仓库 / GitHub Releases，版本信息与下载来自 [Sysinternals 官网](https://learn.microsoft.com/en-us/sysinternals/downloads/process-explorer)（与 Snipaste / iTerm2 / Android Studio 同类为闭源例外）。 <br/>

共 43 个条目，37 个 GitHub 仓库。

