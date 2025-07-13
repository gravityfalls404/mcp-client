# MCP Client

A Python client for the Model Context Protocol (MCP) that enables seamless integration between Claude AI and MCP servers. This client provides an interactive interface to connect to MCP servers and process queries using Claude's reasoning capabilities combined with server-provided tools.

## Features

- **MCP Server Integration**: Connect to any MCP server (Python or JavaScript) via stdio transport
- **Claude AI Integration**: Leverage Claude 3.5 Sonnet for intelligent query processing
- **Tool Execution**: Automatically execute server tools based on Claude's reasoning
- **Interactive Chat Interface**: Command-line interface for real-time interaction
- **Async Architecture**: Built with asyncio for efficient concurrent operations
- **Environment Configuration**: Support for .env files for API key management

## Prerequisites

- Python 3.12 or higher
- Anthropic API key (for Claude AI integration)
- MCP server script (Python or JavaScript)

## Installation

1. **Clone the repository**:
   ```bash
   git clone <repository-url>
   cd mcp-client
   ```

2. **Install dependencies using uv**:
   ```bash
   uv venv
   uv sync
   ```

3. **Activate the virtual environment**:
   ```bash
   source .venv/bin/activate
   ```

4. **Set up environment variables**:
   Create a `.env` file in the project root:
   ```env
   ANTHROPIC_API_KEY=your_anthropic_api_key_here
   ```

## Usage

### Basic Usage

Run the client with an MCP server script:

```bash
python client.py /path/to/your/server_script.py
```

Or for JavaScript servers:

```bash
python client.py /path/to/your/server_script.js
```

### Interactive Mode

Once connected, you can interact with the client:

```
MCP Client Started!
Type your queries or 'quit' to exit.

Query: What tools are available?
Connected to server with tools: ['tool1', 'tool2', 'tool3']

Query: Use tool1 to get some data
[Calling tool tool1 with args {...}]
Response from tool1: ...

Query: quit
```

### Programmatic Usage

You can also use the client programmatically:

```python
import asyncio
from client import MCPClient

async def main():
    client = MCPClient()
    try:
        # Connect to server
        await client.connect_to_server("/path/to/server.py")
        
        # Process a query
        response = await client.process_query("Your query here")
        print(response)
        
    finally:
        await client.cleanup()

asyncio.run(main())
```

## How It Works

1. **Server Connection**: The client connects to an MCP server using stdio transport
2. **Tool Discovery**: Available tools are listed and their schemas are retrieved
3. **Query Processing**: User queries are sent to Claude AI with available tool schemas
4. **Tool Execution**: Claude determines which tools to use and the client executes them
5. **Response Generation**: Results are sent back to Claude for final response generation

## Project Structure

```
mcp-client/
├── client.py          # Main client implementation
├── pyproject.toml     # Project configuration and dependencies
├── README.md          # This file
└── uv.lock           # Dependency lock file
```

## Dependencies

- **anthropic**: Claude AI API client
- **mcp**: Model Context Protocol implementation
- **python-dotenv**: Environment variable management

## Configuration

### Environment Variables

- `ANTHROPIC_API_KEY`: Your Anthropic API key for Claude AI access

### Server Requirements

- MCP servers must support stdio transport
- Servers can be written in Python or JavaScript
- Servers must implement the MCP protocol specification

## Examples

### Example 1: File System Server

```bash
python client.py /path/to/filesystem_server.py
```

Query: "List all files in the current directory"

### Example 2: Database Server

```bash
python client.py /path/to/database_server.py
```

Query: "Query the users table and show me all active users"

## Error Handling

The client includes comprehensive error handling for:
- Invalid server script paths
- Connection failures
- Tool execution errors
- API rate limiting
- Network timeouts