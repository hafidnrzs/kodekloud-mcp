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

Basic Client
```bash
cd mcp-client
uv run python basic_client.py
```

## Tool Parameter Analysis

Examine the tools_client.py file to understand how MCP clients call server tools with specific parameters.

📋 Analysis Task:

1. Open and examine the file: /home/lab-user/mcp-client/tools_client.py
2. Look at the search_flights tool call in Test 1
3. Identify the destination airport parameter value
4. Optionally run the client to see the tools in action

🔍 Code Location:
```python
flight_result = await client.call_tool("search_flights", {
    "origin": "LAX",
    "destination": "???"
})
```

Command to test:
```bash
cd mcp-client
uv run python tools_client.py
```

💡 Find the destination airport code used in the search_flights tool call!

## MCP Roots Configuration

Examine the roots_client.py file to understand which directories are provided as file system roots to the server.

📋 Analysis Task:

1. Open and examine the file: /home/lab-user/mcp-client/roots_client.py
2. Look at the project_roots list defined at the top
3. Identify which directory path is NOT included in the current roots
4. Optionally run the client to see roots functionality

🔍 Code Location:
```python
project_roots = [
    "file:///home/lab-user/",
    "file:///home/lab-user/flight-booking-server/",
    "file:///home/lab-user/mcp-client/"
]
```

Command to test:
```bash
cd /home/lab-user/mcp-client
uv run python roots_client.py
```
💡 Which directory is missing from the roots list?

### Output:
🌳 Roots configuration summary:
📁 Provided 3 project roots:
   1. file:///home/lab-user/
   2. file:///home/lab-user/flight-booking-server/
   3. file:///home/lab-user/mcp-client/

💡 The server can now access files within these directories
   if it has file-related tools implemented!

🎉 Roots functionality test completed!
✨ Server now has potential access to specified directories!

## Implementing Sampling Support

Sampling allows servers to request LLM responses from clients. Learn how to handle these requests.

1. Examine the sampling_client.py file
2. Run it to see the sampling callback in action
3. Understand how to handle CreateMessageRequestParams
4. See how to return CreateMessageResult responses

Command to run:
```bash
cd /home/lab-user/mcp-client
uv run python sampling_client.py
```

🤖 Sampling scenarios handled:
- Travel explanation requests
- Story creation requests
- General recommendation requests

### Output:
🎭 SAMPLING MCP CLIENT
========================================
🎯 Goal: Handle server LLM sampling requests
========================================
✅ Connected to server with sampling support!

🔧 Checking for sampling-enabled tools...
  📝 create_booking: Create a flight booking

🧪 Testing potential sampling scenarios...
💡 Testing flight recommendation prompt...
✅ Prompt generated successfully (no sampling required)

🎭 Sampling callback is ready!
   If the server had tools that use ctx.session.create_message(),
   our sampling callback would handle those requests.

📋 Sampling callback features:
   - Handles travel explanation requests
   - Provides travel/flight explanations
   - Creates stories and recommendations
   - Returns contextual responses

🎉 Sampling client test completed!
✨ Ready to handle server LLM requests!

## Implementing Interactive Elicitation

Elicitation allows servers to request user input from clients. Experience true interactive MCP communication where the server can ask you for information directly.

1. Examine the elicitation_client.py file
2. Run it to see the interactive elicitation callback
3. Understand how real user input is captured
4. Experience live server-to-user communication

Command to run:
```bash
cd /home/lab-user/mcp-client
uv run python elicitation_client.py
```

🔔 Interactive features:

- Real-time user input prompts
- Intelligent response parsing (text or JSON)
- User can cancel with Ctrl+C
- Supports various input formats

### Output:
🔔 ELICITATION MCP CLIENT
========================================
🎯 Goal: Handle server user input requests
========================================
✅ Connected to server with elicitation support!

🔧 Checking for interactive tools...
  ⚠️  No obvious interactive tools found
     This is normal - the basic flight server doesn't have user input tools

🧪 Testing for potential elicitation triggers...
🎫 Testing booking creation (might ask for user details)...
✅ Booking result: {
  "booking_id": "BK456",
  "flight_id": "FL456",
  "passenger": "Test User",
  "status": "confirmed"
}

🔔 Elicitation callback is ready!
   If the server had tools that use ctx.session.elicit(),
   our elicitation callback would handle those requests.

📋 Elicitation callback features:
   - Prompts user for real input when server requests
   - Handles text responses and JSON input
   - Intelligently parses responses based on request type
   - Allows user to decline with Ctrl+C
   - Supports various input formats

🎯 Example user interaction:
   • Server asks: 'Please enter your name'
   • Client prompts: 'Please enter your response: '
   • User types: 'John Smith'
   • Client sends: {'name': 'John Smith'}

💡 Supported input formats:
   • Simple text: 'John Smith' → {'name': 'John Smith'}
   • JSON: '{"name": "John", "age": 30}' → parsed as JSON
   • Numbers: '500' for budget → {'budget': 500.0, 'currency': 'USD'}
   • Confirmations: 'yes' → {'confirmation': 'yes'}

🎉 Elicitation client test completed!
✨ Ready to handle server user input requests!

## Complete MCP Client Implementation

Now let's test a complete client that combines all MCP capabilities in one comprehensive implementation.

1. Examine the complete_client.py file
2. Run it to see all features working together
3. Observe the phased testing approach
4. See how all callbacks work in harmony

Command to run:
```bash
cd /home/lab-user/mcp-client
uv run python complete_client.py
```

🌟 Complete client features:

- Server discovery and capability listing
- Tool execution and resource access
- Roots provision for file system access
- Sampling for LLM request handling
- Elicitation for user input handling

## Building Production MCP Clients

Learn the key considerations for building robust, production-ready MCP clients.

1. Error handling and connection retry logic
2. Proper async/await patterns and resource cleanup
3. Security considerations for roots and callbacks
4. Testing strategies for MCP client applications

🏗️ Best practices covered:

- Connection management and error recovery
- Callback implementation patterns
- Integration with existing Python applications
- Monitoring and logging for MCP operations

## End

You've successfully learned to build comprehensive MCP clients that can integrate with any MCP server:

- ✅ Built basic clients for server discovery and connection
- ✅ Created tool-calling clients for automation
- ✅ Implemented advanced features: roots, sampling, elicitation
- ✅ Developed production-ready integration patterns
- ✅ Mastered async Python programming for MCP

🚀 What you've mastered:

- Complete MCP client development workflow
- Programmatic server integration techniques
- Advanced callback implementations
- Production deployment considerations
- Building custom MCP-powered applications

🔜 Next Steps in MCP Development:

- Integrate MCP clients into existing applications
- Build MCP-powered automation systems
- Create custom transport implementations
- Develop MCP-based AI agent platforms

💡 Pro Tip: MCP clients are the bridge between MCP servers and your applications - they enable powerful AI integrations!