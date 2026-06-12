# xiaozhi-esp32-dev Skill

面向 `xiaozhi-esp32` 项目的 Qoder Agent Skill，规范 AI 在嵌入式开发中的行为准则。

---

## 用途

本 Skill 约束 Agent 在处理 `xiaozhi-esp32` 项目任务时的工作方式，覆盖以下场景：

- **需求分析**：需求不明确时强制提问，禁止幻想实现
- **代码阅读**：要求阅读完整上下文，不只看单个文件
- **新功能开发**：判断代码归属层级（BSP / Middleware / App），遵守 ESP-IDF 组件化规范
- **外设驱动迁移**：基于例程或数据手册的 16 步标准化迁移流程
- **Bug 修复**：最小变更原则，优先复现定位再修复
- **BSP / Middleware 分层**：清晰的架构边界和依赖方向
- **调试日志**：三层（BSP / Middleware / App）分层日志规范
- **低功耗与提醒**：离线 NVS 持久化、RTC 休眠唤醒架构设计
- **Git 工作流**：任务前后必须检查工作区状态，规范 commit message
- **调用链追踪**：每次代码变更自动维护项目根目录 `Call_chain.md`，用 Mermaid 流程图 + 调用链表记录调用逻辑

---

## 目录结构

```
xiaozhi-esp32-dev/
├── SKILL.md          # 主入口，包含 frontmatter 元数据和文档索引
├── README.md         # 本文件，Skill 使用说明
└── docs/             # 20 个规则子文档
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

## 触发条件

当用户对 `xiaozhi-esp32` 项目发起以下操作时，本 Skill 会被自动加载：

- 提出新功能需求
- 报告 Bug 或请求修复
- 要求阅读或分析代码
- 请求外设驱动迁移
- 要求修改 BSP 或 Middleware
- 请求构建、调试或验证
- 要求记录或查看代码调用链

---

## 架构核心思想

```
app / main        ← 业务流程编排
      ↓
middleware        ← 能力抽象、状态管理、策略封装
      ↓
bsp               ← 外设驱动、板级硬件适配
      ↓
esp-idf drivers   ← 底层外设接口
```

任何新代码都必须先判断归属层级，再编写实现。

---

## 快速参考：推荐工作流

```
1.  检查 git status
2.  阅读项目结构
3.  阅读 README.md
4.  判断任务类型
5.  需求不明确 → 向用户提问
6.  新硬件     → 索要例程或数据手册
7.  Bug        → 索要日志和复现步骤
8.  阅读相关代码和调用链
9.  判断 BSP / Middleware / App 归属层
10. 最小必要修改
11. 更新 CMakeLists.txt
12. 更新 README 日志
13. 执行构建或静态检查
14. 再次检查 git status
15. 总结变更，给出 commit message 建议
16. 更新 Call_chain.md 调用链记录
```

---

## 禁止行为（摘要）

- 不检查 Git 就改代码
- 只读单文件就修 Bug
- 需求不清时幻想实现
- 没有数据手册就写驱动
- BSP 和 Middleware 混在一起
- BSP 反向依赖 Middleware
- 大规模重构无关代码
- 修改完不汇报变更范围

完整列表见 [docs/18-prohibited-actions.md](docs/18-prohibited-actions.md)。
