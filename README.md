# My First MCP Server (RecipeBox)

A custom **MCP (Model Context Protocol) server** built with **Python** and **FastMCP**, connected to **Claude Desktop**. It lets Claude use my own tools and data to answer questions.

> Built as a learning project: my first time giving Claude tools that I wrote myself.

## What it does

- Exposes RecipeBox tools that Claude can call from a chat
- Runs locally over stdio, so Claude Desktop launches it automatically
- Defined in `recipebox_fastmcp.py`

<!-- Add a short list of your actual tools here, for example:
- `search_recipes`: find recipes by ingredient
- `add_recipe`: save a new recipe
-->

## Tech stack

- Python 3.12
- [FastMCP](https://gofastmcp.com)
- [uv](https://docs.astral.sh/uv/) for dependency management

## Project structure

```
.
├── recipebox_fastmcp.py   # The MCP server (tools live here)
├── main.py
├── pyproject.toml         # Project dependencies
├── uv.lock                # Locked versions
└── .python-version
```

## Setup

**1. Install uv**

```powershell
winget install --id astral-sh.uv
```

**2. Clone the repo and install dependencies**

```bash
git clone https://github.com/singhnupurvns/My_first_Mcp_Server.git
cd My_first_Mcp_Server
uv sync
```

**3. Test the server**

```bash
uv run fastmcp run recipebox_fastmcp.py
```

If you see the FastMCP banner, the server started correctly.

## Connect to Claude Desktop

1. Open Claude Desktop and go to **Settings → Developer → Edit Config**.
2. Add this inside `mcpServers` (change the paths to match your machine):

```json
{
  "mcpServers": {
    "RecipeBox": {
      "command": "C:\\Users\\YOUR_NAME\\.local\\bin\\uv.exe",
      "args": [
        "run",
        "--with",
        "fastmcp",
        "fastmcp",
        "run",
        "C:\\Users\\YOUR_NAME\\path\\to\\My_first_Mcp_Server\\recipebox_fastmcp.py"
      ],
      "env": {}
    }
  }
}
```

3. Fully quit Claude Desktop (from the system tray) and reopen it.
4. Go to **Settings → Developer**. The server should show **Running**.
5. Start a new chat and ask Claude a question that uses your tools.

Find your uv path with `where.exe uv`.

## Troubleshooting

- **Server not showing up:** check `claude_desktop_config.json` for missing commas or single backslashes in paths.
- **Server shows an error:** open the log at `%APPDATA%\Claude\logs\mcp-server-<name>.log`.
- **Connection breaks:** don't use `print()` in the server code. Stdout is the protocol channel for stdio servers.

## What I learned

- How MCP gives AI models a standard way to use real tools and data
- Registering a local server in Claude Desktop
- Debugging paths, virtual environments, and JSON config
- Publishing a project to GitHub

## License

MIT (add a `LICENSE` file if you want to share it publicly)
