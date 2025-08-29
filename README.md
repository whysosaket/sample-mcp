# Mem0 Memory MCP Server

A Model Context Protocol (MCP) server that provides memory storage and retrieval capabilities using [Mem0](https://github.com/mem0ai/mem0). This tool allows you to store and search through memories, making it useful for maintaining context and making informed decisions based on past interactions.

## Features

- Store memories with user-specific context
- Search through stored memories with relevance scoring
- Simple and intuitive API
- Built on the Model Context Protocol
- Automatic error handling
- Support for multiple user contexts

## Usage

This server now supports StreamableHTTP via Smithery CLI while retaining optional STDIO compatibility.

### StreamableHTTP (recommended)

- Development (opens Smithery Playground and exposes HTTP transport):

```bash
npm run dev
```

- Build for HTTP/StreamableHTTP (Smithery):

```bash
npm run build
```

- Start the HTTP server locally (Smithery-built entrypoint):

```bash
npm run start:http
```

You can configure the server using Smithery’s generated form in the playground or by setting environment variables (e.g., `MEM0_API_KEY`).

### STDIO (backward compatible)

Run the server over STDIO (useful for local clients that only support STDIO):

```bash
env MEM0_API_KEY=your-api-key-here npm run build:stdio && npm run start:stdio
```

## Configuration for AI Tools

### Running on Cursor (STDIO)

#### Configuring Cursor 🖥️

To configure Mem0 MCP in Cursor:

1. Open Cursor Settings
2. Go to Features > MCP Servers
3. Click "+ Add New MCP Server"
4. Enter the following:
   - Name: "mem0-mcp" (or your preferred name)
   - Type: "command"
   - Command: `env MEM0_API_KEY=your-api-key-here npx -y @mem0/mcp` (or use `start:stdio` from this repo)

To configure Mem0 MCP using JSON configuration:

```json
{
  "mcpServers": {
    "mem0-mcp": {
      "command": "npx",
      "args": ["-y", "@mem0/mcp"],
      "env": {
        "MEM0_API_KEY": "YOUR-API-KEY-HERE"
      }
    }
  }
}
```

### Running on VS Code (STDIO)

Add the following JSON block to your User Settings (JSON) file in VS Code:

```json
{
  "mcp": {
    "inputs": [
      {
        "type": "promptString",
        "id": "apiKey",
        "description": "Mem0 API Key",
        "password": true
      }
    ],
    "servers": {
      "mem0-memory": {
        "command": "npx",
        "args": ["-y", "@mem0/mcp"],
        "env": {
          "MEM0_API_KEY": "${input:apiKey}"
        }
      }
    }
  }
}
```

## Available Tools

### 1. Add Memory Tool (add-memory)

Store new memories with user-specific context.

```json
{
  "name": "add-memory",
  "arguments": {
    "content": "User prefers dark mode interface",
    "userId": "user123"
  }
}
```

### 2. Search Memories Tool (search-memories)

Search through stored memories to retrieve relevant information.

```json
{
  "name": "search-memories",
  "arguments": {
    "query": "What are the user's interface preferences?",
    "userId": "user123"
  }
}
```

## Response Format

### Add Memory Response

```json
{
  "content": [
    {
      "type": "text",
      "text": "Memory added successfully"
    }
  ],
  "isError": false
}
```

### Search Memory Response

```json
{
  "content": [
    {
      "type": "text",
      "text": "Memory: User prefers dark mode interface\nRelevance: 0.95\n---\nMemory: User mentioned liking minimal UI\nRelevance: 0.82\n---"
    }
  ],
  "isError": false
}
```

## Configuration

### Environment Variables

- `MEM0_API_KEY`: Your Mem0 API key (required)
  - Required for operation
  - Can be obtained from [Mem0 Dashboard](https://app.mem0.ai/dashboard/api-keys)

## Development

### Prerequisites

- Node.js >= 18
- A Mem0 API key

### Setup

1. Install dependencies:

```bash
npm install
```

2. Optionally create a `.env` file in the project directory and add your Mem0 API key:

```bash
MEM0_API_KEY=your-api-key-here
DEFAULT_USER_ID=mem0-mcp-user
```

### HTTP/StreamableHTTP Dev

```bash
npm run dev
```

### STDIO Dev

```bash
npm run build:stdio
npm run start:stdio
```

## Error Handling

The server includes error handling for:

- API connection issues
- Invalid memory operations
- Search errors
- Authentication failures

Example error response:

```json
{
  "content": [
    {
      "type": "text",
      "text": "Error: Failed to search memories: Invalid API key"
    }
  ],
  "isError": true
}
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

MIT 
