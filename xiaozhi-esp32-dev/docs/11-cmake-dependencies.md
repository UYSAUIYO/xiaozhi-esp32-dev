# 十一、CMake 依赖规则

新增组件时，必须正确维护 `CMakeLists.txt`。

示例：

```cmake
idf_component_register(
    SRCS "xxx.c"
    INCLUDE_DIRS "include"
    REQUIRES driver freertos
    PRIV_REQUIRES nvs_flash esp_timer
)
```

规则：

1. 对外头文件依赖的组件放入 `REQUIRES`。
2. 只在 `.c` 文件内部使用的依赖放入 `PRIV_REQUIRES`。
3. 不应把所有依赖都无脑放进 `REQUIRES`。
4. 不应通过相对路径跨组件 include 私有头文件。
5. 如果组件需要被多个板子复用，应保持 API 稳定和硬件配置可参数化。
6. 如果项目使用 `idf_component.yml`，应同步维护依赖声明。
