# gh 命令食谱

PR/Issue 调研常用命令。`<owner>/<repo>` 替换成实际仓库（如 `NousResearch/hermes-agent`）。

## 多维度搜索

### 按标题关键词

```bash
gh pr list --repo <owner>/<repo> --search "deepseek thinking in:title" --state all --limit 30 \
  --json number,title,state,author,createdAt \
  -q '.[] | "\(.state)\t#\(.number)\t\(.author.login)\t\(.title)"'
```

### 按 body 关键词（捞用不同标题描述同一问题的 PR）

```bash
gh pr list --repo <owner>/<repo> --search "reasoning_effort in:body" --state all --limit 30 \
  --json number,title,state -q '.[] | "\(.state)\t#\(.number)\t\(.title)"'
```

### 按文件路径（**最易漏的维度**）

```bash
gh pr list --repo <owner>/<repo> --search "plugins/model-providers/deepseek" --state all --limit 30 \
  --json number,title,state -q '.[] | "\(.state)\t#\(.number)\t\(.title)"'
```

### 按 author（看某人是否已经在做同样的事）

```bash
gh pr list --repo <owner>/<repo> --author <username> --state all --json number,title,state
```

### 同时搜 issues（bug 报告角度的同问题）

```bash
gh issue list --repo <owner>/<repo> --search "deepseek reasoning" --state all --limit 20 \
  --json number,title,state,labels -q '.[] | "\(.state)\t#\(.number)\t\(.title)"'
```

## 批量看 diff / metadata

### 取多个 PR 的简要对比

```bash
for pr in 22218 16448 21052; do
  echo "=== #$pr ==="
  gh pr view $pr --repo <owner>/<repo> \
    --json title,state,createdAt,author,additions,deletions,files
done
```

### 直接看完整 diff（不靠脑补）

```bash
gh pr diff <num> --repo <owner>/<repo>
```

### 看 PR 的 review/comments 历史（看 maintainer 的偏好与拒绝理由）

```bash
gh pr view <num> --repo <owner>/<repo> --comments
gh api repos/<owner>/<repo>/pulls/<num>/reviews -q '.[] | {state, user: .user.login, body: .body[:200]}'
```

## 架构现状

### 看最近合并的相关 PR（识别架构迁移方向）

```bash
gh pr list --repo <owner>/<repo> --state merged --search "<keyword>" --limit 20 \
  --json number,title,mergedAt -q '.[] | "\(.mergedAt[:10])\t#\(.number)\t\(.title)"'
```

### 找已注册的 profile/plugin（看你要修的入口是不是已经有占位）

```bash
gh api repos/<owner>/<repo>/contents/<path/to/plugins/dir> \
  -q '.[] | select(.type=="dir") | .name'
```

## 排查"自动机制是否已覆盖"

### 看远程能力目录是否已收录你的 case

```bash
# 例：models.dev 公共 API
curl -s https://models.dev/api.json | jq '.deepseek.models | keys'
```

### 在 codebase 内搜自动发现入口

```bash
# 找所有 fetch_* / discover_* / supports_* 函数
grep -rn "def \(fetch_\|discover_\|supports_\)" --include="*.py" <project_root>
```

## 关闭自己 PR 时的标准模板

```bash
gh pr comment <my-pr> --repo <owner>/<repo> \
  --body "Acknowledged — closing as a duplicate of #<earliest-equivalent>. I missed it in my PR-graph survey before opening this one. Apologies for the noise."
gh pr close <my-pr> --repo <owner>/<repo>
```

不解释、不辩护、不"但是我有 X 优势"——保持简短、专业。
