# OpenWorkStation 🖥️⚡

一套基于 [OpenCode](https://opencode.ai) 和各种 MCP 的全能 AI 工作站配置。

## 🎯 设计原则

- ⚖️ **规则驱动，而非流程驱动** — AGENTS.md 定义边界和策略，具体执行交给 AI 判断
- 🧩 **模块化** — 每个 MCP 服务器、技能、子 Agent 独立文件，可按需启用/禁用
- 🪒 **奥卡姆剃刀** — 每项能力只搭配一个工具，但这个工具能力是目前可用的最好选择

## 🛠️ 能力

| 能力 | 工具 | 效果 |
|---|---|---|
| 💻 代码编写与重构 | OpenCode + AGENTS.md 宪法约束 | 超强自主代码能力 |
| 🌐 浏览器自动化 | Playwright MCP | 超强信息检索、互联网操作能力 |
| 🐙 GitHub 全操作 | GitHub MCP (Copilot 接口) | 不再报错，自动处理 PR/Issue  |
| 📖 神经网络搜索 | Exa MCP | 信息实时可靠、搜索迅速，大幅减少上下文消耗 |
| 📖 学术搜索 | Academix MCP | 查询学术论文、引文网络、权威可靠 |
| 🧠 持久化记忆 | Engram MCP | 持续积累项目管理经验 |
| 🖱️ 桌面 GUI 操作 | Open Computer Use MCP | 直接操控桌面应用，全自动 loop 开发调试 GUI 程序 |
| 📦 隔离环境 | Docker MCP | 不污染宿主机，解决 Root 权限问题 |
| ⏳ 后台任务 | Background Process MCP | 管理 dev server / 长时间下载 |
| 📊 配额监控 | opencode-quota plugin  | 实时查看 Token 消耗与余额 |
| 🔒 密钥保护 | opencode-secret-redactor plugin | 自动拦截密钥泄漏 |
| 🎓 费曼学习法 | Feynman Skill | AI 陪你查漏补缺，直到真正掌握 |

## 📂 结构

```
.
├── AGENTS.md           # 📜 宪法规则、搜索/计算/工具箱策略
├── opencode.jsonc      # ⚙️ MCP 服务器、插件、权限配置
├── tui.jsonc           # 🖥️ 终端 UI 插件配置
├── skills/
│   └── feynman/        # 🎓 费曼学习法技能
├── plugins/
│   └── engram.ts       # 🧠 Engram 记忆存储插件
├── commands/           # ➕ 自定义命令
└── .gitignore
```

## 🚀 快速开始

```bash
# 克隆后复制到 OpenCode 配置目录
cp -r . ~/.config/opencode

# 设置密钥环境变量
sed -i '/^[[:space:]]*export GITHUB_PERSONAL_ACCESS_TOKEN=/d' ~/.zshrc
printf '%s\n' 'export GITHUB_PERSONAL_ACCESS_TOKEN="ghp_..."' >> ~/.zshrc
sed -i '/^[[:space:]]*export OPENALEX_EMAIL=/d' ~/.zshrc
printf '%s\n' 'export OPENALEX_EMAIL="your@email.com"' >> ~/.zshrc
source ~/.zshrc

```
