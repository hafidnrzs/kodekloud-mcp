# Using an MCP Server

Ini adalah lab 1 dari KodeKloud tutorial [MCP Tutorial: Build Your First MCP Server and Client from Scratch (Free Labs)](https://www.youtube.com/watch?v=RhTiAOGwbYE&t=79s) bagian **Using an MCP Server**.

## Prerequisites

- Windows dengan PowerShell
- Python 3.12+
- [uv](https://docs.astral.sh/uv/)
- Roo Code extension pada VSCode

## Installation

Repo sudah menyertakan `.roo/mcp.json` yang dikonfigurasi untuk menjalankan MCP server via uv:

```json
{
	"mcpServers": {
		"flight-booking": {
			"command": "uv",
			"args": ["run", "python", "server.py"],
			"cwd": "D:/aegislabs/kodekloud-mcp/flight-booking-server"
		}
	}
}
```

Catatan:
- Ubah `cwd` ke path local Anda.
- uv akan mendeteksi `pyproject.toml` dan menginstall `mcp[cli]` secara otomatis pada env yang terisolasi.
- MCP Client (Roo Code extension) akan memulai server secara otomatis dari berkas ini.

## What this server provides

- Tools: `search_flights`, `create_booking`
- Resources: `file://airports`, `file://airlines`
- Prompts: `find_best_flight`, `handle_disruption`

MCP Client Anda (Roo Code) seharusnya terdapat itu setelah terhubung ke "Flight Booking Server".

## Example Prompt

Contoh prompt yang bisa dicoba untuk mengetes MCP Server melalui chat pada MCP Client.

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

- Missing uv: install dari https://docs.astral.sh/uv/
- Dependency issues: jalankan ulang `uv run` supaya uv bisa resolve, atau `pip install "mcp[cli]>=1.12.4"` pada venv Anda.

