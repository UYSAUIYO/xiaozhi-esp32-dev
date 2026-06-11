# 六、README.md 与更新日志规则

在完成代码修改后，应检查项目根目录是否存在 `README.md`。

执行：

```bash
test -f README.md && echo "README exists" || echo "README not found"
```

如果存在 `README.md`，应根据本次修改内容加入项目更新日志或变更说明。

推荐在 README 中维护类似结构：

```markdown
## 更新日志

### YYYY-MM-DD
- 新增 xxx 功能。
- 修复 xxx 问题。
- 调整 xxx 组件结构。
- 新增 xxx BSP / middleware。
```

如果 README 已有 `更新日志`、`Changelog`、`Change Log`、`变更记录` 等章节，应优先追加到已有章节。

如果 README 没有更新日志章节，可以建议用户新增，但不要在不确认项目文档风格的情况下大幅重写 README。

完成 README 更新后，必须再次检查 Git 状态：

```bash
git status --short
git diff --stat
```

并向用户给出合适建议，包括：

1. 哪些文件被修改；
2. 哪些文件是新增；
3. 是否建议提交；
4. 建议的 commit message；
5. 是否存在未处理的用户原始改动；
6. 是否建议先构建或运行测试。
