# 八、Middleware 分层规则

当新功能不是外设的最基础驱动，而是对外设驱动能力的二次抽象、数据处理、业务适配、能力组合或状态管理时，应创建或修改 middleware 层。

Middleware 的定义：

middleware 是中间件层。它位于 BSP 和业务主代码之间，用于对外设驱动能力、外设产生的数据、配置流程、状态逻辑、业务调用方式进行再处理、再设置、再引用或抽象化。
middleware 的目的，是保持主业务代码相对整洁，并避免业务层直接面对复杂底层驱动细节。

Middleware 适合放置：

1. 对传感器数据的滤波、校准、融合；
2. 对音频设备的播放策略、提示音管理；
3. 对按键事件的单击、双击、长按识别；
4. 对触摸输入的去抖和事件抽象；
5. 对 LED 的状态灯模式封装；
6. 对低功耗唤醒策略的封装；
7. 对 NVS 配置的业务化管理；
8. 对多个 BSP 驱动的组合调用；
9. 对设备能力的统一接口抽象。

推荐目录结构：

```text
middleware/
└── <feature_name>/
    ├── CMakeLists.txt
    ├── include/
    │   └── <feature_name>.h
    └── <feature_name>.c
```

例如对本地提醒功能做中间件封装：

```text
middleware/
└── reminder_service/
    ├── CMakeLists.txt
    ├── include/
    │   └── reminder_service.h
    └── reminder_service.c
```

对应 `CMakeLists.txt` 示例：

```cmake
idf_component_register(
    SRCS "reminder_service.c"
    INCLUDE_DIRS "include"
    REQUIRES nvs_flash esp_timer esp_hw_support
    PRIV_REQUIRES driver
)
```

middleware 可以依赖 BSP，但 BSP 不应反向依赖 middleware。

正确依赖方向：

```text
main / app
    ↓
middleware
    ↓
BSP
    ↓
ESP-IDF driver / esp_timer / nvs / freertos
```

错误依赖方向：

```text
BSP
    ↓
middleware
```
