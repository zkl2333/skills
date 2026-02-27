Based on the Zhipu CodePlan MCP server configuration, this skill provides functionality for visual understanding tasks. This server offers several specialized sub-tools:

- `ui_to_artifact`: Converts UI screenshots into code, prompts, specifications, or descriptions.
- `extract_text_from_screenshot`: Performs OCR to extract text content from screenshots (supports local file paths or remote URLs).
- `diagnose_error_screenshot`: Analyzes error screenshots to provide diagnostic information.
- `understand_technical_diagram`: Interprets technical diagrams such as architecture diagrams, flowcharts, etc.
- `analyze_data_visualization`: Analyzes data charts and visualizations.

**Common Parameters (for most sub-tools):**
- `image_source` (string, required): The local file path or remote URL of the image.
- `prompt` (string, required): Describes what you want to extract or analyze from the image.

**Example (using `extract_text_from_screenshot`):**
```bash
mcporter call zai-mcp-server.extract_text_from_screenshot image_source="https://avatars.githubusercontent.com/u/252820863" prompt="图中有什么文字或内容？"
```
