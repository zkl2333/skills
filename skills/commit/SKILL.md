---
name: commit
description: Generate conventional commit messages following git-cz style by default (emoji between type and subject). Automatically detects project configuration when present. Use when creating git commit messages with Conventional Commits specification. See references/config-detection.md for supported configurations.
---

# Commit

生成符合 Conventional Commits 规范的提交信息。

## 默认格式

```
<type>[(<scope>)]: <emoji> <subject>
```

**示例**：
```
feat: 🎸 添加用户注册功能
fix(api): 🐛 修复查询参数解析错误
docs: ✏️ 更新 README 安装说明
```

## Commit Type

根据更改内容选择：

- **feat** 🎸: 新功能
- **fix** 🐛: Bug 修复
- **docs** ✏️: 文档更新
- **style** 💄: 代码格式调整（不影响功能）
- **refactor** 💡: 代码重构（不是新功能也不是修复）
- **perf** ⚡️: 性能优化
- **test** 💍: 添加或修改测试
- **chore** 🤖: 构建过程或辅助工具的变更
- **ci** 🎡: CI 配置变更
- **release** 🏹: 创建发布提交

## Scope（可选）

标识 commit 影响的范围，例如：`api`、`auth`、`database`、`ui`

如果 scope 不明显或涉及多个范围，可以省略。

## Description（必填）

- 使用祈使句、现在时态："添加"不是"添加了"
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

1. **检测配置**：查找项目配置文件（见下方）
2. **分析变更**：使用 `git status` 和 `git diff` 查看更改
3. **确定 type**：根据变更性质选择合适的 type
4. **确定 scope**（可选）：识别影响范围
5. **编写 description**：简洁描述做了什么
6. **生成 message**：组合成完整的 commit message

## 配置检测

自动检测项目配置以确定正确的格式和 emoji 位置。

### 检测优先级

按顺序查找以下配置文件：

1. **cz-git 配置**：
   - `.commitlintrc.*` / `commitlint.config.*`
   - `.czrc` / `cz.config.*`
   - 读取 `useEmoji` 和 `types`

2. **git-cz 配置**：
   - `changelog.config.js` / `.git-cz.json`
   - 读取 `disableEmoji` 和 `types`

3. **无配置**：使用默认 git-cz 风格

### 格式差异

| 配置 | 格式 | 示例 |
|------|------|------|
| 无配置（默认） | `type: emoji subject` | `feat: 🎸 新功能` |
| cz-git (useEmoji: true) | `emoji type: subject` | `✨ feat: 新功能` |
| cz-git (useEmoji: false) | `type: subject` | `feat: 新功能` |

**详细说明**：[references/config-detection.md](references/config-detection.md)

## 参考资源

- **完整规范**：[references/spec.md](references/spec.md)
- **更多示例**：[references/examples.md](references/examples.md)
- **格式变体**：[references/variants/](references/variants/)
  - [快速对比](references/variants/README.md)
  - [默认格式](references/variants/git-cz-default.md)
  - [cz-git 格式](references/variants/cz-git-emoji.md)
  - [纯文本格式](references/variants/plain.md)
