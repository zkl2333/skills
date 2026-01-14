# Git-cz 默认格式

这是 commit skill 的默认格式，当项目没有特殊配置时使用。

## 格式

```
<type>[(<scope>)]: <emoji> <subject>
```

**特点**：
- Emoji 位于 type 和 subject 之间（冒号之后）
- Emoji 和 subject 之间有空格
- 默认启用 emoji

## 示例

```
feat: 🎸 添加用户注册功能
fix(api): 🐛 修复查询参数解析错误
docs: ✏️ 更新 README 安装说明
style: 💄 统一代码缩进格式
refactor: 💡 重构用户数据获取逻辑
perf: ⚡️ 优化列表渲染性能
test: 💍 添加用户模块单元测试
chore: 🤖 更新依赖包版本
ci: 🎡 添加 GitHub Actions 工作流
release: 🏹 发布 v1.0.0
```

## 类型映射

| Type | Emoji | 说明 |
|------|-------|------|
| feat | 🎸 | 新功能 |
| fix | 🐛 | Bug 修复 |
| docs | ✏️ | 文档更新 |
| style | 💄 | 代码格式 |
| refactor | 💡 | 代码重构 |
| perf | ⚡️ | 性能优化 |
| test | 💍 | 测试相关 |
| chore | 🤖 | 构建工具 |
| ci | 🎡 | CI 配置 |
| release | 🏹 | 发布 |

## 完整示例

```
feat: 🎸 实现文件上传功能

支持用户上传头像和文档文件，包含以下特性：
- 限制文件大小为 10MB
- 支持图片预览
- 添加上传进度显示
- 服务端文件类型验证

技术细节：
- 使用 multer 处理文件上传
- 客户端使用 FormData API
- 文件存储在 OSS

Closes #234
```

## 何时使用

- 项目没有 commitizen/cz-git 配置
- 项目使用 streamich/git-cz
- 想要 emoji 在 type 和 subject 之间的格式
