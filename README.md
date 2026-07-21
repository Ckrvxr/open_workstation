# OpenWorkStation 🖥️⚡

一套基于 [OpenCode](https://opencode.ai) 和各种 MCP 的全能 AI 工作站配置。

## 🎯 设计原则

- ⚖️ **规则驱动，而非流程驱动** — AGENTS.md 定义边界和策略，具体执行交给 AI 判断
- 🧩 **模块化** — 每个 MCP 服务器、技能、子 Agent 独立文件，可按需启用/禁用
- 🪒 **奥卡姆剃刀** — 每项能力只搭配一个工具，但这个工具能力是目前可用的最好选择

## 🛠️ 能力

| 能力 | 工具 |
|---|---|
| 💻 代码编写与重构 | OpenCode 核心 + AGENTS.md 宪法约束 |
| 🌐 浏览器自动化 | Playwright MCP + Browser Sub-Agent |
| 🐙 GitHub 全操作 | GitHub MCP (Copilot 接口) |
| 📖 学术搜索 | Academix MCP (OpenAlex / DBLP / Semantic Scholar) |
| 🧠 持久化记忆 | Engram MCP |
| 🖱️ 桌面 GUI 操作 | Open Computer Use MCP |
| 📦 隔离环境 | Docker MCP (Debian 容器) |
| ⏳ 后台任务 | Background Process MCP |
| 🎓 费曼学习法 | Feynman Skill |
| 📊 配额监控 | opencode-quota 插件 |
| 🔒 密钥保护 | secret-redactor 插件 |

## 📂 结构

```
.
├── AGENTS.md           # 📜 宪法规则、搜索/计算/工具箱策略
├── opencode.jsonc      # ⚙️ MCP 服务器、插件、权限配置
├── tui.jsonc           # 🖥️ 终端 UI 插件配置
├── agents/
│   └── browser.md      # 🕵️ 浏览器 Sub-Agent 定义
├── skills/
│   └── feynman/        # 🎓 费曼学习法技能
├── plugins/
│   └── engram.ts       # 🧠 Engram 记忆存储插件
├── commands/           # ➕ 自定义命令（可扩展）
└── .gitignore
```

## 🚀 快速开始

```bash
# 克隆后复制到 OpenCode 配置目录
cp -r . ~/.config/opencode

# 确保需要环境变量（按需设置）
export GITHUB_PERSONAL_ACCESS_TOKEN="ghp_..."
export OPENALEX_EMAIL="your@email.com"
```