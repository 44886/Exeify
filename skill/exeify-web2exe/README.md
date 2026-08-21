# exeify-web2exe skill

> **这里是源稿存档。** 正式发布版在独立仓库 <https://github.com/44886/exeify-web2exe-skill>，
> 那边是 Claude Code 插件市场结构，且有 CI 每天自动把内置 `exeify.exe` 同步到本仓库最新 Release。
> 安装请用：`/plugin marketplace add 44886/exeify-web2exe-skill`，不要照下面的手动步骤操作。

把 Exeify 封装成一个 Claude Code / AI skill —— 让 AI 按用户意图，把「本地网页目录」或
「在线网址」打包成 Windows `.exe`。

## 内容
- `SKILL.md` —— skill 定义（触发条件 + 调用 `exeify.exe pack` 命名参数 CLI 的说明）。
- `exeify.exe` —— **打包进 skill 的 Exeify 可执行文件（本仓库 `.gitignore` 忽略了 `*.exe`，故不入库，需自行放入）**。

## 安装 / 使用
1. 把本目录复制到用户 skill 目录：`~/.claude/skills/exeify-web2exe/`
   （Windows：`C:\Users\<用户名>\.claude\skills\exeify-web2exe\`）。
2. 放入 `exeify.exe`：从 https://github.com/44886/Exeify/releases 下载最新版，
   或用本仓库 `cargo build --release` 产物，改名为 `exeify.exe` 放进该目录。
3. 重启 Claude Code，skill `exeify-web2exe` 即可用。用户说「把这个网页/文件夹/网址打包成 exe」
   时，AI 会自动用它。

## 维护
- Exeify 发新版后，用新版 `exeify.exe` 替换 skill 目录里的旧的即可（CLI 契约保持稳定：
  `exeify pack ...` 成功打印 `OK: <路径>`）。
- CLI 完整参数见 `exeify.exe pack --help` 或 `SKILL.md` 里的参数表。
