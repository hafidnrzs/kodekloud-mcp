Before we can test MCP clients, we need to start the flight booking server that our clients will connect to.
1. Open a terminal and navigate to the server directory
2. Start the MCP server using the MCP CLI with streamable-http transport
3. Verify the server is running on port 8000
4. Leave this terminal open - the server must keep running

Commands to run:
```bash
cd ../lab2/flight-booking-server
uv run mcp run server.py --transport streamable-http
```

