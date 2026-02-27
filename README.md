# Skills

个人 Agent Skills 集合，兼容 Claude Code、Codex、OpenClaw 等支持 AgentSkill 规范的平台。

## Skills

| Skill | 说明 | 备注 |
|-------|------|------|
| `bark` | Bark(day.app) 推送通知 | |
| `caiyun-weather` | 彩云天气（实时/预报/预警） | MCP wrapper ¹ |
| `check-updates` | OpenClaw 版本更新检查 | OpenClaw 专用 |
| `codex-cli` | OpenAI Codex CLI 工作流封装 | |
| `commit` | Conventional Commits/emoji 风格提交 | |
| `first-principles-decomposer` | 第一性原理拆解框架 | |
| `web-reader` | 网页正文提取 | MCP wrapper ¹ |
| `web-search-prime` | Web Search | MCP wrapper ¹ |
| `zai-mcp-server` | 视觉/OCR/UI→artifact | MCP wrapper ¹ |
| `zread` | GitHub 仓库文档检索 | MCP wrapper ¹ |

> ¹ **MCP wrapper**：这些 skill 是对 MCP server 的包装。Claude Code 和 Codex 原生支持 MCP，无需通过 skill 包装；OpenClaw 需要通过 mcporter + skill 来调用 MCP 工具。

## Installation

### Claude Code

```bash
# 克隆仓库
git clone https://github.com/zkl2333/skills.git

# 在 Claude Code 设置中添加 skills/ 路径
```

### OpenClaw

将 `skills/` 目录加入 `skills.load.extraDirs`：

```json
{
  "skills": {
    "load": {
      "extraDirs": ["/path/to/skills/skills"]
    }
  }
}
```

MCP wrapper 类 skill 还需在 `mcporter.json` 中配置对应的 MCP server。

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
