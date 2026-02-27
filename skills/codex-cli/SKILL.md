# Codex CLI

OpenAI's terminal coding agent. Reads, edits, and runs code locally with configurable sandbox and approval controls. Built in Rust, open source.

## Quick Reference

\`\`\`bash
# Interactive session
codex "your prompt here"

# Non-interactive (scripting/CI)
codex exec "your prompt here"
codex exec --json "prompt"           # JSONL output
codex exec -o result.txt "prompt"    # Save last message

# Code review
codex review                         # Review current branch vs main
codex review --uncommitted           # Review all uncommitted changes
codex review --base develop          # Review against specific branch
codex review --commit abc123         # Review a specific commit

# Session management
codex resume                         # Pick from recent sessions
codex resume --last                  # Continue most recent
codex fork --last                    # Fork most recent into new thread

# Cloud tasks (experimental)
codex cloud exec --env ENV_ID "task"
codex cloud list
codex cloud apply TASK_ID
codex cloud diff TASK_ID
\`\`\`

## Key Flags

| Flag | Short | Purpose |
|------|-------|---------|
| \`--model\` | \`-m\` | Select model (e.g. \`gpt-5.3-codex\`, \`o3\`) |
| \`--sandbox\` | \`-s\` | \`read-only\` / \`workspace-write\` / \`danger-full-access\` |
| \`--ask-for-approval\` | \`-a\` | \`untrusted\` / \`on-failure\` / \`on-request\` / \`never\` |
| \`--full-auto\` | | Shortcut: \`-a on-request -s workspace-write\` |
| \`--image\` | \`-i\` | Attach image(s) to prompt |
| \`--search\` | | Enable live web search |
| \`--cd\` | \`-C\` | Set the working directory for the \`codex\` command. This directory will be used as the base for operations by subcommands like \`exec\`, \`review\`, \`cloud exec\`, etc. This flag should be passed directly to the \`codex\` command. For example: \`codex --cd /path/to/project review\`. |
| \`--add-dir\` | | Grant additional writable directories |
| \`--config\` | \`-c\` | Override config value (e.g. \`-c model="o3"\`) |
| \`--profile\` | \`-p\` | Load named config profile |
| \`--oss\` | | Use local model provider (LM Studio / Ollama) |
| \`--json\` | | (exec only) JSONL output |
| \`--output-last-message\` | \`-o\` | (exec only) Write last message to file |
| \`--yolo\` | | Bypass all approvals and sandbox (DANGEROUS) |

## Approval Modes

- **untrusted** (default): Only trusted commands (ls, cat, sed…) run without asking. Others prompt user.
- **on-failure**: Run all commands freely. Only prompt if a command fails.
- **on-request**: Model decides when to ask.
- **never**: Never prompt. Failures go straight back to model.

## Sandbox Modes

- **read-only**: No file writes, no network.
- **workspace-write**: Write only in workspace + specified dirs. Configurable network access.
- **danger-full-access**: Unrestricted. Use only in isolated environments.

## Slash Commands (Interactive)

| Command | Purpose |
|---------|---------|
| \`/model\` | Switch model mid-session |
| \`/permissions\` | Adjust approval/sandbox settings |
| \`/review\` | Run code review |
| \`/diff\` | Show git diff including untracked files |
| \`/compact\` | Summarize conversation to free tokens |
| \`/mention\` | Attach file to conversation |
| \`/new\` | Start new conversation in same session |
| \`/resume\` | Resume saved conversation |
| \`/fork\` | Fork current conversation |
| \`/init\` | Generate AGENTS.md scaffold |
| \`/mcp\` | List MCP tools |
| \`/status\` | Show session config and token usage |
| \`/ps\` | Show background terminals |
| \`/quit\` | Exit |

## Common Patterns

### CI/CD Automation

\`\`\`bash
# Run task non-interactively with JSON output
codex exec --json --full-auto "fix all lint errors" | process_results.sh

# Structured output with schema
codex exec --output-schema schema.json "analyze this codebase"
\`\`\`

### Code Review in CI

\`\`\`bash
codex review --base main --title "PR #42: Add auth"
\`\`\`

### Multi-Directory Projects

\`\`\`bash
codex --cd ./backend --add-dir ./shared "update the API"
\`\`\`

### Image-Driven Development

\`\`\`bash
codex -i mockup.png "implement this UI exactly"
\`\`\`

### Local OSS Models

\`\`\`bash
codex --oss --CPA ollama -m codellama "refactor this function"
\`\`\`

## Configuration

Config file: \`~/.codex/config.toml\` (user-level), \`.codex/config.toml\` (project-level).

For full configuration reference including all keys, MCP server setup, model providers, sandbox permissions, shell environment policies, and OpenTelemetry: see [references/config-reference.md](references/config-reference.md).

## MCP Server Management

\`\`\`bash
codex mcp list                       # List configured servers
codex mcp add <name>                 # Add MCP server
codex mcp remove <name>              # Remove MCP server
codex mcp login <name>               # Authenticate with server
codex mcp logout <name>              # Remove server credentials
\`\`\`

Configure in config.toml:

\`\`\`toml
[mcp_servers.my-server]
command = "npx"
args = ["-y", "@my/mcp-server"]
enabled = true
\`\`\`