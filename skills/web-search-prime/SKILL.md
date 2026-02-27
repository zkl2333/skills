Based on the Zhipu CodePlan MCP server configuration, this skill provides functionality for web searching.

**Usage:**
Call the `webSearchPrime` tool with the `search_query` parameter.

**Parameters:**
- `search_query` (string, required): The content to search for. Recommended to keep under 70 characters.

**Example:**
```bash
mcporter call web-search-prime.webSearchPrime search_query="OpenClaw 最新进展"
```
