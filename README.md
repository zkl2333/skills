# Skills

个人 Claude Code skills 集合

## About

这是一个用于 Claude Code 的个人技能集合，包含了常用的开发工作流增强工具。每个技能都是自包含的文件夹，包含指令、脚本和资源，Claude 会根据需要动态加载。

## Skills

当前目录：`skills/`

- `bark` — Bark(day.app) 推送通知
- `caiyun-weather` — 彩云天气（通过 MCP 获取实时/预报/预警）
- `check-updates` — OpenClaw 更新检查（三步：本地/npm/GitHub）
- `codex-cli` — OpenAI Codex CLI 工作流封装
- `commit` — Conventional Commits/emoji 风格提交
- `first-principles-decomposer` — 第一性原理拆解框架
- `web-reader` — MCP 网页正文提取
- `web-search-prime` — MCP Web Search
- `zai-mcp-server` — MCP 视觉/OCR/UI→artifact
- `zread` — MCP GitHub 仓库文档检索

每个 skill 都是一个文件夹（至少包含 `SKILL.md`）。

## Installation

### Claude Code Plugin Marketplace (推荐)

将此仓库注册为 Claude Code 插件市场：

```bash
# 在 Claude Code 中运行
/plugin marketplace add zkl2333/skills

# 安装特定插件
/plugin install commit@zkl2333-skills
/plugin install codex-cli@zkl2333-skills
```

安装后，直接在对话中提及技能即可使用：

```
"Use the commit skill to commit these changes"
"Use codex-cli to review this PR"
```

### 手动安装

克隆仓库并将 `skills/` 目录添加到 Claude Code 配置：

```bash
git clone https://github.com/zkl2333/skills.git
cd skills
```

然后在 Claude Code 设置中添加 `skills/` 路径到 skills 配置。

### 使用 npx add-skill

```bash
npx add-skill zkl2333/skills
```

## Usage

技能在使用时会自动加载。只需在对话中提及相关功能：

```
"帮我提交这些更改"                    # 自动触发 commit skill
"用 codex 审查这个分支"                # 自动触发 codex-cli skill
"生成符合规范的 commit message"        # 自动触发 commit skill
```

## Development

基于 [Anthropic skill-creator](https://github.com/anthropics/skills) 指南创建。

### 创建新 Skill

```bash
# 使用 skill-creator 初始化
python ~/.claude/skills/skill-creator/scripts/init_skill.py my-skill --path ./skills

# 编辑 SKILL.md
# 添加 scripts/、references/、assets/（按需）

# 打包（用于分发）
python ~/.claude/skills/skill-creator/scripts/package_skill.py ./skills/my-skill
```

### Skill 结构

```
my-skill/
├── SKILL.md          # 必需：包含元数据和指令
├── scripts/          # 可选：可执行脚本
├── references/       # 可选：参考文档
└── assets/           # 可选：模板、资源文件
```

### 更新 marketplace.json

添加新技能到 `.claude-plugin/marketplace.json`：

```json
{
  "name": "my-skill",
  "description": "Brief description",
  "source": "./",
  "skills": ["./skills/my-skill"]
}
```

## License

MIT License - 详见 [LICENSE.txt](LICENSE.txt)

## Credits

基于 [Anthropic Agent Skills](https://github.com/anthropics/skills) 标准构建。
