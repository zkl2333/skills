# Commit Message 示例

## 基本示例

### 新功能
```
feat: 添加用户注册功能
feat(api): 添加新的商品搜索接口
feat(auth): 实现 OAuth 登录
```

### Bug 修复
```
fix: 修复登录页面在移动端的显示问题
fix(api): 修复查询参数解析错误
fix(button): 修复点击事件未触发的 bug
```

### 文档更新
```
docs: 更新 README 安装说明
docs(api): 补充 API 文档示例
docs: 修正部署指南中的错误命令
```

### 代码格式
```
style: 统一代码缩进格式
style: 移除未使用的 import
style: 调整组件样式格式
```

### 重构
```
refactor: 重构用户数据获取逻辑
refactor(auth): 简化认证流程
refactor: 提取公共组件到 utils
```

### 性能优化
```
perf: 优化列表渲染性能
perf(api): 添加数据库查询索引
perf: 减少不必要的重渲染
```

### 测试
```
test: 添加用户模块单元测试
test(auth): 补充登录流程测试
test: 修复测试用例中的断言错误
```

### 构建/CI
```
build: 升级 webpack 到 5.x
ci: 添加 GitHub Actions 工作流
build: 配置生产环境构建优化
```

### 其他
```
chore: 更新依赖包版本
chore: 添加 .gitignore 文件
chore: 配置代码格式化工具
```

## 带详细描述的示例

```
feat: 实现文件上传功能

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
Ref #123
```

```
fix(api): 修复并发请求时的竞态条件

问题描述：当多个请求同时发送时，可能导致数据不一致。

解决方案：
- 添加请求队列机制
- 实现请求去重逻辑
- 添加适当的错误处理

Fixes #456
```

## Breaking Changes 示例

```
feat(api): 重构认证系统

使用新的基于 token 的认证机制替代 session 认证。

BREAKING CHANGE: 不再支持 cookie-based session 认证。
所有客户端需要升级到使用 Bearer token。

迁移指南请参考：docs/migration-guide.md
```

```
refactor(database): 重新设计用户表结构

优化数据库结构以提高查询性能。

BREAKING CHANGE: `users` 表结构已变更。
字段 `full_name` 拆分为 `first_name` 和 `last_name`。
需要运行数据迁移脚本。
```

## Revert 示例

```
revert: 回退 "feat: 添加新功能"

此功能存在问题，需要重新设计。

This reverts commit 1a2b3c4d
```
