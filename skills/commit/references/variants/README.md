# Commit 格式变体

本文档目录包含不同配置下的 commit message 格式说明。

## 快速对比

| 配置 | 格式 | Emoji 位置 | 详细说明 |
|------|------|-----------|---------|
| 默认（无配置） | `type: emoji subject` | type 和 subject 之间 | [git-cz-default.md](git-cz-default.md) |
| cz-git (useEmoji: true) | `emoji type: subject` | type 之前 | [cz-git-emoji.md](cz-git-emoji.md) |
| cz-git (useEmoji: false) | `type: subject` | 无 emoji | [cz-git-no-emoji.md](cz-git-no-emoji.md) |
| git-cz (disableEmoji: true) | `type: subject` | 无 emoji | [plain.md](plain.md) |

## 使用说明

根据检测到的配置，加载对应的变体文档：

1. **无配置或 git-cz 配置**：查看 [git-cz-default.md](git-cz-default.md)
2. **检测到 cz-git 配置**：
   - `useEmoji: true` → [cz-git-emoji.md](cz-git-emoji.md)
   - `useEmoji: false` → [cz-git-no-emoji.md](cz-git-no-emoji.md)
3. **禁用 emoji**：查看 [plain.md](plain.md)

## 类型映射差异

不同配置下的 type 和 emoji 映射见各自文档。
