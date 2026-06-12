---
name: xiaozhi-esp32-dev
description: 辅助 xiaozhi-esp32 项目的需求分析、代码阅读、Bug 修复、新功能开发、外设驱动迁移、BSP 分层、中间件抽象、README 更新和 Git 状态检查。在处理任何代码任务时触发。
---

# xiaozhi-esp32 开发 Skill

## 目标

本 Skill 用于辅助 `xiaozhi-esp32` 项目的需求分析、代码阅读、Bug 修复、新功能开发、外设驱动迁移、BSP 分层、中间件抽象、README 更新和 Git 状态检查。
在处理任何代码任务时，必须优先保证代码变更可追踪、需求边界清晰、工程结构合理、实现符合 ESP-IDF 项目的组件化开发习惯。

---

## Hooks

### Git 工作区检查 Hook

当 AI 准备执行以下任一动作之前，必须先自动运行 Git 工作区检查逻辑：

1. 分析代码、阅读调用链或定位问题前；
2. 修改、新增、删除、格式化代码或文档前；
3. 运行构建、测试、脚本、代码生成、格式化或其他可能改写工作区的命令前。

Hook 检查逻辑：

```bash
git status --short
```

如发现当前仓库存在未提交修改，必须先提示用户：

- 当前工作区存在未提交修改；
- 这些改动可能属于用户已有工作；
- 在继续分析、修改或运行脚本前，建议先确认这些改动是否需要保留、是否与当前任务相关，避免覆盖用户已有改动。

若未提交修改与当前任务相关，AI 必须先阅读并纳入分析；若无关，必须避开这些文件或明确说明潜在冲突风险。禁止在未确认来源和影响的情况下覆盖、回滚或格式化用户已有改动。

### 需求与 Bug 上下文阅读 Hook

当用户提出需求、功能修改、Bug 修复、编译报错、运行异常或类似请求时，AI 必须自动提醒并执行上下文阅读逻辑：

- 不要只看报错文件、用户点名文件或关键词命中的单个文件；
- 需要同时查看调用链、配置文件、构建系统和相关组件初始化逻辑；
- 需要确认当前功能所在层级，以及是否涉及 `main/`、`components/`、`boards/`、`bsp/`、`middleware/`、Kconfig、CMake 或 `idf_component.yml`；
- 在形成修复方案前，必须说明已阅读的上下文范围，以及仍缺少哪些日志、复现步骤、板卡型号或配置条件。

当用户说“帮我修复这个 bug”或提供报错信息时，AI 应先提醒自己：

```text
不要只看报错文件，还需要查看调用链、配置文件、构建系统、相关组件初始化逻辑。
```

### 需求不明确阻断 Hook

当用户提出的需求缺少关键条件时，AI 必须先阻断实现流程并向用户提问，不得自行规划需求边界、假设硬件参数或生成幻觉代码。

触发条件包括但不限于：

- 新增传感器、外设、芯片、板级能力，但未提供准确型号；
- 未说明通信接口、GPIO 接线、I2C/SPI/UART 总线、地址或中断脚；
- 未提供参考例程、数据手册、原理图或现有驱动迁移来源；
- 未说明目标板子、固件路径、ESP-IDF 版本或期望功能边界；
- Bug 需求缺少现象、日志、复现步骤、期望行为或实际行为。

当用户说“帮我加一个新传感器支持”但信息不足时，AI 必须先提问：

```text
请提供传感器型号、通信接口、参考例程或数据手册。
```

对于硬件类需求，至少问清：型号、通信接口、接线方式、目标板子、参考例程或数据手册、期望功能边界。未获得这些关键信息前，禁止直接新增驱动、配置 GPIO、编写寄存器初始化序列或修改构建系统。

### 新硬件资料确认 Hook

当用户提出新硬件驱动编写或加入新硬件支持时，AI 必须先确认是否存在可迁移资料，再开始设计或修改代码。

触发条件包括但不限于用户提到：

- 添加 XX 芯片支持；
- 编写 XX 驱动；
- 接入 XX 模块；
- 支持新的传感器；
- 新增屏幕、音频芯片、Codec、触摸芯片、IO 扩展、电源管理芯片、灯带、电机或其他外设。

Hook 自动提醒：

```text
先确认是否存在官方例程、Arduino 例程、ESP-IDF 例程、寄存器手册、通信协议说明，再开始迁移代码。
```

处理顺序：

1. 优先索要官方例程、厂商例程、Arduino 例程、ESP-IDF 例程或当前项目可参考的已有驱动；
2. 如果没有例程代码，必须索要数据手册、寄存器手册、通信协议说明或原理图；
3. 如果例程和资料都没有，必须停止驱动实现，只能说明缺失信息和可准备的最小资料清单；
4. 获得资料后，先分析初始化流程、通信接口、寄存器配置、时序要求、GPIO 依赖、中断或轮询方式，再迁移到 ESP-IDF 组件结构。

### Superpowers 辅助能力检查 Hook

任务开始时，如果用户环境中安装了 Superpowers 的 skills，AI 必须优先尝试结合 Superpowers 和本 Skill 强化问题解决能力；不要只靠当前上下文猜测。

Hook 自动提醒：

```text
不要只靠上下文猜测，优先使用已有工具增强分析。
```

应检查并优先利用可用的辅助能力，包括但不限于：

- 代码搜索；
- 调用链分析；
- 项目索引；
- 测试运行；
- 文档检索；
- 日志分析；
- 多文件编辑；
- 变更审查。

使用原则：

1. 先检查 Superpowers skills 是否存在，不假设一定可用；
2. 如果存在与当前任务相关的 Superpowers skill，优先读取其规则并结合本 Skill 使用；
3. 本 Skill 继续负责约束 `xiaozhi-esp32` 的 ESP-IDF 组件化、BSP/middleware 分层、Git 检查和硬件驱动流程；
4. 如果 Superpowers 与本 Skill 冲突，优先遵守本 Skill 对本项目工程结构和嵌入式流程的约束。

---

## 文档索引

本 Skill 的规则拆分为以下 22 个文档，按用户所需阅读（推荐全部阅读）：

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
