# 二、代码阅读原则：不能只看局部代码

当用户提出新需求、功能修改、Bug 修复、编译报错、运行异常或架构调整时，不能只阅读与关键词直接相关的单个文件。

必须主动了解更多上下文，包括但不限于：

1. 项目目录结构；
2. 根目录 `CMakeLists.txt`；
3. 目标板子的主入口文件；
4. 当前板子的组件引用关系；
5. `components/`、`main/`、`boards/`、`bsp/`、`middleware/` 等目录；
6. 相关头文件和实现文件；
7. 当前功能的调用链；
8. 当前功能涉及的 Kconfig、menuconfig、idf_component.yml；
9. 当前功能是否已有类似实现；
10. 当前功能是否存在多板卡、多型号或多配置差异。

推荐先执行：

```bash
find . -maxdepth 3 -type f | sort
```

再根据项目规模进一步查看：

```bash
find . -name "CMakeLists.txt" -o -name "Kconfig" -o -name "idf_component.yml"
```

如果项目较大，应优先定位当前板子固件的入口路径和组件依赖关系，而不是盲目全局修改。
