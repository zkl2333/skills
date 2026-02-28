---
name: zread
description: "Search and read GitHub repository docs via the Zhipu CodePlan MCP zread server. Use when you need quick repo structure lookup or documentation search without cloning. Triggers: zread, search_doc, repo structure, read_file, GitHub docs search."
---

## Prerequisites (mcporter)

Add this server to your `mcporter.json` (merge under `mcpServers`):

```json
"zread": {
  "baseUrl": "https://open.bigmodel.cn/api/mcp/zread/mcp",
  "headers": {
    "Authorization": "Bearer YOUR_ZHIPU_API_KEY",
    "Accept": "application/json, text/event-stream"
  }
}
```

Based on the Zhipu CodePlan MCP server configuration, this skill provides functionality for searching GitHub repositories (documents, issues, commits).

**Usage:**
Call the `zread` tool with the `search_doc` function, providing the `repo_name` and `query` parameters.

**Parameters:**
- `repo_name` (string, required): The GitHub repository name in the format `owner/repo`.
- `query` (string, required): The search term or question.

**Example:**
```bash
mcporter call zread.search_doc repo_name="openclaw/openclaw" query="documentation"
```
