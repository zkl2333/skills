---
name: web-reader
description: "Read and extract readable content from a URL via the Zhipu CodePlan MCP web-reader server. Use when you need to fetch and parse a web page into clean text/markdown, especially when browser automation is unnecessary. Triggers: web-reader, webReader, read webpage, extract article, fetch url."
---

Based on the Zhipu CodePlan MCP server configuration, this skill provides functionality for reading web page content.

**Usage:**
Call the `webReader` tool with the `url` parameter.

**Parameters:**
- `url` (string, required): The URL of the website to read.

**Example:**
```bash
mcporter call web-reader.webReader url="https://github.com/openclaw/openclaw"
```
