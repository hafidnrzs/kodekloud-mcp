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

Ini bertujuan untuk mengecek konektivitas MCP Client dengan flight-booking MCP Server. Setelah terhubung, akan mengecek apa yang disediakan oleh MCP server: daftar Tools, Resources, dan Prompts

## Tool Parameter Analysis

Examine the tools_client.py file to understand how MCP clients call server tools with specific parameters.

📋 Analysis Task:

1. Open and examine the file: `mcp-client/tools_client.py`
2. Look at the `search_flights` tool call in Test 1
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

`tools_client.py` menjalankan tes pemanggilan tools, resource, dan prompt dari sisi client. Yang dilakukan pada kode tersebut:
- Client memanggil tools `call_tool("nama_tool", {"param": "value"})`
- Client membaca resource `read_resource("nama_resource")`
- Client mengambil prompt `get_prompt("nama_prompt", {"param": "value"})`

## MCP Roots Configuration

Examine the `roots_client.py` file to understand which directories are provided as file system roots to the server.

📋 Analysis Task:

1. Open and examine the file: `mcp-client/roots_client.py`
2. Look at the project_roots list defined at the top
3. Identify which directory path is NOT included in the current roots
4. Optionally run the client to see roots functionality

🔍 Code Location:
```python
project_roots = [
   "D:/aegislabs/kodekloud-mcp/lab3",
   "D:/aegislabs/kodekloud-mcp/lab3/flight-booking-server/",
   "D:/aegislabs/kodekloud-mcp/lab3/mcp-client/",
]
```

Command to test:
```bash
cd mcp-client
uv run python roots_client.py
```
💡 Which directory is missing from the roots list?

### Output:
🌳 Roots configuration summary:
📁 Provided 3 project roots:
   1. D:/aegislabs/kodekloud-mcp/lab3
   2. D:/aegislabs/kodekloud-mcp/lab3/flight-booking-server/
   3. D:/aegislabs/kodekloud-mcp/lab3/mcp-client/

💡 The server can now access files within these directories
   if it has file-related tools implemented!

🎉 Roots functionality test completed!
✨ Server now has potential access to specified directories!

## Implementing Sampling Support

Sampling allows servers to request LLM responses from clients. Learn how to handle these requests.

1. Examine the `sampling_client.py` file
2. Run it to see the sampling callback in action
3. Understand how to handle CreateMessageRequestParams
4. See how to return CreateMessageResult responses

Command to run:
```bash
cd mcp-client
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

### Penjelasan Output

Secara singkat sampling = server minta client menjalankan panggilan ke LLM (generate completion) atas nama server. Bukan client yang selalu prompting server, tapi server mengajukan permintaan completion ke client.

Saat menjalankan `sampling_client.py`, pertama client akan terhubung ke **server**.

Kemudian mengecek tools yang mengaktifkan fitur sampling. Pada kasus ini, yaitu `create_booking` karena filter keyword "generate", "create", "write", "compose".

Uji coba skenario, yaitu prompt `find_best_flight` yang ternyata tidak perlu sampling (artinya server bisa langsung jawab tanpa minta klien tembak ke LLM).

Sampling callback sudah siap, yaitu fungsi/hook di client yang nanti akan dipanggil kalau server butuh LLM completion (misalnya server panggil `ctx.session.create_message()` di kode).

Fitur callback ini diprogram untuk:
- Menjelaskan perjalanan/penerbangan
- Memberi rekomendasi/cerita
- Menjawab pertanyaan kontekstual terkait perjalanan/penerbangan

**Question**
Apakah sampling ini digunakan misal server minta buatkan bahasa natural sebagai pelengkap output yang dihasilkan oleh tool atau resource dari server?

Server bisa mengirimkan permintaan sampling dan client sepenuhnya mengontrol. Kalau diterima, client akan melakukan panggilan ke LLM untuk menghasilkan teks yang lebih natural dan sesuai konteks. Kalau ditolak, client bisa memberikan respons alternatif atau mendapatkan data mentah dari server.

## Implementing Interactive Elicitation

Elicitation allows servers to request user input from clients. Experience true interactive MCP communication where the server can ask you for information directly.

1. Examine the `elicitation_client.py` file
2. Run it to see the interactive elicitation callback
3. Understand how real user input is captured
4. Experience live server-to-user communication

Command to run:
```bash
cd mcp-client
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

### Penjelasan Output

Elicitation memungkinkan server meminta input dari client.

Pada file `elicitation_client.py` terdapat fungsi `handle_elicitation()` yang merupakan handler untuk "elicitation" di MCP Client. Artinya ketika MCP Server meminta input langsung dari user, fungsi ini akan dipanggil untuk mengumpulkan, memproses, dan mengirimkan kembali jawaban user.

Fungsi tersebut mengembalikan `ElicitResult` yang isinya:
- `action`: "accept" atau "decline"
- `content`: data/jawaban yang diberikan user atau alasan penolakan

**Pertanyaan**
Jadi, elicitation itu semacam pertanyaan konfirmasi dari server ke client untuk mendapatkan input yang lebih spesifik untuk melengkapi informasi yang akan dikirimkan pada MCP server?

Ya, Elicitation di MCP memang seperti pertanyaan langsung dari server ke client untuk melengkapi data yang belum dimiliki server, meminta konfirmasi sebelum melakukan aksi (misal: approve, hapus data), dan mengumpulkan input yang sifatnya sensitif atau personal.

Perbedaannya dengan sampling?
- Sampling: Server minta client memanggil LLM untuk memproses/generate sesuatu
- Elicitation: Server meminta user di sisi client untuk memberikan input manual. Tidak selalu melibatkan LLM, fokusnya adalah interaksi manusia.


Tapi, Elicitation itu didefinisikan pada MCP Client ya? Lalu, bagaimana cara kerja di MCP Server agar bisa mendeteksi kalau itu men-trigger elicitaiton?

Bukan terdeteksi otomatis. Server secara sadar memanggil API elicitation ketika:
- butuh data yang belum ada (parameter tool kurang, klarifikasi, preferensi), atau
- butuh konfirmasi user sebelum lanjut aksi

Kapan aman dipakai?
- Client harus memastikan kemampuan "elicitation" saat inisialisasi. Server bisa cek `capability` dari client dulu; kalau tidak ada, fallback misal pakai default atau batalkan aksi.


Berarti extension AI Agent atau chat pada VSCode sperti GitHub Copilot Chat, Roo Code, Cline itu udah punya fitur Elicitation yang sangat canggih dan menangani berbagai kasus?

Ya, extension AI Agent/chat di VSCode seperti GitHub Copilot Chat, Roo Code, dan Cline sudah memiliki fitur Elicitation yang canggih. Mereka dapat meminta input dari pengguna dengan cara yang lebih interaktif. Umumnya sudah punya fitur elicitation yang:
- Sangat canggih
  - UI interaktif: tidak cuma lewat terminal, tapi bisa lewat pop-up, dialog, atau elemen UI lainnya.
  - JSON Schema aware: jika server mengirim _elicitation request_ dengan JSON Schema, UI akan otomatis buat form yang sesuai schema.
  - Validasi otomatis: bisa validasi input sesuai tipe data
  - Multi-modal: beberapa bisa menangani input file, pilih dari workspae, atau klik bagian kode langsung dari editor.
- Menangani berbagai kasus
   - Konfirmasi tindakan
   - Pengisian parameter tool
   - Refinement: saat jawaban LLM masih ambigu, server bisa klarifikasi lewat elicitation
   - Sensitive input: token API, kredensial, atau input yang tidak aman kalau lewat LLM

## Complete MCP Client Implementation

Now let's test a complete client that combines all MCP capabilities in one comprehensive implementation.

1. Examine the `complete_client.py` file
2. Run it to see all features working together
3. Observe the phased testing approach
4. See how all callbacks work in harmony

Command to run:
```bash
cd mcp-client
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