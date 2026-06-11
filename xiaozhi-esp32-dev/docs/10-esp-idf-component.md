# 十、ESP-IDF 组件化要求

新增 BSP 或 middleware 时，应遵守 ESP-IDF 组件化方式。

每个组件至少包含：

```text
<component_name>/
├── CMakeLists.txt
├── include/
│   └── <component_name>.h
└── <component_name>.c
```

头文件应提供清晰 API，不应暴露不必要的内部结构。

实现文件中内部变量和内部函数应使用 `static` 修饰。

函数命名应带模块前缀，例如：

```c
esp_err_t reminder_service_init(void);
esp_err_t reminder_service_start(void);
esp_err_t pca9557_init(void);
esp_err_t pca9557_set_level(uint8_t pin, bool level);
```

错误返回值优先使用：

```c
esp_err_t
```

错误处理应使用：

```c
ESP_RETURN_ON_ERROR(...)
ESP_GOTO_ON_ERROR(...)
ESP_ERROR_CHECK(...)
```

日志应使用：

```c
ESP_LOGI
ESP_LOGW
ESP_LOGE
ESP_LOGD
```
