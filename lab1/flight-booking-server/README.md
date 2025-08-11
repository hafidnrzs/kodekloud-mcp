# Flight Booking MCP Server

Windows quick-start for running and wiring this MCP server locally.
This project is from the KodeKloud tutorial [MCP Tutorial: Build Your First MCP Server and Client from Scratch (Free Labs)](https://www.youtube.com/watch?v=RhTiAOGwbYE&t=79s)

## Prerequisites

- Windows with PowerShell
- Python 3.12+
- One of:
	- uv (recommended), or
	- a local virtualenv with pip

## Option A — Use uv (recommended)

The repo already includes `.roo/mcp.json` configured to start this server via uv:

```json
{
	"mcpServers": {
		"flight-booking": {
			"command": "uv",
			"args": ["run", "python", "server.py"],
			"cwd": "D:/aegislabs/kodekloud-mcp/lab1/flight-booking-server"
		}
	}
}
```

Notes:
- Change the `cwd` to your local absolute path.
- uv will detect `pyproject.toml` and resolve/install `mcp[cli]` automatically in an isolated env.
- Your MCP-capable client/extension should auto-start the server from this file.

## Option B — Use a local virtualenv

```powershell
cd .\flight-booking-server
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -U pip
python -m pip install "mcp[cli]>=1.12.4"

# Optional: quick import check
python -c "import mcp.server.fastmcp; print('mcp OK')"

# Run the server (press Ctrl+C to stop)
python .\server.py
```

If you prefer the virtualenv route for your MCP client, adjust `.roo/mcp.json` to:

```
{
	"mcpServers": {
		"flight-booking": {
			"command": ".\\flight-booking-server\\.venv\\Scripts\\python.exe",
			"args": ["server.py"],
			"cwd": "D:/aegislabs/kodekloud-mcp/lab1/flight-booking-server"
		}
	}
}
```

## What this server provides

- Tools: `search_flights`, `create_booking`
- Resources: `file://airports`, `file://airlines`
- Prompts: `find_best_flight`, `handle_disruption`

Your MCP client should list these once connected to "Flight Booking Server".

## Example Prompt

```
Search for flights from LAX to JFK using the flight-booking server
```

```
Get airport information using the flight-booking MCP server
```

```
Book flight FL123 for John Doe using flight-booking server
```

## Troubleshooting

- Activation script blocked: run PowerShell as admin, or temporarily allow scripts in this session:
	```powershell
	Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
	```
- Missing uv: install it from https://docs.astral.sh/uv/ or use Option B (virtualenv).
- Dependency issues: re-run either `uv run` to let uv resolve, or `pip install "mcp[cli]>=1.12.4"` in your venv.

