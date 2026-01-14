---
name: commit-helper
description: Generate conventional commit messages following git-cz standards. Use when Claude needs to create git commit messages with Conventional Commits specification (<type>[scope]: <subject>). Automatically generates properly formatted commit messages by analyzing git changes and suggesting appropriate type, scope, and description.
---

# Commit Helper

生成符合 Conventional Commits 规范的提交信息。该规范格式为：

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

## Commit Type

根据更改内容选择合适的 type：

- **feat**: 新功能
- **fix**: Bug 修复
- **docs**: 文档更新
- **style**: 代码格式调整（不影响功能）
- **refactor**: 代码重构（不是新功能也不是修复）
- **perf**: 性能优化
- **test**: 添加或修改测试
- **build**: 构建系统或依赖变更
- **ci**: CI/CD 配置变更
- **chore**: 其他不修改 src 或测试文件的更改
- **revert**: 回退之前的 commit

## Scope

可选的 scope，用于标识 commit 影响的范围。例如：
- 组件名：`button`, `header`, `auth`
- 模块名：`api`, `database`, `ui`
- 功能区：`user`, `admin`, `billing`

如果 scope 不明显或涉及多个范围，可以省略。

## Description

使用中文简短描述做了什么：
- 使用祈使句（现在时态）
- 首字母小写
- 不要以句号结尾
- 限制在 50 个字符以内

## Body（可选）

详细描述 what 和 why（不是 how）：
- 每行限制在 72 个字符以内
- 使用中文

## Footer（可选）

- 关联 issue: `Closes #123`, `Fixes #456`
- Breaking changes: `BREAKING CHANGE: API endpoint changed`

## 工作流程

1. **分析变更**：使用 `git status` 和 `git diff` 查看更改
2. **确定 type**：根据变更性质选择合适的 type
3. **确定 scope**（可选）：识别影响范围
4. **编写 description**：简洁描述做了什么
5. **添加 body**（可选）：详细说明背景和原因
6. **执行提交**：使用生成的 commit message

## 参考资源

- 完整的 Conventional Commits 规范：见 [references/spec.md](references/spec.md)
- 更多示例：见 [references/examples.md](references/examples.md)
