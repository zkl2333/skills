---
name: check-updates
description: 检查 OpenClaw 版本更新。当用户说"检查更新"、"有没有新版本"、"update"、"升级"时使用。执行标准三步检查：当前版本、npm 最新版、GitHub 最新版，并给出更新内容摘要和是否建议更新的判断。
---

# Check Updates

## 流程

### 1. 获取三方版本

同时执行：

```bash
openclaw --version
npm show openclaw version
```

再访问 GitHub releases 页（web_search 或 web_fetch）获取 GitHub 最新版。

### 2. 对比并汇报

| 项目 | 值 |
|------|----|
| 当前版本 | `openclaw --version` 输出 |
| npm 最新 | `npm show openclaw version` 输出 |
| GitHub 最新 | 从 releases 页获取 |

**注意：** GitHub 版本有时超前于 npm（发布延迟）。三者不同时需明确说明。

### 3. 查更新内容

优先用 `web_search` 搜索 `openclaw <版本号> changelog release notes`，获取：
- 新增功能
- 安全修复
- Breaking changes

### 4. 给出判断

- 有安全修复 → **建议立即更新**
- 仅功能/修复 → 说明更新内容，让用户决定
- GitHub 超前 npm → 注明"npm 尚未同步，建议等 npm 版本"

### 5. 执行更新（用户确认后）

```
gateway action=update.run
```

更新完成后自动重启。如有安全审计建议，提示运行 `openclaw security audit`。

## 注意事项

- 我们用 **npm 安装**，更新走 `gateway update.run`，不要手动 git pull
- GitHub 版本号有时比 npm 超前，不要误报"有新版本但装不了"
- 更新后检查是否有 CRITICAL 安全问题（`openclaw security audit`）
