# Building Your Own MCP Server

Learn how to create custom MCP servers to extend AI capabilities with your own tools and data sources.

## Table of Contents

- [Overview](#overview)
- [Quick Start](#quick-start)
- [TypeScript Server](#typescript-server)
- [Python Server](#python-server)
- [Core Concepts](#core-concepts)
- [Testing Your Server](#testing-your-server)
- [Publishing](#publishing)

---

## Overview

### When to Build Custom Servers

Build your own server when:
- You need to integrate a service without an existing MCP server
- You have internal tools/APIs to expose
- You want custom functionality for your workflow
- Existing servers don't meet your security requirements

### Server Capabilities

MCP servers can provide:

| Capability | Description | Example |
|------------|-------------|---------|
| **Tools** | Actions the AI can execute | `create_issue`, `send_email` |
| **Resources** | Data the AI can read | File contents, database rows |
| **Prompts** | Pre-defined prompt templates | Code review template |

---

## Quick Start

### Option 1: Use create-mcp-server (Recommended)

```bash
# Create a new TypeScript server
npx @anthropic-ai/create-mcp-server my-server
cd my-server
npm install
npm run build
```

### Option 2: Clone a Template

```bash
# TypeScript template
git clone https://github.com/modelcontextprotocol/typescript-server-template
cd typescript-server-template
npm install

# Python template
git clone https://github.com/modelcontextprotocol/python-server-template
cd python-server-template
pip install -e .
```

---

## TypeScript Server

### Project Structure

```
my-mcp-server/
├── package.json
├── tsconfig.json
├── src/
│   └── index.ts
└── README.md
```

### package.json

```json
{
  "name": "my-mcp-server",
  "version": "1.0.0",
  "type": "module",
  "bin": {
    "my-mcp-server": "./dist/index.js"
  },
  "scripts": {
    "build": "tsc",
    "start": "node dist/index.js"
  },
  "dependencies": {
    "@modelcontextprotocol/sdk": "^1.0.0"
  },
  "devDependencies": {
    "typescript": "^5.0.0",
    "@types/node": "^20.0.0"
  }
}
```

### Basic Server (src/index.ts)

```typescript
#!/usr/bin/env node

import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import {
  CallToolRequestSchema,
  ListToolsRequestSchema,
} from "@modelcontextprotocol/sdk/types.js";

// Create server instance
const server = new Server(
  {
    name: "my-mcp-server",
    version: "1.0.0",
  },
  {
    capabilities: {
      tools: {},
    },
  }
);

// Define available tools
server.setRequestHandler(ListToolsRequestSchema, async () => {
  return {
    tools: [
      {
        name: "greet",
        description: "Greet someone by name",
        inputSchema: {
          type: "object",
          properties: {
            name: {
              type: "string",
              description: "The name to greet",
            },
          },
          required: ["name"],
        },
      },
      {
        name: "add_numbers",
        description: "Add two numbers together",
        inputSchema: {
          type: "object",
          properties: {
            a: { type: "number", description: "First number" },
            b: { type: "number", description: "Second number" },
          },
          required: ["a", "b"],
        },
      },
    ],
  };
});

// Handle tool calls
server.setRequestHandler(CallToolRequestSchema, async (request) => {
  const { name, arguments: args } = request.params;

  switch (name) {
    case "greet":
      return {
        content: [
          {
            type: "text",
            text: `Hello, ${args.name}! Nice to meet you.`,
          },
        ],
      };

    case "add_numbers":
      const sum = (args.a as number) + (args.b as number);
      return {
        content: [
          {
            type: "text",
            text: `The sum of ${args.a} and ${args.b} is ${sum}`,
          },
        ],
      };

    default:
      throw new Error(`Unknown tool: ${name}`);
  }
});

// Start the server
async function main() {
  const transport = new StdioServerTransport();
  await server.connect(transport);
  console.error("MCP server running on stdio");
}

main().catch(console.error);
```

### Build and Run

```bash
# Build
npm run build

# Test manually
node dist/index.js

# Configure in your client
# Command: node
# Args: ["/path/to/my-mcp-server/dist/index.js"]
```

---

## Python Server

### Project Structure

```
my-mcp-server/
├── pyproject.toml
├── src/
│   └── my_mcp_server/
│       ├── __init__.py
│       └── server.py
└── README.md
```

### pyproject.toml

```toml
[project]
name = "my-mcp-server"
version = "1.0.0"
description = "My custom MCP server"
requires-python = ">=3.10"
dependencies = [
    "mcp>=1.0.0",
]

[project.scripts]
my-mcp-server = "my_mcp_server.server:main"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

### Basic Server (src/my_mcp_server/server.py)

```python
#!/usr/bin/env python3

import asyncio
from mcp.server import Server
from mcp.server.stdio import stdio_server
from mcp.types import Tool, TextContent

# Create server instance
server = Server("my-mcp-server")

@server.list_tools()
async def list_tools() -> list[Tool]:
    """List available tools."""
    return [
        Tool(
            name="greet",
            description="Greet someone by name",
            inputSchema={
                "type": "object",
                "properties": {
                    "name": {
                        "type": "string",
                        "description": "The name to greet",
                    },
                },
                "required": ["name"],
            },
        ),
        Tool(
            name="add_numbers",
            description="Add two numbers together",
            inputSchema={
                "type": "object",
                "properties": {
                    "a": {"type": "number", "description": "First number"},
                    "b": {"type": "number", "description": "Second number"},
                },
                "required": ["a", "b"],
            },
        ),
    ]

@server.call_tool()
async def call_tool(name: str, arguments: dict) -> list[TextContent]:
    """Handle tool calls."""
    if name == "greet":
        return [
            TextContent(
                type="text",
                text=f"Hello, {arguments['name']}! Nice to meet you.",
            )
        ]
    
    elif name == "add_numbers":
        result = arguments["a"] + arguments["b"]
        return [
            TextContent(
                type="text",
                text=f"The sum of {arguments['a']} and {arguments['b']} is {result}",
            )
        ]
    
    else:
        raise ValueError(f"Unknown tool: {name}")

async def main():
    """Run the server."""
    async with stdio_server() as (read_stream, write_stream):
        await server.run(
            read_stream,
            write_stream,
            server.create_initialization_options(),
        )

if __name__ == "__main__":
    asyncio.run(main())
```

### Install and Run

```bash
# Install in development mode
pip install -e .

# Or using uv
uv pip install -e .

# Run
my-mcp-server

# Configure in client
# Command: uvx
# Args: ["my-mcp-server"]
```

---

## Core Concepts

### Tools

Tools are actions the AI can execute. Define them with:

- **name**: Unique identifier (snake_case)
- **description**: What the tool does (be descriptive!)
- **inputSchema**: JSON Schema for parameters

```typescript
{
  name: "search_database",
  description: "Search the database for records matching a query",
  inputSchema: {
    type: "object",
    properties: {
      query: {
        type: "string",
        description: "The search query",
      },
      limit: {
        type: "number",
        description: "Maximum results to return",
        default: 10,
      },
    },
    required: ["query"],
  },
}
```

### Resources

Resources provide data the AI can read:

```typescript
server.setRequestHandler(ListResourcesRequestSchema, async () => {
  return {
    resources: [
      {
        uri: "file:///config.json",
        name: "Configuration",
        description: "Application configuration",
        mimeType: "application/json",
      },
    ],
  };
});

server.setRequestHandler(ReadResourceRequestSchema, async (request) => {
  const { uri } = request.params;
  // Return resource contents
  return {
    contents: [
      {
        uri,
        mimeType: "application/json",
        text: JSON.stringify(config),
      },
    ],
  };
});
```

### Prompts

Pre-defined prompt templates:

```typescript
server.setRequestHandler(ListPromptsRequestSchema, async () => {
  return {
    prompts: [
      {
        name: "code_review",
        description: "Template for code review",
        arguments: [
          {
            name: "code",
            description: "Code to review",
            required: true,
          },
        ],
      },
    ],
  };
});
```

### Error Handling

Always handle errors gracefully:

```typescript
server.setRequestHandler(CallToolRequestSchema, async (request) => {
  try {
    // Tool logic here
  } catch (error) {
    return {
      content: [
        {
          type: "text",
          text: `Error: ${error.message}`,
        },
      ],
      isError: true,
    };
  }
});
```

---

## Testing Your Server

### Using MCP Inspector

```bash
# TypeScript
npx @modelcontextprotocol/inspector node dist/index.js

# Python
npx @modelcontextprotocol/inspector uvx my-mcp-server
```

### Unit Testing (TypeScript)

```typescript
import { describe, it, expect } from "vitest";
import { handleGreet } from "./handlers.js";

describe("greet tool", () => {
  it("should greet by name", async () => {
    const result = await handleGreet({ name: "Alice" });
    expect(result.content[0].text).toContain("Alice");
  });
});
```

### Integration Testing

```bash
# Create a test script
echo '{"jsonrpc":"2.0","id":1,"method":"tools/list"}' | node dist/index.js
```

---

## Publishing

### npm (TypeScript)

```bash
# Ensure package.json is correct
npm publish --access public
```

### PyPI (Python)

```bash
# Build
python -m build

# Publish
twine upload dist/*
```

### Best Practices for Publishing

1. **Clear README**: Include installation, configuration, and examples
2. **Semantic versioning**: Follow semver for updates
3. **Changelog**: Document changes between versions
4. **License**: MIT or Apache 2.0 recommended
5. **Tests**: Include and run tests in CI

---

## Real-World Example: API Integration

Here's a more complete example integrating with an external API:

```typescript
import { Server } from "@modelcontextprotocol/sdk/server/index.js";

const server = new Server({
  name: "weather-server",
  version: "1.0.0",
});

server.setRequestHandler(ListToolsRequestSchema, async () => ({
  tools: [{
    name: "get_weather",
    description: "Get current weather for a city",
    inputSchema: {
      type: "object",
      properties: {
        city: { type: "string", description: "City name" },
      },
      required: ["city"],
    },
  }],
}));

server.setRequestHandler(CallToolRequestSchema, async (request) => {
  const { city } = request.params.arguments;
  
  // Call external API
  const response = await fetch(
    `https://api.weather.com/v1/current?city=${encodeURIComponent(city)}`,
    {
      headers: {
        "Authorization": `Bearer ${process.env.WEATHER_API_KEY}`,
      },
    }
  );
  
  if (!response.ok) {
    return {
      content: [{ type: "text", text: `Error fetching weather: ${response.statusText}` }],
      isError: true,
    };
  }
  
  const data = await response.json();
  
  return {
    content: [{
      type: "text",
      text: `Weather in ${city}: ${data.temperature}°C, ${data.conditions}`,
    }],
  };
});
```

---

## Resources

- [MCP Specification](https://spec.modelcontextprotocol.io)
- [TypeScript SDK](https://github.com/modelcontextprotocol/typescript-sdk)
- [Python SDK](https://github.com/modelcontextprotocol/python-sdk)
- [Official Examples](https://github.com/modelcontextprotocol/servers)

---

**← Previous**: [Troubleshooting](troubleshooting.md)
