---
name: exeify-web2exe
description: "把网页打包成 Windows exe 的技能。当用户想把「一个本地 HTML/CSS/JS 网页文件夹」或「一个在线网址」打包成可双击运行、免安装的独立 Windows .exe（用系统自带 WebView2 渲染，终端用户无需装任何东西）时使用；可设置窗口标题/尺寸/全屏、程序图标、启动页、源码保护等；还能**同时输出安卓 APK**（把同一个网页做成安卓 App，免 Android SDK）。仅 Windows 运行打包器。 | Use when the user wants to package a website — a local HTML/CSS/JS folder or an online URL — into a standalone, double-click-to-run Windows .exe (rendered via the system WebView2, no install for end users); supports window title/size/fullscreen, app icon, splash screen, and source protection; can also output an Android APK from the same website (no Android SDK needed). Packer runs on Windows only. | 触发/Triggers: 打包成exe、网页打包、网页转exe、把网址做成桌面程序、把文件夹做成exe、package website to exe、turn HTML folder into a desktop app、website to exe、网页转apk、打包成安卓app、把网页做成安卓应用、website to apk、web to android app。"
license: Apache-2.0
compatibility: "Windows only. The bundled exeify.exe is a Windows executable; packed apps rely on WebView2, which is built into Windows 10/11. Not usable on macOS or Linux."
---

# Exeify 网页打包器（把网页/网址打包成 Windows exe）

用 **Exeify** 把「一个本地网页目录」或「一个在线网址」打包成一个**双击即运行的 Windows `.exe`**。
产物用系统自带的 WebView2 渲染，终端用户无需安装任何东西。

## 前置检查
1. **仅 Windows 可用**。若当前不是 Windows，直接告诉用户此 skill 只能在 Windows 上运行，停止。
2. **定位 exeify.exe**：它**打包在本 skill 的目录里**（与本 `SKILL.md` 同一个文件夹，文件名 `exeify.exe`）。
   - 解析路径的可靠方式：取本 SKILL.md 所在目录，拼上 `exeify.exe`。在 Claude Code 里可用变量
     `${CLAUDE_SKILL_DIR}\exeify.exe`；其它环境用本文件所在目录的绝对路径。
   - **若该文件不存在**（未随 skill 一起分发）：从
     https://github.com/44886/Exeify/releases/latest 下载最新的 `exeify-*-windows-x64.exe`，
     重命名为 `exeify.exe` 放到本 skill 目录后再用。

## 检查 Exeify 是否有新版（可选、非阻塞；exe 已随 skill 内置，离线照常用）
> exeify.exe **始终随本 skill 一起分发**，离线即可打包，此检查纯属锦上添花。
1. 取**已安装版本**：运行 `exeify.exe version`（打印形如 `0.4.1` 后立即退出）。
2. 取**最新版本**：`GET https://api.github.com/repos/44886/Exeify/releases/latest`
   （带 header `User-Agent: exeify-skill`），读 `tag_name`（形如 `v0.4.2`）。
3. 若最新版**更高**：
   - 先一句话告知：「Exeify 有新版 `<tag>`（当前 `<installed>`）」。
   - **征得用户同意后**，从该 release 下载 `exeify-*-windows-x64.exe`，覆盖本 skill 目录里的
     `exeify.exe`（即 `${CLAUDE_SKILL_DIR}\exeify.exe`）；之后的打包即用新版。用户不同意就继续用内置版。
- **访问不畅时**（不少中国大陆用户无法顺畅访问 GitHub）：检查或下载失败就**静默跳过**，
  直接用**已内置**的 `exeify.exe` 正常打包——不依赖网络、不影响出包。
- 别为此反复联网或刷屏，每次会话最多做一次。

## 向用户问清这些（缺就问，别乱猜）
- **源**（二选一）：要打包的**本地网页目录**（含 index.html 的文件夹）**或**一个**在线网址**（http/https）。
- **输出路径**：产物 `.exe` 存哪、叫什么（默认可放在源目录旁，如 `app.exe`）。
- 可选：窗口标题、窗口尺寸、是否**全屏/最大化**、程序**图标**、**启动图**、是否关闭**源码保护**（默认开）。

## 调用方式
命名参数 CLI（推荐，能力全）。可输出 **Windows exe** 和/或 **安卓 APK**：
```
exeify.exe pack --local <目录> [--entry index.html] --out <app.exe> [--apk <app.apk>] [选项...]
exeify.exe pack --url <网址>   --out <app.exe> [--apk <app.apk>] [选项...]
```
参数表：

| 参数 | 说明 | 默认 |
|---|---|---|
| `--local <目录>` / `--url <网址>` | 源，二选一必填 | — |
| `--out <路径.exe>` | 输出 Windows exe（.exe 结尾） | 见下 |
| `--apk <路径.apk>` | 输出安卓 APK（.apk 结尾）；应用名=--title、图标=--icon、启动图=--splash、全屏=--window fullscreen 自动复用，包名自动生成 | 见下 |
| `--entry <文件>` | 本地入口文件（仅 --local） | index.html |
| `--title <文字>` | 窗口标题 | App |
| `--width <数字>` / `--height <数字>` | 窗口宽 / 高 | 1024 / 720 |
| `--window <模式>` | 启动状态 normal\|maximized\|fullscreen | normal |
| `--no-resizable` | 禁止缩放窗口 | 允许 |
| `--icon <路径>` | 窗口与 exe 图标（.ico/.png） | 默认图标 |
| `--splash <图片>` | 启动图（.png/.jpg），消除白屏 | 无 |
| `--splash-bg <#rrggbb>` | 启动图背景色 | #0f172a |
| `--splash-sec <秒>` | 启动图最少显示秒数 | 1.5 |
| `--no-protect` | 关闭源码保护（默认加密内嵌资源+禁用查看源码） | 开启 |

- `--out` 与 `--apk` **至少给一个**，可**同时给**（一次同时产出 exe + apk）。
- 安卓 APK **免 Android SDK 本地生成**；产物依赖系统自带的 Android System WebView（**Android 7+**）；安装需在手机开"未知来源"。用内置密钥签名，适合侧载/内部分发。

用 `exeify.exe pack --help` 可随时打印完整用法。

## 执行与结果判定
1. 拼好命令后用 Bash/PowerShell 运行（路径含空格要加引号）。
2. **成功**：标准输出打印一行 `OK: <输出路径>`，返回码 0。→ 告诉用户产物路径、可双击运行；提醒需 Windows 10+（自带 WebView2）。
3. **失败**：stderr 打印 `失败: <原因>`，返回码 1（打包错误）或 2（用法/参数错误）。→ 把原因转述给用户并修正参数重试。
4. 运行后**确认输出 .exe 文件确实生成**（检查文件存在与大小），再向用户报告成功。

## 例子
- 本地目录、全屏、带图标：
  `exeify.exe pack --local "D:\site" --out "D:\site\app.exe" --title "我的应用" --window fullscreen --icon "D:\logo.ico"`
- 在线网址、固定尺寸、关源码保护（便于调试）：
  `exeify.exe pack --url https://example.com --out "D:\demo.exe" --width 1280 --height 800 --no-protect`

## 说明与边界
- 产物依赖 Windows 自带的 WebView2（Win10/11 通常已内置）。
- 源码保护是「提高门槛」而非绝对加密（详见 Exeify 项目说明）。
- 本地目录会被完整打包进 exe（离线自包含）；网址模式需联网。
- 项目主页：https://github.com/44886/Exeify
