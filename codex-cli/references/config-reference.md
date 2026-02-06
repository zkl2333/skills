# Codex CLI Configuration Reference

Config file locations:
- User-level: `~/.codex/config.toml`
- Project-level: `.codex/config.toml` (only loads when project is trusted)

Override any key via CLI: `codex -c key=value` or `codex -c 'nested.key=value'`

## Core Model Settings

```toml
model = "gpt-5.3-codex"
model_provider = "openai"                    # Provider ID from model_providers table
model_context_window = 200000                # Token count
model_auto_compact_token_limit = 150000      # Auto-compaction threshold
model_reasoning_effort = "medium"            # minimal | low | medium | high | xhigh
model_reasoning_summary = "auto"             # auto | concise | detailed | none
model_personality = "none"                   # none | friendly | pragmatic (experimental)
model_verbosity = "medium"                   # low | medium | high (GPT-5 Responses API)
review_model = "gpt-5.3-codex"              # Override for /review
```

## Security & Sandbox

```toml
sandbox_mode = "workspace-write"             # read-only | workspace-write | danger-full-access
approval_policy = "untrusted"                # untrusted | on-failure | on-request | never

[sandbox_workspace_write]
writable_roots = ["/tmp/my-project"]         # Additional writable dirs
network_access = false                       # Outbound network access
exclude_slash_tmp = false
exclude_tmpdir_env_var = false
```

## Authentication

```toml
forced_login_method = "chatgpt"              # chatgpt | api
forced_chatgpt_workspace_id = "uuid-here"    # Restrict to specific workspace
cli_auth_credentials_store = "auto"          # file | keyring | auto
chatgpt_base_url = "https://chatgpt.com"     # Override login URL
```

## Shell Environment

```toml
[shell_environment_policy]
inherit = "all"                              # all | core | none
experimental_use_profile = false             # Use user shell profile
ignore_default_excludes = false              # Keep KEY/SECRET/TOKEN vars

[shell_environment_policy.set]
MY_VAR = "value"                             # Set specific env vars

# include_only = ["PATH", "HOME"]            # Whitelist patterns
# exclude = ["*SECRET*", "*TOKEN*"]           # Glob exclusion patterns
```

## Web Search

```toml
web_search = "cached"                        # disabled | cached | live
# cached: OpenAI-maintained index
# live: fetch current web data (default with --yolo or full-access sandbox)
```

## Feature Flags

```toml
[features]
shell_tool = true                            # Default shell tool (stable)
unified_exec = false                         # PTY-backed exec tool (beta)
shell_snapshot = false                       # Snapshot environment (beta)
apply_patch_freeform = false                 # Freeform apply_patch (experimental)
exec_policy = true                           # Rules checks (experimental)
request_rule = true                          # Smart approvals (stable)
remote_compaction = true                     # Remote compaction (experimental)
remote_models = false                        # Refresh model list (experimental)
experimental_windows_sandbox = false         # Windows restricted-token sandbox
powershell_utf8 = true                       # Force UTF-8 output
child_agents_md = false                      # Append AGENTS.md guidance
```

## History

```toml
[history]
persistence = "save-all"                     # save-all | none
max_bytes = 10485760                         # Cap history file size
```

## MCP Servers

```toml
mcp_oauth_callback_port = 8080              # Fixed OAuth callback port
mcp_oauth_credentials_store = "auto"         # auto | file | keyring

[mcp_servers.my-stdio-server]
enabled = true
command = "npx"
args = ["-y", "@my/mcp-server"]
cwd = "/path/to/dir"
startup_timeout_sec = 10
tool_timeout_sec = 60
enabled_tools = ["tool1", "tool2"]           # Allow list
disabled_tools = ["tool3"]                   # Deny list

[mcp_servers.my-stdio-server.env]
API_KEY = "key"

[mcp_servers.my-http-server]
enabled = true
url = "https://my-server.example.com/mcp"
bearer_token_env_var = "MY_TOKEN"
startup_timeout_sec = 10

[mcp_servers.my-http-server.http_headers]
X-Custom = "value"
```

## Custom Model Providers

```toml
[model_providers.azure]
name = "Azure OpenAI"
base_url = "https://my-resource.openai.azure.com/openai"
env_key = "AZURE_OPENAI_API_KEY"
wire_api = "chat"                            # chat | responses
request_max_retries = 4
stream_idle_timeout_ms = 300000
stream_max_retries = 5

[model_providers.ollama]
name = "Ollama"
base_url = "http://localhost:11434/v1"
env_key = "OLLAMA_API_KEY"
wire_api = "chat"
```

## Profiles

```toml
profile = "default"                          # Default profile at startup

[profiles.fast]
model = "gpt-4o-mini"
model_reasoning_effort = "low"

[profiles.review]
model = "gpt-5.3-codex"
model_reasoning_effort = "high"

[profiles.local]
oss_provider = "ollama"
model = "codellama"
web_search = "disabled"
```

## Project Trust

```toml
[projects."/path/to/project"]
trust_level = "trusted"                      # trusted | untrusted
```

## UI & Notifications

```toml
file_opener = "vscode"                       # vscode | vscode-insiders | windsurf | cursor | none

[tui]
animations = true
alternate_screen = "auto"                    # auto | always | never
notifications = true                         # or array: ["completion", "error"]
notification_method = "auto"                 # auto | osc9 | bel
show_tooltips = true

disable_paste_burst = false
```

## Project Settings

```toml
project_root_markers = [".git", "package.json"]
project_doc_max_bytes = 65536                # Max bytes from AGENTS.md
project_doc_fallback_filenames = ["CODEX.md"]
model_instructions_file = "./instructions.md" # Replace built-in instructions
developer_instructions = "Always use TypeScript"
```

## Skills

```toml
[[skills.config]]
path = "/path/to/skill"
enabled = true
```

## OpenTelemetry

```toml
[otel]
environment = "dev"
exporter = "none"                            # none | otlp-http | otlp-grpc
log_user_prompt = false

[otel.exporter.otlp-http]
endpoint = "https://otel.example.com/v1/logs"
protocol = "json"                            # binary | json

[otel.trace_exporter.otlp-http]
endpoint = "https://otel.example.com/v1/traces"
```

## Feedback & Updates

```toml
[feedback]
enabled = true

check_for_update_on_startup = true
```

## Notifications (External)

```toml
notify = ["notify-send", "Codex"]            # Command for notifications (receives JSON)
```

## Admin Requirements (requirements.toml)

Enforced constraints users cannot override:

```toml
allowed_approval_policies = ["untrusted", "on-failure"]
allowed_sandbox_modes = ["read-only", "workspace-write"]

[mcp_servers.allowed-server]
[mcp_servers.allowed-server.identity]
command = "npx @approved/mcp-server"

[[rules.prefix_rules]]
pattern = [{ token = "rm" }, { token = "-rf" }]
decision = "forbidden"
justification = "Recursive deletion is not allowed"
```
