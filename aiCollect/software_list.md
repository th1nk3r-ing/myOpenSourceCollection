# 开源软件清单

## 背景知识

> **Windows 版本**：Windows 软件存放在 `/mnt/c/ProgramFilesMy/`（WSL 挂载的 Windows C 盘自定义目录），主要通过 `exiftool` 检查 exe 文件的 Product Version 确认本地版本，部分软件使用其他方式（见表格）。
>
> **注意**：并非所有仓库都有 GitHub Releases 页面，有些项目（如 FFmpeg）仅使用 tags 标记版本，`gh release list` 会返回空。

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
|----------------|------|--------|---------|--------------|--------|------|
| ffmpeg | Win+WSL-1(ffplay软链接到Win⁵) | <https://github.com/BtbN/FFmpeg-Builds> | n8.1.1 | `ffmpeg -version` / `exiftool ffmpeg.exe` | daily build (2026-06-11)³ | ❌ |
| RenderDoc_1.44_64 | Win | <https://github.com/baldurk/renderdoc> | v1.44 | `exiftool renderdocui.exe \| grep "Product Version"` | v1.44 | ✅ |
| scrcpy | Win+WSL-1(软链接到Win) | <https://github.com/Genymobile/scrcpy> | 4.0 | `scrcpy --version` | v4.0 | ✅ |
| jadx-gui-1.5.5 | Win+WSL-1(软链接到Win) | <https://github.com/skylot/jadx> | 1.5.5 | 文件夹名推断 | v1.5.5 | ✅ |
| draw.io | Win | <https://github.com/jgraph/drawio-desktop> | 30.0.4 | `exiftool draw.io.exe \| grep "Product Version"` | v30.0.4 | ✅ |
| draw.io | macOS | <https://github.com/jgraph/drawio-desktop> | 30.0.4 | `defaults read /Applications/draw.io.app/Contents/Info.plist CFBundleShortVersionString` | v30.0.4 | ✅ |
| IINA | macOS | <https://github.com/iina/iina> | 1.4.3 | `defaults read /Applications/IINA.app/Contents/Info.plist CFBundleShortVersionString` | v1.4.3 | ✅ |
| Stats | macOS | <https://github.com/exelban/stats> | 3.0.1 | `defaults read /Applications/Stats.app/Contents/Info.plist CFBundleShortVersionString` | v3.0.1 | ✅ |
| KeyCastr | macOS | <https://github.com/keycastr/keycastr> | 0.10.5 | `defaults read /Applications/KeyCastr.app/Contents/Info.plist CFBundleShortVersionString` | v0.10.5 | ✅ |
| macos | macOS | <https://support.apple.com/zh-cn/109033> | 15.7.7 | `sw_vers` | 15.7.7 (Sequoia)¹ | ✅ |
| AltTab | macOS | <https://github.com/lwouis/alt-tab-macos> | 11.3.0 | `defaults read /Applications/AltTab.app/Contents/Info.plist CFBundleShortVersionString` | v11.3.0 | ✅ |
| Clash Verge | macOS | <https://github.com/clash-verge-rev/clash-verge-rev> | 2.5.1 | `defaults read /Applications/Clash\ Verge.app/Contents/Info.plist CFBundleShortVersionString` | v2.5.1 | ✅ |
| iTerm2 | macOS | <https://github.com/gnachman/iTerm2> | 3.6.11 | `defaults read /Applications/iTerm.app/Contents/Info.plist CFBundleShortVersionString` | 3.6.11² | ✅ |
| Notepad-- | macOS | <https://github.com/cxasm/notepad--> | 3.7.3 | `defaults read /Applications/Notepad--.app/Contents/Info.plist CFBundleShortVersionString` | v3.7.3 | ✅ |
| Ollama | macOS | <https://github.com/ollama/ollama> | 0.30.7 | `defaults read /Applications/Ollama.app/Contents/Info.plist CFBundleShortVersionString` | v0.30.7 | ✅ |
| mpv-x86_64-v3 | Win+WSL-1(软链接到Win) | <https://github.com/zhongfly/mpv-winbuild> | v0.41.0 | `mpv --version` | daily build (2026-06-11)³ | ❌ |
| YUView-Win | Win | <https://github.com/IENT/YUView> | v2.14-322 | `strings YUView.exe \| grep` | v2.14 | ✅ |
| MediaInfo_GUI | Win | <https://github.com/MediaArea/MediaInfo> | 26.01.0 | `exiftool MediaInfo.exe \| grep "Product Version"` | v26.05 | ❌ |
| ImageGlass_x64 | Win | <https://github.com/d2phap/ImageGlass> | 9.4.1.15 | `exiftool ImageGlass.exe \| grep "Product Version"` | 9.5.0.515 | ❌ |
| WinDirStat.exe | Win | <https://github.com/windirstat/windirstat> | 2.6.2 | `exiftool WinDirStat.exe \| grep "Product Version"` | v2.6.2 | ✅ |
| ContextMenuManager.NET.4.0.exe | Win | <https://github.com/BluePointLilac/ContextMenuManager> | 3.3.3.1 | `exiftool ContextMenuManager.NET.4.0.exe \| grep "Product Version"` | 3.3.3.1 | ✅ |
| Snipaste-2.10.6-x64 | Win | <https://github.com/Snipaste/feedback> | 2.11.2⁴ | `exiftool Snipaste.exe \| grep "Product Version"` | 无 release | - |
| WSL | Win | <https://github.com/microsoft/WSL> | 2.7.8 | `cmd.exe /c wsl --version` | 2.7.8 | ✅ |
| git | Win | <https://github.com/git-for-windows/git> | 2.54.0 | `cmd.exe /c git --version` | v2.54.0.windows.1 | ✅ |
| git | WSL-1 | <https://github.com/git/git> | 2.54.0 | `git --version` | v2.54.0 | ✅ |
| yt-dlp | WSL-1 | <https://github.com/yt-dlp/yt-dlp> | 2026.06.09 | `yt-dlp --version` | 2026.06.09 | ✅ |
| glslang | WSL-1 | <https://github.com/KhronosGroup/glslang> | 16.3.0 | `glslang --version` | 16.3.0 | ✅ |
| bloaty | WSL-1 | <https://github.com/google/bloaty> | 1.1 | `bloaty --version` | v1.1 | ✅ |
| tldr | WSL-1 | <https://github.com/tldr-pages/tlrc> | v1.11.1 | `tldr --version` | v1.13.1 | ❌ |
| axel-2.17.14 | WSL-1 | <https://github.com/axel-download-accelerator/axel> | 2.17.14 | `axel --version` | v2.17.14 | ✅ |

> ¹ macOS 当前关注 Sequoia (15.x) 分支，暂不关注 Tahoe (26) 版本 ⏸️，后续大版本（如 27）仍会关注。 <br/>
> ² iTerm2 无 GitHub Releases，最新版本来自 [官网](https://iterm2.com/downloads.html)。 <br/>
> ³ FFmpeg (BtbN) 和 mpv (zhongfly/mpv-winbuild) 无正式 Release tag，`daily build` 为每日构建包。 <br/>
> ⁴ Snipaste exe 有可能已被原地替换升级，文件夹名仍为旧版本。 <br/>
> ⁵ WSL-1 无法启动图形窗口，因此 ffplay 通过软链接调用 Windows 端的 exe 运行。 <br/>

共 30 个条目，27 个 GitHub 仓库。

