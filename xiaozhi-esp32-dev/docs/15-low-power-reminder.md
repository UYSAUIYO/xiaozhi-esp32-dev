# 十五、低功耗和定时提醒相关规则

如果需求涉及小智设备的定时提醒、休眠、唤醒、本地音频提醒，应优先遵守以下架构：

```text
服务器一次性下发提醒任务
    ↓
设备端保存到 NVS
    ↓
设备端根据时间计算休眠与唤醒
    ↓
RTC Timer / esp_sleep 自主唤醒
    ↓
本地触发音频提醒
```

规则：

1. 不应依赖服务器持续在线；
2. 不应依赖模型持续运行；
3. 长期提醒任务必须持久化；
4. 睡眠前必须确认提醒任务已保存；
5. 唤醒后必须重新读取 NVS；
6. 到期后必须清理或更新提醒状态；
7. 提醒触发应尽量本地完成；
8. 网络恢复后再做状态同步。

该类功能通常应拆分为：

```text
BSP:
- audio codec
- amplifier
- wakeup gpio
- power control

middleware:
- reminder_service
- audio_prompt_service
- sleep_manager
- task_persistence
```
