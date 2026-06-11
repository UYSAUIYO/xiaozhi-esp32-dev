# 三、Superpowers Skill 协同原则

如果用户环境中安装了 Superpowers 的 skills，应优先尝试结合 Superpowers 和本 Skill 进行问题解决。

处理原则：

1. 如果存在可用的 Superpowers Skill，应优先读取相关 Skill 的规则。
2. 本 Skill 负责约束 `xiaozhi-esp32` 项目的工程结构、ESP-IDF 组件化方式、BSP/middleware 分层和调试流程。
3. Superpowers Skill 可用于增强：

   * 代码搜索；
   * 任务规划；
   * 测试执行；
   * 重构；
   * 日志分析；
   * 多文件编辑；
   * 变更审查。
4. 如果 Superpowers Skill 与本 Skill 存在冲突，应优先遵守本 Skill 中关于 `xiaozhi-esp32` 项目结构和嵌入式开发流程的要求。
5. 不应假设 Superpowers 一定存在；应先检查再使用。
