# 三、Superpowers Skill 协同原则

如果用户环境中安装了 Superpowers 的 skills，应优先尝试结合 Superpowers 和本 Skill 进行问题解决。

该规则应作为 Superpowers 辅助能力检查 hook 在任务开始时自动触发。其目的不是替代本 Skill，而是在代码搜索、调用链分析、项目索引、测试运行、文档检索等方面增强分析能力，避免只靠当前上下文猜测。

Hook 自动提醒：

```text
不要只靠上下文猜测，优先使用已有工具增强分析。
```

处理原则：

1. 先检查用户环境中是否存在可用的 Superpowers skills，不应假设 Superpowers 一定存在。
2. 如果存在可用且与当前任务相关的 Superpowers Skill，应优先读取相关 Skill 的规则。
3. 本 Skill 负责约束 `xiaozhi-esp32` 项目的工程结构、ESP-IDF 组件化方式、BSP/middleware 分层和调试流程。
4. Superpowers Skill 可用于增强：

   * 代码搜索；
   * 调用链分析；
   * 项目索引；
   * 测试执行；
   * 文档检索；
   * 重构；
   * 日志分析；
   * 多文件编辑；
   * 变更审查。
5. 如果 Superpowers Skill 与本 Skill 存在冲突，应优先遵守本 Skill 中关于 `xiaozhi-esp32` 项目结构和嵌入式开发流程的要求。
6. 如果没有安装 Superpowers skills，应继续使用本 Skill 的项目规则和本地工具完成任务。
