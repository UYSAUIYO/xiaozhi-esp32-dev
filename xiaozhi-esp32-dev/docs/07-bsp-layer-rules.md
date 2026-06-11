# 七、BSP 分层规则

当新功能属于外设驱动、板级硬件适配或硬件基础能力封装时，应创建或修改 BSP 层，而不是直接写入某个板子的主业务代码。

BSP 的定义：

BSP，即 Board Support Package，是对板级硬件和外设驱动能力的最基础封装。它负责提供硬件初始化、基础读写、底层控制、GPIO/I2C/SPI/UART/I2S/PWM/ADC 等资源适配能力。

BSP 适合放置：

1. 传感器基础驱动；
2. Codec / 功放 / 麦克风基础驱动；
3. IO 扩展芯片基础驱动；
4. 屏幕、灯带、电机、电源管理芯片驱动；
5. 板级 GPIO 定义；
6. I2C/SPI/UART 总线初始化；
7. 硬件复位、使能、睡眠控制；
8. 板级硬件资源映射。

BSP 不应放置复杂业务逻辑。

推荐目录结构：

```text
bsp/
└── <hardware_name>/
    ├── CMakeLists.txt
    ├── include/
    │   └── <hardware_name>.h
    └── <hardware_name>.c
```

例如新增 PCA9557 IO 扩展芯片驱动：

```text
bsp/
└── pca9557/
    ├── CMakeLists.txt
    ├── include/
    │   └── pca9557.h
    └── pca9557.c
```

对应 `CMakeLists.txt` 示例：

```cmake
idf_component_register(
    SRCS "pca9557.c"
    INCLUDE_DIRS "include"
    REQUIRES driver esp_common
)
```

如果项目已经有统一的 BSP 目录规范，应遵守现有结构，不应擅自创建不一致的新结构。

关键要求：

1. BSP 包不应创建在某个指定板子固件主文件路径下。
2. BSP 应创建在统一的 BSP 目录体系中。
3. 目标板子的主文件或组件应通过 `CMakeLists.txt` 引用 BSP。
4. 不应为了快速实现功能而把外设驱动直接堆进 `main.c`。
5. 不应把硬件底层驱动和业务逻辑混在同一个文件里。
