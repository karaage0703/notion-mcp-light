# NotionMCP Light

NotionMCP Light is a Model Context Protocol (MCP) server that synchronizes Markdown files with Notion pages using the Notion API.

## Overview

This project was developed to address the inefficiencies of Notion's official Model Context Protocol (MCP) server, which reads and writes Markdown on a block-by-block basis, consuming LLM tokens. It provides an unofficial MCP server that can directly synchronize Markdown files with Notion pages/databases via API without consuming tokens.

## Features

- **Markdown → Notion**
  - Recognizes H1 as the page title
  - Creates Markdown content as a Notion page or database page
  - Database ID can be specified
  - Direct use of Notion API (no tokens consumed)

- **Notion → Markdown**
  - Converts specified pages or database pages to Markdown format
  - Outputs title as H1
  - Converts block structure to Markdown
  - Saves to file

- **MCP Server Support**
  - Complies with Model Context Protocol (MCP)
  - Provides endpoints callable from AI tools like Cursor and Cline
  - Operates on JSON-RPC over stdio basis

## Installation

### Installing Dependencies

```bash
# Install uv first if not already installed
# pip install uv

# Install dependencies
uv sync
```

### Setting Up Notion API Token

1. Create an account at [Notion Developers](https://developers.notion.com/) and obtain an API token.
2. Set it as an environment variable or create a `.env` file with the token.

```bash
# Example .env file
NOTION_TOKEN=your_notion_api_token
```

## Usage

### Starting the MCP Server

#### Using uv (Recommended)

```bash
uv run python -m src.main
```

Or, to specify the token directly:

```bash
uv run python -m src.main --token your_notion_api_token
```

#### Using Regular Python

```bash
python -m src.main
```

Or, to specify the token directly:

```bash
python -m src.main --token your_notion_api_token
```

### Configuration for Cline/Cursor

To use NotionMCP Light with AI coding tools like Cline/Cursor/Claude Code, add the following configuration to the JSON file that sets up the MCP server settings for each tool:

```json
{
  "mcpServers": {
    "notion-mcp-light": {
      "command": "uv",
      "args": [
        "run",
        "--directory",
        "/path/to/notion-mcp-light",
        "-m",
        "src.main"
      ],
      "env": {
        "NOTION_TOKEN": "your_notion_api_token"
      }
    }
  }
}
```

Replace `/path/to/notion-mcp-light` with the NotionMCP Light installation directory and `your_notion_api_token` with your Notion API token.

## Using MCP Tools

NotionMCP Light provides the following MCP tools:

### uploadMarkdown

Uploads a Markdown file and creates it as a Notion page.

```json
{
  "jsonrpc": "2.0",
  "method": "uploadMarkdown",
  "params": {
    "filepath": "path/to/markdown.md",
    "database_id": "optional_database_id",
    "page_id": "optional_parent_page_id"
  },
  "id": 1
}
```

### downloadMarkdown

Downloads a Notion page and saves it as a Markdown file.

```json
{
  "jsonrpc": "2.0",
  "method": "downloadMarkdown",
  "params": {
    "page_id": "notion_page_id",
    "output_path": "path/to/output.md"
  },
  "id": 2
}
```

## License

This project is released under the MIT License. For details, please refer to the [LICENSE](LICENSE) file.
