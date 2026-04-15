# Building MCP Servers

An MCP (Model Context Protocol) server is a running process that exposes typed tool functions to Claude Code. Instead of Claude writing bash commands to interact with a system, it calls structured tools with defined parameters and return values.

## When to build one

Build an MCP server when you have repeated structured interactions with a local system. Good candidates: database queries, API wrappers, file processing pipelines, monitoring dashboards. If you find yourself writing the same bash patterns across sessions, an MCP server turns them into clean tool calls with typed inputs and outputs.

## Basic server (Python with FastMCP)

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("my-tools")

@mcp.tool()
def query_database(sql: str) -> str:
    """Run a read-only SQL query against the local database."""
    import sqlite3
    conn = sqlite3.connect("data.db")
    result = conn.execute(sql).fetchall()
    conn.close()
    return str(result)

@mcp.tool()
def check_service_health(service: str) -> dict:
    """Check if a service is running and return its status."""
    import subprocess
    result = subprocess.run(["systemctl", "is-active", service], capture_output=True, text=True)
    return {"service": service, "status": result.stdout.strip()}

if __name__ == "__main__":
    mcp.run()
```

Install FastMCP:

```bash
pip install mcp
```

## Registering with Claude Code

Add to `.claude/settings.json`:

```json
{
  "mcpServers": {
    "my-tools": {
      "command": "python3 /path/to/server.py",
      "description": "Local tools for database queries and service monitoring"
    }
  }
}
```

For pip-installed packages that provide a CLI entry point:

```json
{
  "mcpServers": {
    "my-tools": {
      "command": "my-tools-mcp",
      "description": "Description of what the tools do"
    }
  }
}
```

## How it differs from slash commands

Slash commands are markdown prompt templates. Claude reads the instructions and follows them. No running process, no typed interface. Good for workflows and procedures.

MCP servers are running processes with typed tool definitions. Claude calls them like functions. Good for structured data access where you want reliable inputs, outputs, and error handling.

Use a slash command when the workflow is a sequence of steps Claude should follow. Use an MCP server when you need Claude to interact with a system through a stable typed API.

## Tips

Keep tools focused. One tool per action, not one tool with a mode parameter. Claude selects from a list of tools better than it selects from a list of mode strings inside one tool.

Include docstrings on every tool. Claude reads them to decide when to call each tool and how to format arguments.

Return structured data (dicts, lists) rather than formatted strings. Claude reasons about structured data more reliably than it parses prose output.
