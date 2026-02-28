# Skills

个人 Agent Skills 集合，兼容 Claude Code、Codex、OpenClaw 等支持 AgentSkill 规范的平台。

## Skills

| Skill | 说明 | 备注 |
|-------|------|------|
| `amap-maps` | 高德地图（地理编码/路线/天气/POI） | MCP wrapper ¹ |
| `bark` | Bark(day.app) 推送通知 | |
| `caiyun-weather` | 彩云天气（实时/预报/预警） | MCP wrapper ¹ |
| `check-updates` | OpenClaw 版本更新检查 | OpenClaw 专用 |
| `codex-cli` | OpenAI Codex CLI 工作流封装 | |
| `commit` | Conventional Commits/emoji 风格提交 | |
| `web-reader` | 网页正文提取 | MCP wrapper ¹ |
| `web-search-prime` | Web Search | MCP wrapper ¹ |
| `zai-mcp-server` | 视觉/OCR/UI→artifact | MCP wrapper ¹ |
| `zread` | GitHub 仓库文档检索 | MCP wrapper ¹ |

> ¹ **MCP wrapper**：这些 skill 是对 MCP server 的包装。Claude Code 和 Codex 原生支持 MCP，无需通过 skill 包装；OpenClaw 需要通过 mcporter + skill 来调用 MCP 工具。

## Installation

二选一：要么用 CLI 自动安装（推荐），要么手动配置路径。

### Option A: CLI 自动安装（推荐）

使用 [vercel-labs/skills](https://github.com/vercel-labs/skills)：

```bash
# 安装全部 skill
npx skills add zkl2333/skills

# 安装指定 skill
npx skills add zkl2333/skills --skill caiyun-weather

# 安装到指定 agent
npx skills add zkl2333/skills -a claude-code -a codex

# 全局安装
npx skills add zkl2333/skills -g
```

### Option B: 手动安装/配置（OpenClaw）

将仓库中的 `skills/` 目录加入 OpenClaw 的 `skills.load.extraDirs`：

```json
{
  "skills": {
    "load": {
      "extraDirs": ["/path/to/skills/skills"]
    }
  }
}
```

### MCP Server 配置

如果你使用了 MCP wrapper 类 skill（见上方备注），还需要配置对应的 MCP server：

1. 复制示例配置：
   ```bash
   cp mcporter.example.json mcporter.json
   ```
2. 编辑 `mcporter.json`，替换占位符：
   - `YOUR_ZHIPU_API_KEY` → 你的[智谱开放平台](https://open.bigmodel.cn/) API Key（用于 web-reader / zread / web-search-prime / zai-mcp-server）
   - `YOUR_CAIYUN_API_TOKEN` → 你的[彩云天气](https://platform.caiyunapp.com/) API Token
   - `YOUR_AMAP_API_KEY` → 你的[高德开放平台](https://lbs.amap.com/) Web 服务 API Key
3. 删除不需要的 server 条目即可，不影响其他 skill。

> `mcporter.json` 已在 `.gitignore` 中，不会被提交。`mcporter.example.json` 是不含真实密钥的示例文件。

详细的 MCP server 配置说明（各 server 用途、参数、注意事项），参见 [mcporter.md](mcporter.md)。

## Skill 结构

```
my-skill/
├── SKILL.md          # 必需：frontmatter(name+description) + 指令
├── scripts/          # 可选：可执行脚本
├── references/       # 可选：参考文档
└── assets/           # 可选：模板、资源文件
```

## License

MIT License - 详见 [LICENSE.txt](LICENSE.txt)
