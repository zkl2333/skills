# 纯文本格式

不使用 emoji 的简单格式。

## 格式

```
<type>[(<scope>)]: <subject>
```

**特点**：
- 不使用 emoji
- 纯文本格式
- 最简洁

## 示例

```
feat: 添加用户注册功能
fix(api): 修复查询参数解析错误
docs: 更新 README 安装说明
style: 统一代码缩进格式
refactor: 重构用户数据获取逻辑
perf: 优化列表渲染性能
test: 添加用户模块单元测试
chore: 更新依赖包版本
ci: 添加 GitHub Actions 工作流
```

## 类型

- **feat**: 新功能
- **fix**: Bug 修复
- **docs**: 文档更新
- **style**: 代码格式
- **refactor**: 代码重构
- **perf**: 性能优化
- **test**: 测试相关
- **chore**: 其他更改
- **ci**: CI 配置

## 完整示例

```
feat: 实现文件上传功能

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

- git-cz 配置中 `disableEmoji: true`
- cz-git 配置中 `useEmoji: false`
- 团队明确要求不使用 emoji
- 需要兼容不支持 emoji 的工具

## 优缺点

**优点**：
- 格式简洁
- 兼容性好
- 不依赖 emoji 显示

**缺点**：
- 缺少视觉标识
- 类型不够直观
- 难以快速识别 commit 类型
