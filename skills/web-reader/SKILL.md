---
name: web-reader
description: "Read and extract readable content from a URL via the Zhipu CodePlan MCP web-reader server. Use when you need to fetch and parse a web page into clean text/markdown, especially when browser automation is unnecessary. Triggers: web-reader, webReader, read webpage, extract article, fetch url."
---

## Prerequisites (mcporter)

Add this server to your `mcporter.json` (merge under `mcpServers`):

```json
"web-reader": {
  "baseUrl": "https://open.bigmodel.cn/api/mcp/web_reader/mcp",
  "headers": {
    "Authorization": "Bearer YOUR_ZHIPU_API_KEY",
    "Accept": "application/json, text/event-stream"
  }
}
```

Based on the Zhipu CodePlan MCP server configuration, this skill provides functionality for reading web page content.

**Usage:**
Call the `webReader` tool with the `url` parameter.

**Parameters:**
- `url` (string, required): The URL of the website to read.

**Example:**
```bash
mcporter call web-reader.webReader url="https://github.com/openclaw/openclaw"
```
