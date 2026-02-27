---
name: web-search-prime
description: "Search the web via the Zhipu CodePlan MCP web-search-prime server. Use when you need fast web discovery (titles/URLs/snippets) without a browser. Triggers: web-search-prime, webSearchPrime, search web, web search prime."
---

Based on the Zhipu CodePlan MCP server configuration, this skill provides functionality for web searching.

**Usage:**
Call the `webSearchPrime` tool with the `search_query` parameter.

**Parameters:**
- `search_query` (string, required): The content to search for. Recommended to keep under 70 characters.

**Example:**
```bash
mcporter call web-search-prime.webSearchPrime search_query="OpenClaw 最新进展"
```
