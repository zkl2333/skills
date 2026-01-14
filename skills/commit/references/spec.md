# Conventional Commits 规范

> **说明**：本文档是 Conventional Commits 的基础规范。实际使用时，commit skill 会根据项目配置（git-cz 或 cz-git）自动添加 emoji，但核心格式保持不变。

## 格式

**基础格式（无 emoji）**：
```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

**git-cz 风格（默认）**：
```
<type>[optional scope]: <emoji> <description>

[optional body]

[optional footer(s)]
```

**cz-git 风格（检测到配置时）**：
```
<emoji> <type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

## Type 必填

- **feat**: 新功能
- **fix**: Bug 修复
- **docs**: 仅文档更改
- **style**: 不影响代码运行的格式调整（空格、格式、分号等）
- **refactor**: 既不是新功能也不是修复的代码变更
- **perf**: 性能优化
- **test**: 添加缺失的测试或修正现有测试
- **build**: 影响构建系统或外部依赖的更改
- **ci**: CI/CD 配置文件和脚本的更改
- **chore**: 其他不修改 src 或测试文件的更改
- **revert**: 回退之前的 commit

## Scope 可选

scope 表示 commit 影响的范围，基于项目结构而定。
例如：`button`, `auth`, `api`, `database` 等。

## Description 必填

- 使用祈使句、现在时态（"add" 不是 "added" 或 "adds"）
- 首字母小写
- 不要以句号（.）结尾

## Body 可选

- 使用祈使句、现在时态
- 包含动机和与之前行为的对比
- 每行限制在 72 个字符以内

## Footer 可选

- **Breaking Changes**: 以 `BREAKING CHANGE:` 开头，后跟变更描述
- **关联 Issues**: 使用 `Closes #123` 或 `Fixes #456` 等格式
- 可以有多个 footer

## 示例

**git-cz 风格（默认，带 emoji）**：

完整示例：

```
feat: 🎸 添加用户认证功能

实现基于 JWT 的用户认证系统，支持登录和注册功能。

- 添加 /api/auth/login 端点
- 添加 /api/auth/register 端点
- 集成 JWT token 验证中间件

Closes #123
```

Breaking Changes 示例：

```
feat(api): 🎸 移除 deprecated 的用户列表端点

BREAKING CHANGE: /api/users/list 端点已被移除，使用 /api/users 替代
```

**纯文本格式（无 emoji）**：

```
feat: 添加用户认证功能

实现基于 JWT 的用户认证系统，支持登录和注册功能。

- 添加 /api/auth/login 端点
- 添加 /api/auth/register 端点
- 集成 JWT token 验证中间件

Closes #123
```
