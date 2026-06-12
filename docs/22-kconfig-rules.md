# 二十二、Kconfig 规则：板级配置项的正确放置

新增板级支持时，必须在 `main/Kconfig.projbuild` 中添加对应的 Kconfig 条目。错误放置会导致配置项在 `menuconfig` 中不可见。

## 22.1 理解 menu 嵌套结构

`Kconfig.projbuild` 使用 `menu`/`endmenu` 划分区块，部分 menu 带有 `depends on` 条件：

```kconfig
menu "Xiaozhi Assistant"              # 顶层 menu，无条件

    choice
        prompt "Board Type"            # 板级选择
        config BOARD_TYPE_XXX
        ...
    endchoice

    menu "TAIJIPAI_S3_CONFIG"          # 只对太极派可见！
        depends on BOARD_TYPE_ESP32S3_Taiji_Pi
        ...
    endmenu

endmenu
```

**关键规则：带有 `depends on` 的 menu 内部的任何配置项，都会被该 conditions 过滤。**

## 22.2 新增板级的 Kconfig 放置规则

1. **`BOARD_TYPE` 配置**：必须放在板级 `choice` 块内（与同架构的其他板子并列，如 `depends on IDF_TARGET_ESP32S3`）。

2. **板级专属子选项**（如屏幕类型、网络类型）：**必须放在带条件的 menu 块之外**，在顶层 menu 中独立成 `choice` 或 `config`，并通过自身的 `depends on BOARD_TYPE_XXX` 控制可见性。

3. **插入前必须检查**：
   - 当前是否在某个有 `depends on` 的 `menu` 块内？
   - 上方最近的 `menu` 声明是否有条件？
   - 最近的 `endmenu` 关闭的是哪个 `menu`？

## 22.3 正确示例

```kconfig
menu "Xiaozhi Assistant"
    ...
    menu "TAIJIPAI_S3_CONFIG"
        depends on BOARD_TYPE_ESP32S3_Taiji_Pi
        ...
    endmenu  # ← 太极派 menu 结束

    choice DISPLAY_TYPE_NEW_BOARD      # ← 放在顶层 menu，仅在选该板时可见
        depends on BOARD_TYPE_NEW_BOARD
        prompt "NEW_BOARD LCD Type"
        config USE_LCD_OPTION_A
            bool "..."
        config USE_LCD_OPTION_B
            bool "..."
    endchoice

    choice NET_TYPE_NEW_BOARD
        depends on BOARD_TYPE_NEW_BOARD
        prompt "NEW_BOARD NET Type"
        config USE_WIFI_ONLY
            bool "..."
        config USE_4G_ONLY
            bool "..."
    endchoice
endmenu
```

## 22.4 错误示例：塞进其他板子的 menu

```kconfig
menu "Xiaozhi Assistant"
    ...
    menu "TAIJIPAI_S3_CONFIG"
        depends on BOARD_TYPE_ESP32S3_Taiji_Pi
        ...
        choice DISPLAY_TYPE_NEW_BOARD   # ❌ 被太极派的 depends on 过滤！
            depends on BOARD_TYPE_NEW_BOARD
            ...
        endchoice
    endmenu
endmenu
```

**后果**：`DISPLAY_TYPE_NEW_BOARD` 只有在 `BOARD_TYPE_ESP32S3_Taiji_Pi` 和 `BOARD_TYPE_NEW_BOARD` 同时为 y 时才可见——这永远不会发生（两个 board 互斥）。

## 22.5 Kconfig 验证清单

完成 Kconfig 修改后：

1. **运行 menuconfig 验证**：`idf.py menuconfig`，切换到目标板级，确认子选项是否展开。
2. **检查生成的头文件**：`grep` 生成的 `sdkconfig.h` 确认 `CONFIG_USE_LCD_*` 等宏确实被定义。
3. **检查宏一致性**：确认 Kconfig 中定义的 `config USE_XXX` 名与 C 代码中 `#if CONFIG_USE_XXX` 一致。
4. **缩进检查**：`choice`/`endchoice` 的缩进必须匹配所在 menu 层级（通常 0 或 4 空格）。
