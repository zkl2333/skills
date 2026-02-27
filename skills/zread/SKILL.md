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
