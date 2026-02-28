# mcporter 配置说明

本文档说明 `mcporter.json` 的结构、字段含义，以及本仓库各 skill 依赖的 MCP server 配置。

## 1) 文件位置与安全建议

- 示例模板：仓库根目录 `mcporter.example.json`
- 本地配置：复制为 `mcporter.json`（同目录），已在 `.gitignore` 中
- **不要提交真实 key**：仅提交 example 模板

## 2) 顶层结构

```json
{
  "mcpServers": {
    "server-name": {
      "baseUrl": "https://...",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY"
      }
    },
    "local-server": {
      "command": "npx",
      "args": ["-y", "some-mcp-server"],
      "env": {
        "SOME_API_KEY": "YOUR_KEY"
      }
    }
  },
  "imports": []
}
```

### 字段说明

| 字段 | 说明 |
|------|------|
| `mcpServers` | MCP server 配置集合，key 是 server 名称 |
| `mcpServers.<name>.baseUrl` | 远程 MCP 服务地址（HTTP/SSE） |
| `mcpServers.<name>.headers` | 远程服务请求头，通常放 `Authorization` |
| `mcpServers.<name>.command` | 本地启动 MCP server 的命令（如 `npx`/`uvx`） |
| `mcpServers.<name>.args` | 命令参数数组 |
| `mcpServers.<name>.env` | 该 server 启动时注入的环境变量（敏感 key） |
| `imports` | 可选的外部配置导入列表 |

**两种 server 类型：**

- **远程 (HTTP/SSE)**：使用 `baseUrl` + `headers`，适用于云端 MCP 服务
- **本地 (stdio)**：使用 `command` + `args` + `env`，在本机启动 MCP server 进程

## 3) 本仓库 skill 与 MCP server 对应关系

| Skill | MCP Server Key | 类型 | 所需 API Key |
|-------|---------------|------|-------------|
| `web-reader` | `web-reader` | 远程 | 智谱 API Key |
| `web-search-prime` | `web-search-prime` | 远程 | 智谱 API Key |
| `zread` | `zread` | 远程 | 智谱 API Key |
| `zai-mcp-server` | `zai-mcp-server` | 本地 | 智谱 API Key |
| `caiyun-weather` | `caiyun-weather` | 本地 | 彩云天气 Token |
| `amap-maps` | `amap-maps` | 本地 | 高德 API Key |

以下 skill **不依赖** MCP server 配置：`bark`、`check-updates`、`codex-cli`、`commit`

## 4) 各 Server 详细配置

### 智谱系（web-reader / zread / web-search-prime）

远程 HTTP/SSE 服务，共用同一个智谱 API Key：

```json
"web-reader": {
  "baseUrl": "https://open.bigmodel.cn/api/mcp/web_reader/mcp",
  "headers": {
    "Authorization": "Bearer YOUR_ZHIPU_API_KEY",
    "Accept": "application/json, text/event-stream"
  }
}
```

`zread` 和 `web-search-prime` 结构相同，替换 `baseUrl` 即可：

- `zread`：`https://open.bigmodel.cn/api/mcp/zread/mcp`
- `web-search-prime`：`https://open.bigmodel.cn/api/mcp/web_search_prime/mcp`

> 获取 Key：[智谱开放平台](https://open.bigmodel.cn/)

### zai-mcp-server（视觉/OCR）

本地 stdio 启动，同样使用智谱 API Key：

```json
"zai-mcp-server": {
  "command": "npx",
  "args": ["-y", "@z_ai/mcp-server"],
  "env": {
    "Z_AI_API_KEY": "YOUR_ZHIPU_API_KEY",
    "Z_AI_MODE": "ZHIPU"
  }
}
```

### caiyun-weather（彩云天气）

```json
"caiyun-weather": {
  "command": "uvx",
  "args": ["mcp-caiyun-weather"],
  "env": {
    "CAIYUN_WEATHER_API_TOKEN": "YOUR_CAIYUN_API_TOKEN"
  }
}
```

> 获取 Token：[彩云天气开放平台](https://platform.caiyunapp.com/)

### amap-maps（高德地图）

```json
"amap-maps": {
  "command": "npx",
  "args": ["-y", "@amap/amap-maps-mcp-server"],
  "env": {
    "AMAP_MAPS_API_KEY": "YOUR_AMAP_API_KEY"
  }
}
```

> 获取 Key：[高德开放平台](https://lbs.amap.com/)（Web 服务 API Key）

## 5) 快速开始

```bash
# 1. 复制示例为本地配置
cp mcporter.example.json mcporter.json

# 2. 替换占位符为真实 key
#    YOUR_ZHIPU_API_KEY    → 智谱 API Key
#    YOUR_CAIYUN_API_TOKEN → 彩云天气 Token
#    YOUR_AMAP_API_KEY     → 高德 API Key

# 3. 删除不需要的 server 条目（不影响其他 skill）
```

## 6) 其他 MCP 客户端

本仓库的 MCP server 配置同样适用于其他支持 MCP 的客户端（Claude Code、Codex、Cursor 等）。配置内容（server 地址、命令、环境变量）与上述相同，只需按各客户端的格式要求填写。

常见格式对照：

| 客户端 | 配置文件 | 格式 |
|--------|---------|------|
| OpenClaw (mcporter) | `mcporter.json` | 本文档格式 |
| Claude Code | `.mcp.json` | `{ "mcpServers": { ... } }` |
| Codex | `codex.json` / CLI flags | 参考各版本文档 |
| Cursor | Settings → MCP | GUI 配置 |

> `mcpServers` 内部结构（`baseUrl`/`command`/`args`/`env`）在主流客户端间基本通用。

## 7) 参考

- 示例文件：[`mcporter.example.json`](./mcporter.example.json)
- Skill 列表：[`README.md`](./README.md)
