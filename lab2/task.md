Three main features exposed by an MCP server:
1. Resources
2. Tools
3. Promps

## UV Package Manager

Why?
- MCP documentation recommends UV
- 10-100x faster than pip for dependency resolution
- Handle dependencies, virtual environments, and Python versions
- Modern standards: Uses pyproject.toml (PEP 518) for config

| Traditional                        | UV Equivalent   |
|------------------------------------|-----------------|
| pip install package                | uv add package  |
| python -m venv env                 | uv init project |
| pip install -r requirements.txt    | uv sync         |

```bash
cd lab2
uv init flight-booking-server
cd flight-booking-server
uv add "mcp[cli]"
```

Why these commands?
- `uv init` creates a proper Python project with pyproject.toml
- `mcp[cli]` includes both MCP SDK and development tools (MCP Inspector)

## Test prompt for MCP server

* "Search for flights from LAX to JFK using the flight-booking server"
* "Get airport information using the flight-booking MCP server"
* "Book flight FL123 for John Doe using flight-booking server"

## 🚀 What you've learned:

- Official MCP development workflow with uv
- FastMCP framework from `mcp.server.fastmcp`
- Resource, Tool, and Prompt implementation patterns
- MCP Inspector for server testing and validation
- Real-world MCP server integration with AI assistants

## 🔜 Next Steps:

- Explore HTTP transport: `mcp.run(transport="http")`
- Add error handling and input validation
- Integrate with real flight APIs