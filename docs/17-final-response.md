# 十七、最终回复用户时必须包含的内容

完成任务后，应向用户总结：

1. 已检查的 Git 状态；
2. 阅读了哪些关键代码路径；
3. 判断该功能属于 BSP、middleware 还是 app；
4. 修改了哪些文件；
5. 新增了哪些文件；
6. 是否更新 README；
7. 是否执行构建；
8. 构建结果；
9. 是否还需要用户提供日志、硬件例程或数据手册；
10. 建议的下一步操作；
11. 建议的 commit message。

示例 commit message：

```text
feat(bsp): add pca9557 io expander driver
fix(audio): resolve amplifier enable timing issue
feat(middleware): add reminder service persistence flow
docs: update README changelog
```
