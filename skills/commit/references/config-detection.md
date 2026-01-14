# 项目配置检测

当检测到项目中的配置文件时，commit skill 会自动调整行为以匹配项目设置。

## 配置检测优先级

按以下顺序查找配置文件：

### 1. cz-git 配置

**commitlint 配置**：
- `.commitlintrc.js` / `.commitlintrc.cjs` / `.commitlintrc.mjs`
- `commitlint.config.js` / `commitlint.config.cjs` / `commitlint.config.mjs`

读取配置：
- `prompt.useEmoji` - 是否使用 emoji
- `prompt.types` - 自定义类型定义
- `prompt.emojiAlign` - emoji 对齐方式

**cz-git 独立配置**：
- `.czrc` (JSON 格式)
- `cz.config.js` / `cz.config.cjs` / `cz.config.mjs`

读取配置：
- `useEmoji` - 是否使用 emoji
- `types` - 自定义类型定义
- `emojiAlign` - emoji 对齐方式

**package.json**：
- `config.commitizen` 字段

### 2. streamich/git-cz 配置

**配置文件**：
- `changelog.config.js` / `.git-cz.json`
- 或在 package.json 中的 `config.commitizen.path` 指向 "git-cz"

**配置项**：
- `disableEmoji` - 是否禁用 emoji（与 useEmoji 相反）
- `format` - commit message 格式模板
- `types` - 类型定义数组
- `scopes` - 可用的 scope 列表

## 配置示例

### cz-git 配置示例

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

### git-cz 配置示例

```javascript
// changelog.config.js
module.exports = {
  disableEmoji: false,
  format: '{type}{scope}: {emoji}{subject}',
  types: {
    feat: {
      description: 'A new feature',
      emoji: '🎸',
      value: 'feat'
    },
    fix: {
      description: 'A bug fix',
      emoji: '🐛',
      value: 'fix'
    },
    // ...
  }
}
```

## 行为差异

| 配置类型 | Emoji 位置 | useEmoji 字段 | 默认行为 |
|---------|-----------|--------------|---------|
| 无配置 | type 和 subject 之间 | - | ✅ 使用 emoji |
| cz-git (useEmoji: true) | type 之前 | ✅ | ✨ feat: ... |
| cz-git (useEmoji: false) | - | ❌ | feat: ... |
| git-cz (disableEmoji: false) | type 和 subject 之间 | - | feat: 🎸 ... |
| git-cz (disableEmoji: true) | - | - | feat: ... |

## 自定义类型处理

如果配置文件定义了自定义 types，将使用项目配置中的：
- type 值
- emoji
- description

示例：

```javascript
types: [
  { value: 'custom', name: 'custom:   Custom change', emoji: ':rocket:' }
]
```

生成：`feat: 🚀 自定义变更`

## 检测逻辑

1. 检查是否存在配置文件
2. 读取并解析配置
3. 确定配置类型（cz-git 或 git-cz）
4. 根据 `useEmoji` 或 `disableEmoji` 确定是否使用 emoji
5. 应用自定义类型（如果有）
6. 使用正确的 emoji 位置和格式
