---
name: xiaozhi-esp32-dev
description: 辅助 xiaozhi-esp32 项目的需求分析、代码阅读、Bug 修复、新功能开发、外设驱动迁移、BSP 分层、中间件抽象、README 更新和 Git 状态检查。在处理任何代码任务时触发。
---

# xiaozhi-esp32 开发 Skill

## 目标

本 Skill 用于辅助 `xiaozhi-esp32` 项目的需求分析、代码阅读、Bug 修复、新功能开发、外设驱动迁移、BSP 分层、中间件抽象、README 更新和 Git 状态检查。
在处理任何代码任务时，必须优先保证代码变更可追踪、需求边界清晰、工程结构合理、实现符合 ESP-IDF 项目的组件化开发习惯。

---

## 文档索引

本 Skill 的规则拆分为以下 21 个文档，按用户所需阅读（推荐全部阅读）：

| 序号 | 文档 | 说明 |
|------|------|------|
| 1 | [Git 状态检查](docs/01-git-status-check.md) | 阅读或修改代码前必须先检查 Git 工作区状态，避免覆盖用户已有改动 |
| 2 | [代码阅读原则](docs/02-code-reading-principles.md) | 不能只看局部代码，必须主动了解项目结构、调用链、组件依赖等上下文 |
| 3 | [Superpowers 协同](docs/03-superpowers-collaboration.md) | 与 Superpowers Skill 共存时的协同使用和冲突处理原则 |
| 4 | [需求不明确时提问](docs/04-unclear-requirements.md) | 硬件型号、接口边界、目标板子不明确时必须先提问，禁止幻想实现 |
| 5 | [新功能需求处理](docs/05-new-feature-requirements.md) | 新硬件驱动支持和 Bug 修复两类需求的完整处理流程和索要信息清单 |
| 6 | [README 与更新日志](docs/06-readme-changelog.md) | 代码修改后必须维护 README 更新日志，并给出 commit 建议 |
| 7 | [BSP 分层规则](docs/07-bsp-layer-rules.md) | 外设驱动和板级硬件适配必须放入 BSP 层，附目录结构和 CMake 示例 |
| 8 | [Middleware 分层规则](docs/08-middleware-layer-rules.md) | 能力二次抽象、状态管理、策略封装必须放入 middleware 层，附依赖方向说明 |
| 9 | [代码归类判断](docs/09-code-classification.md) | 判断新代码应放在 BSP、middleware 还是 main/app 层的具体判断标准 |
| 10 | [ESP-IDF 组件化要求](docs/10-esp-idf-component.md) | 组件目录结构、函数命名前缀、错误处理和日志规范 |
| 11 | [CMake 依赖规则](docs/11-cmake-dependencies.md) | REQUIRES 与 PRIV_REQUIRES 的正确使用，避免依赖泄漏 |
| 12 | [最小变更原则](docs/12-minimal-change-principle.md) | Bug 修复时必须遵守最小必要变更，禁止大规模重构无关代码 |
| 13 | [硬件驱动迁移流程](docs/13-driver-migration.md) | 用户提供硬件例程后的 16 步迁移流程，从分析到构建验证 |
| 14 | [调试日志增强](docs/14-debug-logging.md) | BSP、Middleware、App 三层分别应添加哪些类型的调试日志 |
| 15 | [低功耗与定时提醒](docs/15-low-power-reminder.md) | 休眠/唤醒/本地提醒架构设计，NVS 持久化和离线提醒规则 |
| 16 | [修改后验证流程](docs/16-verification-flow.md) | 代码修改后必须执行构建或静态检查的验证步骤 |
| 17 | [最终回复模板](docs/17-final-response.md) | 完成任务后向用户汇报的必备内容清单和 commit message 示例 |
| 18 | [禁止事项](docs/18-prohibited-actions.md) | 项目中明确禁止的 15 种行为，包括幻想实现、混合分层、忽略 Git 等 |
| 19 | [推荐工作流](docs/19-recommended-workflow.md) | 每次任务的 15 步完整执行流程，从 Git 检查到总结汇报 |
| 20 | [核心设计原则](docs/20-core-principles.md) | 项目整体架构思想：app → middleware → bsp → esp-idf drivers |
| 21 | [调用链追踪](docs/21-call-chain-tracking.md) | 在项目根目录维护 Call_chain.md，用 Mermaid 流程图 + 调用链表 + 文件路径记录每次代码变更的调用逻辑 |
| 22 | [Kconfig 规则：板级配置项的正确放置](docs/22-kconfig-rules.md) | 新增板级支持时，必须在 `main/Kconfig.projbuild` 中添加对应的 Kconfig 条目。错误放置会导致配置项在 `menuconfig` 中不可见。 |
