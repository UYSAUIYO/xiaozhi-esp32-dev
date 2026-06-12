# xiaozhi-esp32-dev Skill

面向 `xiaozhi-esp32` 项目的 Qoder Agent Skill，用于约束 AI 在 ESP-IDF 嵌入式开发中的分析、实现、验证和汇报流程。

---

## 这个 Skill 解决什么问题

本 Skill 让 Agent 在处理 `xiaozhi-esp32` 项目任务时遵守统一工程规则，重点避免以下问题：

- 需求不清时自行幻想实现；
- 只看报错文件就开始修 Bug；
- 没有例程或数据手册就编写硬件驱动；
- 覆盖用户已有未提交修改；
- BSP、middleware、app 分层混乱；
- 修改后不构建、不检查、不说明变更范围。

---

## 适用场景

当用户对 `xiaozhi-esp32` 项目提出以下请求时，应使用本 Skill：

- 阅读、分析或修改代码；
- 修复 Bug、编译错误或运行异常；
- 新增功能、接入模块或迁移外设驱动；
- 添加芯片、传感器、屏幕、音频、触摸、IO 扩展等硬件支持；
- 调整 BSP、middleware、CMake、Kconfig 或组件依赖；
- 更新 README、调用链记录或最终变更说明；
- 执行构建、测试、静态检查或调试验证。

---

## 核心 Hooks

这些 hooks 是任务开始或关键动作前必须触发的前置规则。

### 1. Git 工作区检查

在分析代码、修改文件或运行脚本前，先执行：

```bash
git status --short
```

如果存在未提交修改，必须先提醒这些改动可能属于用户已有工作，避免覆盖、回滚或格式化。

### 2. Superpowers 辅助能力检查

任务开始时，如果用户环境安装了 Superpowers skills，应优先尝试利用已有辅助能力，例如：

- 代码搜索；
- 调用链分析；
- 项目索引；
- 测试运行；
- 文档检索；
- 日志分析；
- 变更审查。

原则：不要只靠上下文猜测，优先使用已有工具增强分析。

### 3. 需求与 Bug 上下文阅读

当用户提出需求、Bug、编译报错或运行异常时，不要只看单个文件。必须同步查看：

- 调用链；
- 配置文件；
- 构建系统；
- 组件初始化逻辑；
- Kconfig / CMake / `idf_component.yml`；
- 相似实现和多板卡差异。

### 4. 需求不明确阻断

当需求缺少关键信息时，必须先提问，禁止自行规划边界或生成幻觉代码。

例如用户说“帮我加一个新传感器支持”，应先问：

```text
请提供传感器型号、通信接口、参考例程或数据手册。
```

### 5. 新硬件资料确认

当用户说“添加 XX 芯片支持”“编写 XX 驱动”“接入 XX 模块”“支持新的传感器”时，必须先确认：

- 是否有官方例程、厂商例程、Arduino 例程或 ESP-IDF 例程；
- 如果没有例程，是否有数据手册、寄存器手册、通信协议说明或原理图；
- 如果例程和资料都没有，应停止驱动实现，只说明缺失资料清单。

---

## 架构分层

```text
app / main         业务流程编排
    ↓
middleware         能力抽象、状态管理、策略封装
    ↓
bsp                外设驱动、板级硬件适配
    ↓
esp-idf drivers    底层外设接口
```

任何新代码都必须先判断归属层级，再编写实现。

---

## 推荐工作流

```text
1.  检查 git status
2.  检查是否可用 Superpowers 辅助能力
3.  阅读项目结构
4.  阅读 README.md
5.  判断任务类型
6.  需求不明确时先提问，禁止幻觉实现
7.  新硬件优先索要例程；没有例程时索要数据手册或寄存器手册
8.  Bug 需求索要日志和复现步骤
9.  阅读相关代码、调用链、配置文件、构建系统和组件初始化逻辑
10. 判断代码应放在 BSP / middleware / app 哪一层
11. 进行最小必要修改
12. 更新 CMakeLists.txt
13. 必要时更新 README 更新日志
14. 执行构建或静态检查
15. 再次检查 git status
16. 总结修改内容和下一步建议
```

---

## 文档结构

```text
xiaozhi-esp32-dev/
├── SKILL.md
├── README.md
└── docs/
    ├── 01-git-status-check.md
    ├── 02-code-reading-principles.md
    ├── 03-superpowers-collaboration.md
    ├── 04-unclear-requirements.md
    ├── 05-new-feature-requirements.md
    ├── 06-readme-changelog.md
    ├── 07-bsp-layer-rules.md
    ├── 08-middleware-layer-rules.md
    ├── 09-code-classification.md
    ├── 10-esp-idf-component.md
    ├── 11-cmake-dependencies.md
    ├── 12-minimal-change-principle.md
    ├── 13-driver-migration.md
    ├── 14-debug-logging.md
    ├── 15-low-power-reminder.md
    ├── 16-verification-flow.md
    ├── 17-final-response.md
    ├── 18-prohibited-actions.md
    ├── 19-recommended-workflow.md
    ├── 20-core-principles.md
    ├── 21-call-chain-tracking.md
    └── 22-kconfig-rules.md
```

---

## 重点文档

| 文档 | 用途 |
|------|------|
| [SKILL.md](SKILL.md) | Skill 主入口、hooks 和规则索引 |
| [docs/01-git-status-check.md](docs/01-git-status-check.md) | Git 工作区检查，避免覆盖用户改动 |
| [docs/02-code-reading-principles.md](docs/02-code-reading-principles.md) | 代码阅读范围和上下文要求 |
| [docs/03-superpowers-collaboration.md](docs/03-superpowers-collaboration.md) | Superpowers 协同规则 |
| [docs/04-unclear-requirements.md](docs/04-unclear-requirements.md) | 需求不明确时的提问和阻断规则 |
| [docs/05-new-feature-requirements.md](docs/05-new-feature-requirements.md) | 新功能、新硬件和 Bug 处理规则 |
| [docs/13-driver-migration.md](docs/13-driver-migration.md) | 新硬件驱动迁移流程 |
| [docs/18-prohibited-actions.md](docs/18-prohibited-actions.md) | 禁止事项 |
| [docs/19-recommended-workflow.md](docs/19-recommended-workflow.md) | 推荐工作流 |

---

## 禁止行为摘要

- 不检查 Git 状态就修改代码；
- 只阅读单个文件就开始修复 Bug；
- 需求不明确时自行幻想实现；
- 没有硬件型号、例程或数据手册就写驱动；
- 把 BSP 和 middleware 混在一起；
- 让 BSP 依赖 middleware；
- 大规模重构无关代码；
- 覆盖用户已有未提交修改；
- 修改完成后不说明变更范围。

完整列表见 [docs/18-prohibited-actions.md](docs/18-prohibited-actions.md)。
