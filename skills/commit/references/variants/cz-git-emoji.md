# cz-git Emoji 格式

当检测到 cz-git 配置且 `useEmoji: true` 时使用此格式。

## 格式

```
<emoji> <type>[(<scope>)]: <subject>
```

**特点**：
- Emoji 位于行首（type 之前）
- Emoji 和 type 之间有空格
- 需要 `useEmoji: true` 配置

## 配置示例

```javascript
// commitlint.config.js
module.exports = {
  prompt: {
    useEmoji: true,
    emojiAlign: 'center',
    types: [
      { value: 'feat', name: 'feat:     A new feature', emoji: ':sparkles:' },
      { value: 'fix', name: 'fix:      A bug fix', emoji: ':bug:' },
      // ...
    ]
  }
}
```

## 示例

```
✨ feat: 添加用户注册功能
🐛 fix(api): 修复查询参数解析错误
📝 docs: 更新 README 安装说明
💄 style: 统一代码缩进格式
♻️ refactor: 重构用户数据获取逻辑
⚡ perf: 优化列表渲染性能
✅ test: 添加用户模块单元测试
📦 build: 升级 webpack 到 5.x
🔨 chore: 更新依赖包版本
🎡 ci: 添加 GitHub Actions 工作流
⏪ revert: 回退之前的提交
```

## 类型映射

| Type | Emoji | 说明 |
|------|-------|------|
| feat | ✨ | 新功能 |
| fix | 🐛 | Bug 修复 |
| docs | 📝 | 文档更新 |
| style | 💄 | 代码格式 |
| refactor | ♻️ | 代码重构 |
| perf | ⚡ | 性能优化 |
| test | ✅ | 测试相关 |
| build | 📦 | 构建系统 |
| ci | 🎡 | CI 配置 |
| chore | 🔨 | 其他更改 |
| revert | ⏪ | 回退 |

## 完整示例

```
✨ feat: 实现文件上传功能

支持用户上传头像和文档文件，包含以下特性：
- 限制文件大小为 10MB
- 支持图片预览
- 添加上传进度显示

技术细节：
- 使用 multer 处理文件上传
- 客户端使用 FormData API

Closes #234
```

## 何时使用

- 检测到 `.commitlintrc.*` 或 `commitlint.config.*`
- 配置中 `prompt.useEmoji: true`
- 或检测到 `.czrc` / `cz.config.*` 且 `useEmoji: true`
