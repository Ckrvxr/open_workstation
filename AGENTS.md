## 信息搜索

1. 像 Grok AI 一样使用多轮搜索，尽量获取全面详尽的信息。
2. 优先使用搜索 websearch 和抓取 webfetch。
3. 普通搜索与抓取（webfetch/exa_web_fetch_exa）无法获得内容时，使用 Playwright MCP 直接浏览页面。

4. 学术搜索使用 Academix MCP。
5. 搜索或对比对象较多的时候，要分开搜索。
6. 优先使用一手权威来源：

   * 软件：官方文档和仓库
   * 学术：原始论文和综述
   * 医学：指南和监管机构
   * 法律：法规和政府文件
   * 安全：厂商公告、CVE、CERT

7. 重要结论应交叉验证；来源冲突时明确说明。
8. 找不到可靠信息时，明确说明不确定性，不编造结果。

## Playwright MCP 浏览器自动化

Playwright MCP 已配置在本地（`@playwright/mcp@latest`），通过 accessibility tree（不是截图）与页面交互。

### 核心工作流

```
browser_navigate(url) → browser_snapshot() → 取 ref → 操作(ref) → browser_snapshot() 验证
```

每次 `snapshot` 返回元素的 ref（如 `e5`、`e12`），后续用 ref 操作。页面变化后必须重新 snapshot。

### 操作速查

| 工具 | 场景 |
|------|------|
| `browser_navigate` | 打开页面 |
| `browser_snapshot` | 获取 accessibility tree + 元素 ref |
| `browser_click` | 点击（ref 或选择器） |
| `browser_type` | 逐键输入（触发 JS 事件） |
| `browser_fill_form` | 一次填充多个字段（省 token） |
| `browser_select_option` | 下拉框 |
| `browser_take_screenshot` | 截图取证/调试 |
| `browser_wait_for` | 等文本出现/消失/等指定时间 |
| `browser_tabs` | 多标签管理 |
| `browser_handle_dialog` | 处理 alert/confirm/prompt |
| `browser_console_messages` | 看控制台日志 |
| `browser_network_requests` | 查看网络请求 |
| `browser_route` / `browser_unroute` | Mock API |
| `browser_evaluate` | 执行 JS（需要时再用） |
| `browser_file_upload` | 上传文件 |
| `browser_press_key` | 按键盘键 |
| `browser_hover` | 悬停 |
| `browser_drag` | 拖拽 |
| `browser_resize` | 调整窗口 |

### 规则

1. **同一任务只维护一个 Playwright 实例**，完成后关闭。
2. Playwright 环境已准备好，不要重复初始化。
3. **ref 只在当前 snapshot 有效**，页面变化后必须重新 snapshot。
4. **填表优先用 `browser_fill_form`** 一次填多个字段，比逐个 `type` 省 token。
5. **避免固定等待**（`browser_wait_for` 等秒数），优先等文本/元素出现。
6. **调试步骤**：`browser_take_screenshot` + `browser_console_messages` + `browser_network_requests` 三步取证。
7. **遇到验证码/付费墙/登录**：先请求用户登录，如果登录不了或用户没有权限，改找预印本、机构仓储或公开摘要。
8. **需要抓取 GitHub raw 内容**时，`webfetch` 可能超时，改用 `exa_web_fetch_exa` 或 `curl`。

## Context7

当用户询问库、框架、SDK、API、CLI 或云服务的安装、配置、语法、迁移、版本兼容或故障排查时，优先使用 Context7 获取最新文档。

流程：

1. 使用 `resolve-library-id`，传入官方名称和用户完整问题。
2. 优先选择名称准确、版本匹配、来源可靠的结果。
3. 使用 `query-docs` 查询用户的完整问题。
4. Context7 不可用或信息不足时，改查官方文档、官方仓库、Release Notes 或 Issue。

一般编程概念、业务逻辑调试、代码审查、重构和独立脚本编写不需要调用 Context7。

## 多媒体处理策略

1. 使用 Python(uv)、Node.js(pnpm) 辅助处理。
2. PDF、DOCX、XLSX、PPTX：优先提取结构化文本；提取失败时再渲染页面并进行视觉分析。
3. 图片：模型支持时直接进行视觉分析；不支持时转换为 PNG。
4. 音视频：使用 FFmpeg 提取音频，再用本地 Whisper 转录；涉及关键画面内容时截图识别。
5. 优先本地处理，不覆盖、删除或上传用户原文件。

## 技巧

1. 可使用 open-computer-use MCP 操作电脑，自动化操作（点击、输入、读取屏幕等），调试桌面应用。

   1. 流程循环：list_apps → get_app_state 读 element_index（不断变化） → 操作 → 重新 get_app_state 确认
   2. type_text 需要已聚焦 editable 控件
  
2. 可使用 background-process MCP 管理后台进程，启动调试服务器，异步任务（比视频转码）。 

   1. run_command_sync 一次性命令，自动结束。
   2. start_process 后台长任务（dev server/watcher），返回 UUID 后通过 get_process_output（支持 head/tail）查看进度。
   3. list_processes 查看所有进程状态。
   4. 结束先 stop_process 再 clear_process 清理。

3. 复杂的计算与推导使用 Python(SymPy)。

## 约束

1. sudo命令改为pkexec。
2. 优先使用pacman、uv、pnpm、curl、尽量不要用bun、npm。
3. 用户没有明确同意，不要删除和覆盖文件。删除文件为移动到回收站。
4. 用户没有明确同意，不要进行 commit、push 等操作。
5. 代码修改后必须编写并进行 lint unit-test。
6. 所有语法警告必须被消除，包括项目自定义的语法规则。
7. 项目级别的代码必须模块化，除非主动要求。
8. 函数和变量必须准确命名，格式保持统一，不可迁就。代码经若干次修改后，若功能已发生质变，应及时调整命名和结构。即使影响范围较大，也要主动提出，不可默不作声。
9. 修改代码后，相应的文档要同步更新。
10. 常量全大写 MAX_RETRY_COUNT；变量全小写 user_count；函数用动词 load_config；布尔值用判断 is_ready；类型用名词 user_config；集合用复数 users；单位范围入名称 timeout_ms；结构枚举加后缀 device_state_t。
11. C/C++：RAII；嵌入式禁止动态内存；优先23现代语法；优先vecter；优先定宽类型；常量优先级 = enum > constexpr > #define；使用 #pragma once；循环ijk。
