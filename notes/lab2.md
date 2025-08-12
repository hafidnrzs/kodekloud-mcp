# Building an MCP Server

Ini adalah lab 2 dari KodeKloud tutorial [MCP Tutorial: Build Your First MCP Server and Client from Scratch (Free Labs)](https://www.youtube.com/watch?v=RhTiAOGwbYE&t=79s) bagian **Building an MCP Server**.

Fitur utama yang diekspos oleh MCP server:
1. Resources
2. Tools
3. Prompts

## UV Package Manager

Sebelumnya, disarankan untuk menggunakan uv package manager.

Mengapa?
- Dokumentasi MCP merekomendasikan UV
- 10-100x lebih cepat daripada pip untuk resolusi dependensi
- Menangani dependensi, lingkungan virtual, dan versi Python
- Standar modern: Menggunakan pyproject.toml (PEP 518) untuk konfigurasi

| Traditional                        | UV Equivalent   |
|------------------------------------|-----------------|
| pip install package                | uv add package  |
| python -m venv env                 | uv init project |
| pip install -r requirements.txt    | uv sync         |

```bash
uv init flight-booking-server
cd flight-booking-server
uv add "mcp[cli]"
```

Mengapa perintah tersebut?
- `uv init` membuat proyek Python dengan pyproject.toml
- `mcp[cli]` mencakup SDK MCP dan alat pengembangan (MCP Inspector)

## Test prompt for MCP server

* "Search for flights from LAX to JFK using the flight-booking server"
* "Get airport information using the flight-booking MCP server"
* "Book flight FL123 for John Doe using flight-booking server"

## 🚀 What you've learned in lab 2:

- Workflow pengembangan MCP official dengan uv
- FastMCP framework dari `mcp.server.fastmcp`
- Implementasi Resource, Tool, dan Prompt
- MCP Inspector untuk pengujian dan validasi server
- Integrasi server MCP dunia nyata dengan asisten AI

## 🔜 Next Steps:

- Eksplor HTTP transport: `mcp.run(transport="http")`
- Tambahkan penanganan kesalahan dan validasi input
- Integrasikan dengan API penerbangan yang sebenarnya