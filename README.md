<div align="center">

<img src="assets/icon.png" width="112" alt="Exeify" />

# Exeify

**把网址或本地网页，一键打包成 Windows exe 和 安卓 APK**

零工具链 · 秒级出包 · 产物约 2 MB · exe 与 APK 双端输出 · 支持 Vite / Vue / React 等框架

<p>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-Apache--2.0-blue.svg" alt="License" /></a>
  <img src="https://img.shields.io/badge/packer-Windows%2010%2B-0078D6?logo=windows&logoColor=white" alt="Packer" />
  <img src="https://img.shields.io/badge/output-exe%20%2B%20apk-success" alt="Output" />
  <img src="https://img.shields.io/badge/built%20with-Rust-000000?logo=rust&logoColor=white" alt="Rust" />
  <a href="https://github.com/44886/Exeify/releases"><img src="https://img.shields.io/github/v/release/44886/Exeify?label=release&color=orange" alt="Release" /></a>
  <a href="https://github.com/44886/Exeify/stargazers"><img src="https://img.shields.io/github/stars/44886/Exeify?style=flat&logo=github" alt="Stars" /></a>
</p>

给小白用的图形界面工具：选一个在线网址、或一个本地网页文件夹，点一下就得到一个双击即开的 Windows `.exe`，
还能**同时输出一个安卓 `.apk`**。exe 用 Windows 自带的 WebView2 显示，APK 用安卓系统自带的 WebView 显示，终端用户都无需额外安装。

<img src="docs/screenshot.png" width="420" alt="Exeify 界面" />

</div>

> [!IMPORTANT]
> ### ⚠️ 关于源码 —— 本仓库自 2026-08-16 起不再共享源码
>
> **开源真难。** 最近发现很多人直接拿了源码、改了一点点内容，就署上了自己的名。
>
> **从 2026 年 8 月 16 日起，本仓库不再共享源码，只提供 Release 包。**
> 如果你是**使用者**，可以继续正常使用；如果你是**开发者**，请自行开发。
>
> 📄 相关阅读：**[开源被抄袭，我想说几句 →](https://mp.weixin.qq.com/s/_wpM2gxsth9q3lFRVF7dRQ)**

<p align="center">
  <img src="https://cdn2.44886.com/static/2aa0e9014b059e5d364d5463537097b6.png" width="680" alt="源码被抄袭且未署名的证据" />
</p>

## 特点

- **双端输出**：一次打包，可同时得到 **Windows exe** 和 **安卓 APK**（同一个网页，两个平台的 App）。
- **极致轻量**：打包器与产物都只有约 2 MB（对比 Electron ~100 MB、Pake ~10 MB）。
- **秒级出包**：不编译、不联网，选好点一下，一秒生成。
- **零工具链**：打 exe 不用装 Rust / Node；**打 APK 也不用装 Android SDK / Android Studio**。
- **双模式**：**在线网址** 与 **本地网页目录** 都支持，本地项目完全离线自包含。
- **自动识别入口**：选好本地目录自动扫描网页入口（优先 `index.html`），多个可下拉切换。
- **自定义图标 / 启动页 / 全屏**：exe 与 APK 通用；APK 还会自动套用应用名、图标、启动页、全屏，包名自动生成。
- **源码保护**（exe）：加密内嵌网页资源 + 禁用查看源码，防止产物被直接解压出源码。
- **多开数据互通**：同一应用打开多份 = 单实例多窗口，共用本地服务 / 同源，数据互通、端口零冲突。
- **WebView2 自愈**：产物遇 WebView2 缺失 / 损坏白屏时可一键联网修复；仍不行可清除系统运行时重装（`产物.exe repair-webview2`）。
- **图形界面**：现代极简，专为不懂命令行的人设计。

## 使用方法（图形界面）

1. 下载并运行 `exeify.exe`。
2. 选择模式：
   - **本地网页目录** —— 选一个包含网页的文件夹；**入口文件会自动识别**（多个时可下拉切换）。
   - **在线网址** —— 填一个 `https://` 开头的网址。
3. 设置窗口标题、宽高、是否全屏；可选**图标 / 启动页 / 源码保护**。
4. 在**输出区**选好 exe 保存位置；**想同时做成安卓 App，就勾选「同时输出安卓 APK」**。
5. 点 **开始打包** —— 得到 `xxx.exe`（勾选后还有一个 `xxx.apk`）。
6. exe 双击即运行；apk 传到安卓手机、开"未知来源"安装即可。

> 点击右上角「关于」可查看作者信息与公众号。

## 关于输出的安卓 APK

- **免 Android SDK 本地生成**，秒级出包；产物约 2 MB + 你的网页内容。
- 依赖安卓系统自带的 **Android System WebView**（**Android 7+ / minSdk 24**，覆盖绝大多数机型）。
- 自动套用打包设置：**应用名**（窗口标题）、**图标**、**启动页**、**全屏**；**包名自动生成**（无需手填）。
- 用内置密钥签名，适合**侧载 / 内部分发**；上架应用商店需自带签名密钥（后续版本支持）。

## AI 一键打包（Skill）

已封装成 [Agent Skills](https://agentskills.io) 标准的技能，让 AI 助手按你的一句话把网页打成 exe / apk：

👉 **[exeify-web2exe-skill](https://github.com/44886/exeify-web2exe-skill)** （Claude Code 里 `/plugin marketplace add 44886/exeify-web2exe-skill` 一键装）

## 相对 Pake 的差异

| | Pake | **Exeify** |
|---|---|---|
| 打包时要装的东西 | Node.js + Rust + Tauri 工具链 | **无，下载即用** |
| 打包耗时 | 分钟级（真的在编译） | **秒级（不编译，直接生成）** |
| 输出目标 | 桌面 App | **Windows exe + 安卓 APK** |
| 面向人群 | 开发者 / 命令行 | **小白 / 图形界面** |

> Pake 很棒，主打把在线网址做成精致桌面 App；Exeify 主打 **零门槛、秒出包、exe 与 APK 双端、本地与网址双支持**。

## 命令行（可选，进阶 / 供 AI 与脚本调用）

```bash
exeify pack --local <网页目录> --out app.exe [--apk app.apk] [选项...]
exeify pack --url <网址>       --out app.exe [--apk app.apk] [选项...]
```

`--out`（Windows exe）与 `--apk`（安卓 APK）**至少给一个，可同时给**。常用选项：
`--title 应用名` · `--icon 图标.ico/.png` · `--splash 启动图.png` · `--window fullscreen` · `--no-protect`。
用 `exeify pack --help` 查看全部参数。

## 运行环境

- **打包器**：**Windows 10+**（需 [WebView2 运行时](https://developer.microsoft.com/microsoft-edge/webview2/)，Win11 及较新 Win10 已内置）。
- **exe 产物**：Windows 10+，依赖 WebView2。
- **APK 产物**：**Android 7+**，依赖系统自带的 Android System WebView。

## 获取

本仓库**只提供 Release 二进制包**（不再共享源码，见上方说明）。到 **[Releases](https://github.com/44886/Exeify/releases)** 下载最新的 `exeify.exe` 即可。

## 许可证与署名

基于 **[Apache License 2.0](LICENSE)** 发布，Copyright 2026 **不坑老师**。
官网：<https://www.bukenghezi.com>。
再分发（含二进制）时请保留 [`LICENSE`](LICENSE) 与 [`NOTICE`](NOTICE) 文件（`NOTICE` 含对「不坑老师」的署名，须原样带上）。

## Star History

如果 Exeify 对你有帮助，欢迎点个 ⭐ Star 支持一下！

<a href="https://www.star-history.com/?type=date&repos=44886%2FExeify">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/chart?repos=44886/Exeify&type=date&theme=dark&legend=top-left&sealed_token=YMNWUhDFzTVgQEmYcdbs088CQw1c-GF4lqswUDdWzE0YMGY2ZiQ84jvUOSe9qAYt_tnLyDmycmwfWh5xLJ5X8b8WVKArYOBJcqrvxTGq2SPJCJALNagwfwVt5vqPFVs8iSsfKhXyg1q13JT7GB2ZDxtXZA9SDzx5S2Bll4jfxx1tJik9rp4gPwrqgfu7" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/chart?repos=44886/Exeify&type=date&legend=top-left&sealed_token=YMNWUhDFzTVgQEmYcdbs088CQw1c-GF4lqswUDdWzE0YMGY2ZiQ84jvUOSe9qAYt_tnLyDmycmwfWh5xLJ5X8b8WVKArYOBJcqrvxTGq2SPJCJALNagwfwVt5vqPFVs8iSsfKhXyg1q13JT7GB2ZDxtXZA9SDzx5S2Bll4jfxx1tJik9rp4gPwrqgfu7" />
   <img alt="Star History Chart" src="https://api.star-history.com/chart?repos=44886/Exeify&type=date&legend=top-left&sealed_token=YMNWUhDFzTVgQEmYcdbs088CQw1c-GF4lqswUDdWzE0YMGY2ZiQ84jvUOSe9qAYt_tnLyDmycmwfWh5xLJ5X8b8WVKArYOBJcqrvxTGq2SPJCJALNagwfwVt5vqPFVs8iSsfKhXyg1q13JT7GB2ZDxtXZA9SDzx5S2Bll4jfxx1tJik9rp4gPwrqgfu7" />
 </picture>
</a>
