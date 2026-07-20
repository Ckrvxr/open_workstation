## 信息搜索

1. 像 Grok AI 一样使用多轮搜索，尽量获取全面详尽的信息。
2. 优先使用搜索 websearch 和抓取 webfetch。
3. 普通搜索与抓取无法获得内容时，再使用 Playwright MCP。

   1. 同一任务只维护一个 Playwright 实例，完成后关闭。
   2. Playwright 环境已准备好，不要重复初始化。
   3. 遇到付费墙、验证码、登录或访问控制；先请求用户登录，如果登录不了或用户没有权限，改找预印本、机构仓储或公开摘要。

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
