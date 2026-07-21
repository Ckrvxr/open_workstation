# 约束与宪法

1. `sudo` 命令改为 `pkexec`。
2. 用户没有明确同意，不要删除和覆盖文件。删除文件为移动到回收站。
3. 用户没有明确同意，不要进行 `commit`、`push` 等操作。
4. 代码修改后必须编写并进行 `lint` 与 `unit-test`。
5. 所有语法警告必须被消除，包括项目自定义的语法规则。
6. 项目级别的代码必须模块化，除非主动要求。
7. 函数和变量必须准确命名，格式保持统一，不可迁就。
   - 代码经若干次修改后，若功能已发生质变，应及时调整命名和结构。
   - 即使影响范围较大，也要主动提出，不可默不作声。
8. 修改代码后，相应的文档要同步更新。
9. 命名规范：
    - 常量全大写：`MAX_RETRY_COUNT`
    - 变量全小写：`user_count`
    - 函数用动词：`load_config`
    - 布尔值用判断：`is_ready`
    - 类型用名词：`user_config`
    - 集合用复数：`users`
    - 单位范围入名称：`timeout_ms`
    - 结构枚举加后缀：`device_state_t`
10. C/C++ 规范：
    - RAII
    - 嵌入式禁止动态内存
    - 优先 C++23 现代语法
    - 优先 `vector`
    - 优先定宽类型
    - 常量优先级：`enum` > `constexpr` > `#define`
    - 使用 `#pragma once`
    - 循环用 `i`、`j`、`k`
11. 优先使用 `pacman`、`uv`、`pnpm`、`curl`；尽量不要用 `bun`、`npm`。

# 策略与方法

## 知识、信息搜索

1. 像 Grok AI 一样使用多轮搜索，尽量获取全面详尽的信息。
2. 优先使用搜索 `websearch` 和抓取 `webfetch`。
3. 普通搜索与抓取无法获得内容时，用 `task` 启动 Browser Agent 浏览页面，不要在主 Agent 中直接使用 Playwright。
4. GitHub 仓库、Issue、PR、Actions、Release、Asset 和 Raw 内容：使用 GitHub MCP；不得使用`curl`、`webfetch`、直接 GitHub API URL 绕过 MCP。
5. 学术搜索使用 Academix MCP。
6. 搜索或对比对象较多的时候，要分开搜索。
7. 优先使用一手权威来源：
   - 软件：官方文档和仓库
   - 学术：原始论文和综述
   - 医学：指南和监管机构
   - 法律：法规和政府文件
   - 安全：厂商公告、CVE、CERT
8. 重要结论应交叉验证；来源冲突时明确说明。
9. 找不到可靠信息时，明确说明不确定性，不编造结果。

## 多媒体处理策略

1. 使用 Python(`uv`)、Node.js(`pnpm`) 辅助处理。
2. PDF、DOCX、XLSX、PPTX：优先提取结构化文本；提取失败时再渲染页面并进行视觉分析。
3. 图片：模型支持时直接进行视觉分析；不支持时转换为 PNG。
4. 音视频：使用 FFmpeg 提取音频，再用本地 Whisper 转录；涉及关键画面内容时截图识别。
5. 优先本地处理，不覆盖、删除或上传用户原文件。

## 计算与推导

1. 复杂的、大量的计算请使用 Python 计算以保证结果准确
2. 可以使用 SymPy、NumPy 等工具帮助计算与推导。

# 工具箱

## 浏览器 Browser Sub-Agent

1. 调用时，必须传入：明确目标、起始网址（如有）、权限和副作用约束、已知信息与失败尝试、期望证据或输出格式。不得只传一个宽泛主题。
2. 返回结果时应包含：`Result`（结果）、`Evidence`（证据）、`State`（当前页面/标签或下载状态）、`Blocker`（阻塞原因）和 `Next step`（确有必要时的下一步）。

## 文档 Context7 MCP

当用户询问库、框架、SDK、API、CLI 或云服务的安装、配置、语法、迁移、版本兼容或故障排查时，优先使用 Context7 获取最新文档。

流程：

1. 使用 `resolve-library-id`，传入官方名称和用户完整问题。
2. 优先选择名称准确、版本匹配、来源可靠的结果。
3. 使用 `query-docs` 查询用户的完整问题。
4. Context7 不可用或信息不足时，改查官方文档、官方仓库、Release Notes 或 Issue。

一般编程概念、业务逻辑调试、代码审查、重构和独立脚本编写不需要调用 Context7。

## 后台进程管理 Background Process MCP

可使用 background-process MCP 管理后台进程，启动调试服务器、异步任务（如视频转码）。

1. `run_command_sync`：异步运行命令，会运行停止或推出的程序用这个执行，尽量少用，请勿滥用。
2. `start_process`：后台长任务（dev server / watcher），返回 UUID 后通过 `get_process_output`（支持 head / tail）查看进度。
3. `list_processes`：查看所有进程状态。
4. 结束流程：先 `stop_process`，再 `clear_process` 清理。

## 电脑使用 Open Computer Use MCP

可使用 open-computer-use MCP 操作电脑，自动化操作（点击、输入、读取屏幕等），调试桌面应用。

1. 流程循环：`list_apps` → `get_app_state` 读 `element_index`（不断变化） → 操作 → 重新 `get_app_state` 确认。
2. `type_text` 需要已聚焦 editable 控件。
