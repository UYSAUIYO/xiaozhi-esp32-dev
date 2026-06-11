# 九、功能代码归类判断规则

在编写任何新功能代码前，必须先判断代码属于哪一层。

## 9.1 属于 BSP 的情况

如果代码直接操作以下内容，通常属于 BSP：

1. GPIO；
2. I2C；
3. SPI；
4. UART；
5. I2S；
6. ADC；
7. PWM；
8. 芯片寄存器；
9. 硬件复位引脚；
10. 硬件使能引脚；
11. 外设初始化；
12. 外设基础读写。

示例：

```text
pca9557_write_reg()
ns4150b_enable()
es8311_init()
button_gpio_read()
i2c_bus_init()
```

这些应放在 BSP。

---

## 9.2 属于 middleware 的情况

如果代码处理的是以下内容，通常属于 middleware：

1. 状态机；
2. 事件抽象；
3. 数据滤波；
4. 业务策略；
5. 多个 BSP 能力组合；
6. 配置管理；
7. 提醒任务管理；
8. 音频播放策略；
9. 低功耗策略；
10. 用户输入事件转换。

示例：

```text
reminder_service_set_task()
audio_prompt_play_warning()
button_event_get()
led_status_set_mode()
sensor_data_filter_update()
```

这些应放在 middleware。

---

## 9.3 属于 main / app 的情况

如果代码只负责产品业务流程编排，通常属于 main 或 app 层。

示例：

```text
app_main()
app_start_network()
app_start_voice_assistant()
app_register_reminder_service()
```

main / app 层应保持简洁，只做初始化顺序、服务启动和业务流程连接，不应堆积外设细节。
