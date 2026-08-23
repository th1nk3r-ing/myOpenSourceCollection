# 软件清单

记录本机 Win / WSL-1 / macOS / Android 四大平台已安装软件的版本与更新状态，便于定期检查与同步。

## 1. 背景知识

### 1.1 平台说明

| 缩写 | 含义 | 最近检查 |
| ------ | ------ | --------- |
| Win | 仅 Windows 原生 | 2026-08-20 |
| Win -> WSL-1 | Windows 软链接到 WSL-1 | 2026-08-20 |
| WSL-1 | 仅 WSL-1 环境（仅关注 x64 架构） | 2026-08-20 |
| macOS | 仅 macOS（仅关注 arm64 架构） | 2026-08-23 |
| macOS Homebrew | macOS Homebrew 统一管理（`brew leaves`） | 2026-07-06 |
| Android | Android 设备（通过 adb 查询本地版本，仅关注 arm64-v8a 架构） | 2026-08-20 |

**命令执行环境**：

- Win 与 WSL-1 条目的命令在 **WSL-1** 环境下执行，Windows 路径通过 `/mnt/c/` 访问，需调用 Windows 原生命令时使用 `cmd.exe /c <命令>` 前缀
- macOS 条目命令在 macOS 终端执行
- Android 条目命令通过 `adb` 连接设备执行

### 1.2 Windows 软件位置与查询

**软件位置**：Windows 软件存放在 `/mnt/c/ProgramFilesMy/`（WSL 挂载的 Windows C 盘自定义目录）；少数软件安装在系统目录（如 Android Studio、Source Insight，见脚注）。

**版本查询方法**：主要通过 `exiftool` 检查 exe 文件的 Product Version 确认本地版本，部分软件使用其他方式（见表格"查本地版本方法"列）。

### 1.3 GitHub Releases 查询

使用 GitHub CLI (`gh`) 查看仓库的 release 发布情况，需先 `gh auth login` 认证。

```bash
# 列出仓库最近的 release
gh release list -R <owner/repo> --limit 10

# 查看指定 tag 的 release 详情（发布说明、assets 等）
gh release view <tag> -R <owner/repo>

# 下载指定 release 的 assets
gh release download <tag> -R <owner/repo>

# 部分仓库不使用 GitHub Releases（如 FFmpeg），改用 tags 查看版本
gh api repos/<owner/repo>/tags -q '.[].name'
```

**下载校验**：GitHub Releases API 为每个 asset 自动生成 SHA256（`digest` 字段，格式 `sha256:<hash>`），下载后应执行校验确保完整性。

```bash
# 通过 GitHub API 获取 asset 的 SHA256
gh api repos/<owner/repo>/releases/latest --jq '.assets[] | select(.name=="<asset-name>") | .digest'

# 本地哈希对照
sha256sum <下载的文件>   # 两处哈希一致即校验通过
```

### 1.4 表格图例

> - 下表中所有"最新版本"与"是否最新"状态均以 §1.1 各平台"最近检查"日期为准；
> - `✅` 仅表示在该检查日点为最新，不保证当下绝对最新。
> - `❌` 同理。

| 取值 | 含义 |
| ------ | ------ |
| ✅ | 已是最新正式版 |
| ❌ | 有更新可用 |
| 🔄 | daily/rolling build（性质标记，与 ✅/❌ 组合表示新鲜度：✅ 本地构建与最新构建对齐，❌ 落后） |
| 🔨 | 源码编译（无预编译发行版，需本地构建） |
| ⏸️ | 暂不关注/跳过某个版本 |
| - | 不适用 |

## 2. GitHub Release 加速下载

- GitHub Release 文件（`.dmg` / `.exe` / `.tar.gz` 等）在国内直连常被干扰阻断。通过**反代代理**中转可大幅提速。
- 加速源格式 : `https://<加速源域名>/https://github.com/<owner>/<repo>/releases/download/<tag>/<file>`

### 实测最快加速源

排名综合两次实测：① 2026-06-30 macOS 29.5 MB DMG；② 2026-06-30 WSL-1 yt-dlp 15 MB 二进制。各次速度列仅展示 macOS 实测值。

| 排名 | 加速源 | 实测速度 | 地区 |
|------|--------|---------|------|
| 🥇 | `github.boki.moe` | ~9.8 MB/s | 美国 |
| 🥈 | `cors.isteed.cc` | ~8.9 MB/s | 美国 |
| 🥉 | `ghproxy.net` | ~5.0 MB/s | 英国 |
| 4 | `ghfast.top` | ~2.8 MB/s | 韩国 首尔 |
| 5 | `wget.la` | ~450 KB/s | 中国 香港 |

> 以上为 Release / Code(ZIP) 类文件加速源，Git Clone / Raw 另有独立加速源，此处不展开。

### 使用示例

```bash
# 通用格式
curl -# -o <file> "https://<加速源>/https://github.com/<owner>/<repo>/releases/download/<tag>/<file>"

# 示例：通过 ghproxy.net 下载 CC Switch v3.16.4 DMG
curl -# -o /tmp/CC-Switch-v3.16.4-macOS.dmg \
  "https://ghproxy.net/https://github.com/farion1231/cc-switch/releases/download/v3.16.4/CC-Switch-v3.16.4-macOS.dmg"

# 校验 SHA256
gh api repos/farion1231/cc-switch/releases/tags/v3.16.4 \
  --jq '.assets[] | select(.name=="CC-Switch-v3.16.4-macOS.dmg") | .digest'
sha256sum /tmp/CC-Switch-v3.16.4-macOS.dmg
```

### 参考链接

- [XIU2/UserScript](https://github.com/XIU2/UserScript) — GitHub 增强高速下载油猴脚本，自动注入加速按钮
- [hunshcn/gh-proxy](https://github.com/hunshcn/gh-proxy) — 开源反代加速方案
- [WJQSERVER-STUDIO/ghproxy](https://github.com/WJQSERVER-STUDIO/ghproxy) — 另一反代加速实现
- 加速源完整列表：[XIU2 Wiki](https://github.com/XIU2/UserScript/wiki/Github-%E5%A2%9E%E5%BC%BA-%E2%80%90-%E9%AB%98%E9%80%9F%E4%B8%8B%E8%BD%BD)


## 3. Win

### 原生软件

| 软件 / 条目 | 平台 | 参考 URL | 本地版本 | 查本地版本方法 | 最新版本 | 是否最新 |
| ---------------- | ------ | -------- | --------- | -------------- | -------- | ------ |
| ffmpeg | Win¹ | <https://github.com/BtbN/FFmpeg-Builds> | n9.0.1-6-g9d4ca21220-20260819 (2026-08-19) | `ffmpeg.exe -version`（以命令输出为准，不用文件夹名/exiftool） | daily build 2026-08-19² | 🔄 ✅ |
| RenderDoc_1.45_64 | Win | <https://github.com/baldurk/renderdoc> | v1.45 | `exiftool renderdocui.exe \| grep "Product Version"` | v1.45 (2026-07-02) | ✅ |
| scrcpy | Win 安装, WSL-1 链接 | <https://github.com/Genymobile/scrcpy> | 4.1 | `scrcpy --version` | v4.1 (2026-07-12) | ✅ |
| jadx-gui-1.5.6 | Win 安装, WSL-1 链接 | <https://github.com/skylot/jadx> | 1.5.6 | `exiftool jadx-gui.exe \| grep "Product Version"` | v1.5.6 (2026-07-10) | ✅ |
| draw.io¹⁵ | Win | <https://github.com/jgraph/drawio-desktop> | 31.1.8 | `exiftool draw.io.exe \| grep "Product Version"` | v31.1.8 (2026-08-07) | ✅ |
| mpv-x86_64-v3 | Win 安装, WSL-1 链接 | <https://github.com/zhongfly/mpv-winbuild> | v0.41.0-908-g48e6c35c0 (2026-07-26) | `mpv --version`（以命令输出为准，不用文件夹名） | daily build 2026-08-19² | 🔄 ❌ |
| YUView-Win | Win | <https://github.com/IENT/YUView> | v2.14-359 | `grep "Last Commit" YUView-Win/versioninfo.txt` | v2.14 | ✅ |
| MediaInfo_GUI | Win | <https://github.com/MediaArea/MediaInfo> | 26.01.0 | `exiftool MediaInfo.exe \| grep "Product Version"` | v26.05 (2026-05-12) | ❌ |
| ImageGlass_x64 | Win | <https://github.com/d2phap/ImageGlass> | 9.5.0.515 | `exiftool ImageGlass.exe \| grep "Product Version"` | 9.6.1.807 (2026-08-06) | ❌ |
| WinDirStat.exe | Win | <https://github.com/windirstat/windirstat> | 2.8.0 | `exiftool WinDirStat.exe \| grep "Product Version"` | v2.8.0 (2026-08-02) | ✅ |
| ContextMenuManager.NET.4.0.exe | Win | <https://github.com/BluePointLilac/ContextMenuManager> | 3.3.3.1 | `exiftool ContextMenuManager.NET.4.0.exe \| grep "Product Version"` | 3.3.3.1 | ✅ |
| Snipaste | Win | <https://www.snipaste.com/download.html> | 2.11.2³ | `exiftool Snipaste.exe \| grep "Product Version"` | 2.11.3 (2026-01-18)³ | ❌ |
| WSL | Win | <https://github.com/microsoft/WSL> | 2.7.12 | `cmd.exe /c wsl --version` | 2.7.12 (2026-08-18) | ✅ |
| git | Win | <https://github.com/git-for-windows/git> | 2.55.0.windows.3 | `cmd.exe /c git --version` | v2.55.0.windows.4 (2026-08-11) | ❌ |
| Android Studio⁴ | Win | <https://developer.android.google.cn/studio/releases> | 2025.3.4 (AI-253.32098.37.2534.15336583) | `grep '"version"' "/mnt/c/Program Files/Android/Android Studio/product-info.json"` | 2026.1.3 Patch 1 (2026-08) | ❌ |
| window-switcher | Win | <https://github.com/sigoden/window-switcher> | 1.18.0 | `exiftool window-switcher.exe \| grep "Product Version"` | v1.19.0 (2026-07-24) | ❌ |
| Notepad++ | Win | <https://github.com/notepad-plus-plus/notepad-plus-plus> | 8.9.7 | `exiftool notepad++.exe \| grep "Product Version"` | v8.9.7 (2026-07-14) | ✅ |
| Dism++x64 | Win | <https://github.com/Chuyu-Team/Dism-Multi-language> | 10.1.1002.1 | `exiftool Dism++x64.exe \| grep "Product Version"` | v10.1.1002.2 (2023-03-26)⁵ | ❌ |
| ProcessExplorer⁶ | Win | <https://learn.microsoft.com/en-us/sysinternals/downloads/process-explorer> | 17.12 | `exiftool procexp64.exe \| grep "Product Version"` | v17.13 (2026-08-12) | ❌ |
| Source Insight 4.0⁷ | Win | <https://www.sourceinsight.com/download/> | 4.0.0150 | `exiftool sourceinsight4.exe \| grep "Product Version"` | 4.0.0150 (2026-02-06) | ✅ |

## 4. WSL-1

### 原生软件

| 软件 / 条目 | 平台 | 参考 URL | 本地版本 | 查本地版本方法 | 最新版本 | 是否最新 |
| ---------------- | ------ | -------- | --------- | -------------- | -------- | ------ |
| scrcpy | WSL-1（Win 安装） | <https://github.com/Genymobile/scrcpy> | 4.1 | `scrcpy --version` | v4.1 (2026-07-12) | ✅ |
| jadx-gui-1.5.6 | WSL-1（Win 安装） | <https://github.com/skylot/jadx> | 1.5.6 | `exiftool jadx-gui.exe \| grep "Product Version"` | v1.5.6 (2026-07-10) | ✅ |
| mpv-x86_64-v3 | WSL-1（Win 安装） | <https://github.com/zhongfly/mpv-winbuild> | v0.41.0-908-g48e6c35c0 (2026-07-26) | `mpv --version`（以命令输出为准，不用文件夹名） | daily build 2026-08-19² | 🔄 ❌ |
| ffmpeg | WSL-1¹ | <https://github.com/BtbN/FFmpeg-Builds> | n9.0.1-6-g9d4ca21220-20260819 (2026-08-19) | `ffmpeg -version`（以命令输出为准，不用文件夹名） | daily build 2026-08-19² | 🔄 ✅ |
| git | WSL-1 | <https://github.com/git/git> | 2.54.0 | `git --version` | v2.55.0 | ❌ |
| yt-dlp | WSL-1 | <https://github.com/yt-dlp/yt-dlp> | 2026.08.19 | `yt-dlp --version` | 2026.08.19 | ✅ |
| glslang | WSL-1 | <https://github.com/KhronosGroup/glslang> | 16.3.0 | `glslang --version` | 16.5.0 (2026-08-03) | ❌ |
| bloaty | WSL-1 | <https://github.com/google/bloaty> | 1.1 | `bloaty --version` | v1.1 | ✅ |
| tldr | WSL-1 | <https://github.com/tldr-pages/tlrc> | v1.13.1 | `tldr --version` | v1.13.1 (2026-05-22) | ✅ |
| axel-2.17.14 | WSL-1 | <https://github.com/axel-download-accelerator/axel> | 2.17.14 | `axel --version` | v2.17.14 | ✅ |
| exiftool¹⁰ | WSL-1 | <https://github.com/exiftool/exiftool> | 13.59 | `exiftool -ver` | 13.59 | 🔨 ✅ |
| fastfetch¹¹ | WSL-1 | <https://github.com/fastfetch-cli/fastfetch> | 2.65.1 | `fastfetch --version` | 2.67.1 (2026-08-14) | 🔨 ❌ |
| gh | WSL-1 | <https://github.com/cli/cli> | 2.97.0 | `gh --version` | v2.97.0 (2026-07-31) | ✅ |
| checksec | WSL-1 | <https://github.com/slimm609/checksec> | 3.2.0 | `checksec --version` | 3.2.0 (2026-06-17) | ✅ |

### apt

WSL-1 顶层手动安装的 apt 包（`apt-mark showmanual | sort`），用于重装/迁移参考。最近刷新：2026-07-06。

```log
apparmor apport aria2 base-files base-passwd bash bash-completion bear
bind9-dnsutils binutils bsdutils btop build-essential busybox byobu
ca-certificates clang clang-format cloc cloud-init cmake command-not-found
cpio cppcheck cron curl dash dbus-x11 diffutils dirmngr dmidecode dos2unix
dosfstools ed fastfetch ffmpeg figlet file findutils fluidsynth fonts-noto-cjk
fonts-ubuntu friendly-recovery ftp g++ gh git git-doc git-man gnupg grep gzip
hdparm hostname htop info init iptables iputils-tracepath irqbalance
landscape-client language-pack-zh-hant libcurl3-gnutls libcurl4
libdebconfclient0 libegl1 libfmt-dev libgl1 libgrpc++-dev libgtk-3-0
libimage-exiftool-perl libjpeg-dev libnl-3-200 libnl-genl-3-200 libnss3
libnss3-dev libpam-systemd libspdlog-dev libssl-dev login logrotate lshw lsof
man-db manpages media-types mediainfo motd-news-config mtr-tiny nano nasm
ncurses-base ncurses-bin net-tools nftables ninja-build nlohmann-json3-dev npm
ntfs-3g openssh-client p7zip-full parted patch patchelf pciutils pkg-config
plymouth plymouth-theme-ubuntu-text protobuf-compiler-grpc psmisc python3-pip
python3-venv ripgrep rsync screen screenfetch shellcheck show-motd
silversearcher-ag sl snapd software-properties-common strace sysbench
sysvinit-utils tcpdump telnet tig time tnftp trash-cli tree ubuntu-minimal ufw
ugrep unattended-upgrades universal-ctags unrar unzip update-manager-core
usbutils uuid-runtime vim wget wsl-setup xz-utils yasm zsh zstd
```

## 5. macOS

### 原生软件

| 软件 / 条目 | 平台 | 参考 URL | 本地版本 | 查本地版本方法 | 最新版本 | 是否最新 |
| ---------------- | ------ | -------- | --------- | -------------- | -------- | ------ |
| draw.io | macOS | <https://github.com/jgraph/drawio-desktop> | 31.3.2 | `defaults read /Applications/draw.io.app/Contents/Info.plist CFBundleShortVersionString` | v31.3.2 (2026-08-22) | ✅ |
| IINA | macOS | <https://github.com/iina/iina> | 1.4.4 | `defaults read /Applications/IINA.app/Contents/Info.plist CFBundleShortVersionString` | v1.4.4 | ✅ |
| Stats | macOS | <https://github.com/exelban/stats> | 3.0.11 | `defaults read /Applications/Stats.app/Contents/Info.plist CFBundleShortVersionString` | v3.0.11 | ✅ |
| KeyCastr | macOS | <https://github.com/keycastr/keycastr> | 0.10.5 | `defaults read /Applications/KeyCastr.app/Contents/Info.plist CFBundleShortVersionString` | v0.10.5 | ✅ |
| macOS (系统) | macOS | <https://support.apple.com/zh-cn/109033> | 15.7.9 | `sw_vers` | 15.7.9 (Sequoia)⁸ | ✅ |
| Android Studio⁴ | macOS | <https://developer.android.google.cn/studio/releases> | 2025.3 (AI-253.32098.37.2534.15232325) | `python3 -c "import json;print(json.load(open('/Applications/Android Studio.app/Contents/Resources/product-info.json'))['version'])"` | 2026.1.3 Patch 1 (Quail 3) | ❌ |
| CC Switch | macOS | <https://github.com/farion1231/cc-switch> | 3.20.0 | `defaults read "/Applications/CC Switch.app/Contents/Info.plist" CFBundleShortVersionString` | v3.20.0 (2026-08-18) | ✅ |
| AltTab | macOS | <https://github.com/lwouis/alt-tab-macos> | 11.5.0 | `defaults read /Applications/AltTab.app/Contents/Info.plist CFBundleShortVersionString` | v11.5.0 (2026-08-19) | ✅ |
| Clash Verge | macOS | <https://github.com/clash-verge-rev/clash-verge-rev> | 2.5.2 | `defaults read /Applications/Clash\ Verge.app/Contents/Info.plist CFBundleShortVersionString` | v2.5.2 | ✅ |
| iTerm2 | macOS | <https://github.com/gnachman/iTerm2> | 3.6.11 | `defaults read /Applications/iTerm.app/Contents/Info.plist CFBundleShortVersionString` | 3.6.11⁹ | ✅ |
| Notepad-- | macOS | <https://github.com/cxasm/notepad--> | 3.8.3 | `defaults read /Applications/Notepad--.app/Contents/Info.plist CFBundleShortVersionString` | v3.8.3 (2026-08-20) | ✅ |
| Ollama | macOS | <https://github.com/ollama/ollama> | 0.32.15 | `defaults read /Applications/Ollama.app/Contents/Info.plist CFBundleShortVersionString` | v0.32.15 (2026-08-19) | ✅ |
| gh | macOS | <https://github.com/cli/cli> | 2.98.0 | `gh --version` | v2.98.0 (2026-08-20) | ✅ |
| checksec | macOS | <https://github.com/slimm609/checksec> | 3.2.0 | `checksec --version` | 3.2.0 | ✅ |
| list_cpu_features¹⁴ | macOS | <https://github.com/google/cpu_features> | 0.11.0 | 源码 tag 推断（工具无 `--version` 输出） | 0.11.0 | 🔨 ✅ |
| VQ Analyzer¹⁶ | macOS | <https://cdn.vicuesoft.com/vqAnalyzer/docs/VQAnalyzerReleaseNotes.html> | 7.9.0 | 安装目录名 `VQAnalyzer_{ver}.app` 推断（Info.plist 为厂商内部编号，不采用） | 7.9.0 (2026) | ✅ |

### Homebrew

Homebrew 主动安装的顶层包（`brew leaves | sort`），用于重装/迁移参考。平台表中本行的"最近检查"指包列表刷新时间，非版本校验。

```log
automake bear binutils btop clang-format cloc cmake coreutils cppcheck ctags
demumble docker docker-squash exiftool fastfetch ffmpeg@7 file-formula gh
git-lfs gnu-sed htop lftp lld media-info meson mole node p7zip pcre pipx
pkgconf python@3.9 qt scrcpy shellcheck the_silver_searcher tlrc trash tree
wget xcodegen yt-dlp zsh-completions
```

`brew list --cask` : 暂无

## 6. Android

| 软件 / 条目 | 平台 | 参考 URL | 本地版本 | 查本地版本方法 | 最新版本 | 是否最新 |
| ---------------- | ------ | -------- | --------- | -------------- | -------- | ------ |
| Termux | Android | <https://github.com/termux/termux-app> | 0.118.3 | `adb shell dumpsys package com.termux \| grep versionName` | v0.118.3 (2025-05-22) | ✅ |
| LibChecker¹² | Android | <https://github.com/LibChecker/LibChecker> | 2.5.4.5696014 | `adb shell dumpsys package com.absinthe.libchecker \| grep versionName` | 2.5.4 (2026-06-17) | ✅ |
| Clash Meta¹³ | Android | <https://github.com/MetaCubeX/ClashMetaForAndroid> | 2.11.33.Meta | `adb shell dumpsys package com.github.metacubex.clash.meta \| grep versionName` | v2.11.33 (2026-08-16) | ✅ |
| NewPipe | Android | <https://github.com/TeamNewPipe/NewPipe> | 0.29.1 | `adb shell dumpsys package org.schabi.newpipe \| grep versionName` | v0.29.1 (2026-08-15) | ✅ |

> ¹ WSL-1 无法启动图形窗口，因此 ffplay 通过软链接调用 Windows 端的 exe 运行；WSL 本地的 ffmpeg/ffprobe 为 Linux 原生二进制（BtbN linux64-gpl 构建），与 Windows 端同源但独立。BtbN 提供 gpl / gpl-shared / lgpl / lgpl-shared 四类变体（× win64/linux64 平台），本地两端均采用 `gpl-shared`（GPL 动态链接，含 x264/x265/aom/dav1d 等最多第三方库，功能支持最全）；`lgpl` 系列不含 GPL 库，功能较少。 <br/>
> ² FFmpeg (BtbN) 和 mpv (zhongfly/mpv-winbuild) 无正式 Release tag，`daily build` 为每日构建包；两列均括注构建日期，落后天数可由两列日期直接对比得出。 <br/>
> ³ Snipaste 无开源源码仓库，参考 URL 为官网下载页；exe 有可能已被原地替换升级，文件夹名仍为旧版本。最新版本需手动访问官网确认，无法自动查询，"是否最新"经人工检查后标记为 ❌。 <br/>
> ⁴ Android Studio 无开源仓库 / GitHub Releases，版本信息来自 [官网 releases 页](https://developer.android.google.cn/studio/releases)；本地安装路径为 `/mnt/c/Program Files/Android/`（非 `ProgramFilesMy/`），版本号通过 `product-info.json` 的 `version` 字段获取（如 `AI-253.x.x.2534.x` → `2025.3.4`）。 <br/>
> ⁵ Dism++ 仓库 `Chuyu-Team/Dism-Multi-language` 自 2023-03-26 后无新 release，项目处于维护停滞状态，最新版 v10.1.1002.2。 <br/>
> ⁶ Process Explorer 为微软 Sysinternals 闭源免费软件，无开源仓库 / GitHub Releases，版本信息与下载来自 [Sysinternals 官网](https://learn.microsoft.com/en-us/sysinternals/downloads/process-explorer)（与 Snipaste / iTerm2 / Android Studio 同类为闭源例外）。 <br/>
> ⁷ Source Insight 为闭源商业软件，无开源仓库 / GitHub Releases，版本信息与下载来自 [官网下载页](https://www.sourceinsight.com/download/)（与 Snipaste / iTerm2 / Process Explorer / Android Studio 同类为闭源例外）；本地安装路径为 `/mnt/c/Program Files (x86)/Source Insight 4.0/`（非 `ProgramFilesMy/`），版本号通过 `exiftool sourceinsight4.exe` 的 `Product Version` 字段获取（如 `4.0.150.0` → `4.0.0150`）。 <br/>
> ⁸ macOS 当前关注 Sequoia (15.x) 分支，暂不关注 Tahoe (26) 版本 ⏸️，后续大版本（如 27）仍会关注。 <br/>
> ⁹ iTerm2 无 GitHub Releases，最新版本来自 [官网](https://iterm2.com/downloads.html)。 <br/>
> ¹⁰ exiftool 仓库不使用 GitHub Releases，仅用 tags 标记版本，需用 `gh api repos/exiftool/exiftool/tags -q '.[].name'` 查看（与 FFmpeg 同类）。本地从源码构建安装：`perl Makefile.PL && make -j && sudo make install`。 <br/>
> ¹¹ fastfetch 官方提供 Linux 预编译包，但在 WSL-1 上因 glibc 版本要求较高而无法直接运行，故源码编译以确保兼容；构建方式：`cmake -B build -DCMAKE_BUILD_TYPE=Release && cmake --build build -j && sudo cmake --install build`。 <br/>
> ¹² Android 应用本地版本通过 `adb shell dumpsys package <包名> | grep versionName` 查询；LibChecker 本地为开发构建快照（`2.5.4.5696014`），与正式 release `2.5.4` 同版本号，标记为 ✅。 <br/>
> ¹³ Clash Meta（原 Clash for Android）Android 客户端仓库为 `MetaCubeX/ClashMetaForAndroid`，与核心项目 `MetaCubeX/mihomo` 版本号独立；本地 versionName 带 `.Meta` 后缀。 <br/>
> ¹⁴ google/cpu_features 官方无预编译发行版，需源码编译；`list_cpu_features` 工具本身不输出自身版本号，版本由构建时所用源码 tag 推断。构建方式：`cmake -B build && cmake --build build -j && sudo cmake --install build`。 <br/>
> ¹⁵ drawio release 命名规则：Windows 包无 arch 中缀（`draw.io-{ver}-windows.zip` / `-installer.exe`），macOS/Linux 包带 arch 中缀（`draw.io-{x64|arm64|universal}-{ver}.dmg`、`drawio-{amd64|arm64|x86_64}-{ver}.deb/.AppImage`）。 <br/>
> ¹⁶ VQ Analyzer 为 VICUESOFT 闭源商业软件，无开源仓库 / GitHub Releases，最新版本以其 CDN 发布的 [ReleaseNotes](https://cdn.vicuesoft.com/vqAnalyzer/docs/VQAnalyzerReleaseNotes.html) 页为准（与 Snipaste / Source Insight 同类为闭源例外）；Info.plist 中 `CFBundleShortVersionString`（如 4.0.0）为厂商内部编号，与产品发布版本（7.9.0）不一致，本地版本以安装目录名后缀为准。 <br/>

共 54 个软件条目（不含 apt / Homebrew 包列表；含 3 条跨平台重复——draw.io、gh、checksec 在 Win/WSL-1 与 macOS 间重复；macOS 系统版本作为独立条目计入），40 个 GitHub 仓库（跨平台条目共享同一仓库时去重；Snipaste、Android Studio、macOS、Process Explorer、Source Insight、VQ Analyzer 等闭源/非 GitHub 来源不计入）。

