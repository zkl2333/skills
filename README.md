# Skills

个人 Claude Code skills 集合

## 包含的 Skills

### commit

生成符合 Conventional Commits 规范的 git commit message。

**特点：**
- 默认使用 streamich/git-cz 风格（emoji 在 type 和 description 之间）
- 自动检测项目配置（cz-git、git-cz、commitlint）
- 支持根据项目配置自定义 emoji 和类型

**使用场景：**
- 创建规范的 commit message
- 遵循 Conventional Commits 规范
- 支持带 emoji 的提交信息

**触发时机：**
当需要创建 git commit 时自动使用

**默认格式：**
```
feat: 🎸 添加新功能
fix: 🐛 修复 bug
docs: ✏️ 更新文档
```

## 安装

将 skills 目录添加到 Claude Code 配置中。

## 开发

基于 [skill-creator](https://github.com/anthropics/skills) 指南创建。
