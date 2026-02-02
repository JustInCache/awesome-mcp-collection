# Getting Started with MCP Servers

This guide will walk you through setting up MCP servers from scratch, whether you're using Claude Desktop, Cursor, VS Code, or another MCP-compatible client.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Understanding MCP](#understanding-mcp)
- [Client Setup](#client-setup)
- [Installing Your First Server](#installing-your-first-server)
- [Configuration Deep Dive](#configuration-deep-dive)
- [Testing Your Setup](#testing-your-setup)
- [Next Steps](#next-steps)

---

## Prerequisites

Before you begin, ensure you have:

| Requirement | Why | How to Install |
|-------------|-----|----------------|
| **Node.js 18+** | Most MCP servers are TypeScript | [nodejs.org](https://nodejs.org) |
| **Python 3.10+** | For Python-based servers | [python.org](https://python.org) |
| **uv** (optional) | Fast Python package manager | `curl -LsSf https://astral.sh/uv/install.sh \| sh` |
| **An MCP Client** | To connect to servers | See [Client Setup](#client-setup) |

### Verify Your Installation

```bash
# Check Node.js
node --version  # Should be v18.0.0 or higher

# Check npm
npm --version   # Should be v9.0.0 or higher

# Check Python (optional)
python3 --version  # Should be 3.10 or higher

# Check uv (optional, for Python servers)
uv --version
```

---

## Understanding MCP

### What is MCP?

**Model Context Protocol (MCP)** is an open standard that lets AI assistants connect to external tools and data sources. Think of it as:

- **USB for AI**: A universal connector that works with any compatible tool
- **Plugin System**: Extend your AI's capabilities without retraining
- **Secure Bridge**: Controlled access to your files, databases, and APIs

### How It Works

```
┌─────────────────┐     MCP Protocol     ┌─────────────────┐
│   AI Client     │◄───────────────────► │   MCP Server    │
│  (Claude, etc.) │    JSON-RPC 2.0      │  (GitHub, etc.) │
└─────────────────┘                      └─────────────────┘
        │                                        │
        │  "Search for issues                    │
        │   in my repo"                          │
        ▼                                        ▼
   AI understands                         Server executes
   and delegates                          and returns results
```

### Key Concepts

| Concept | Description |
|---------|-------------|
| **Server** | A program that provides tools/resources to AI |
| **Client** | An AI application that connects to servers |
| **Tool** | A specific action (e.g., "create_issue", "read_file") |
| **Resource** | Data the AI can access (e.g., file contents, database rows) |
| **Transport** | How client/server communicate (stdio, SSE, HTTP) |

---

## Client Setup

### Claude Desktop

1. **Download** Claude Desktop from [claude.ai/download](https://claude.ai/download)

2. **Locate** the configuration file:
   - **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
   - **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

3. **Create** the file if it doesn't exist:
   ```bash
   # macOS
   mkdir -p ~/Library/Application\ Support/Claude
   touch ~/Library/Application\ Support/Claude/claude_desktop_config.json
   ```

4. **Add** your first server (see [Installing Your First Server](#installing-your-first-server))

### Cursor

1. **Open** Cursor IDE
2. **Go to** Settings → Features → MCP
3. **Click** "Edit MCP Settings"
4. **Add** your server configurations

### VS Code + Continue

1. **Install** the [Continue extension](https://marketplace.visualstudio.com/items?itemName=Continue.continue)
2. **Open** the Continue configuration: `~/.continue/config.json`
3. **Add** the `mcpServers` section to your config

### Windsurf

1. **Open** Windsurf settings
2. **Navigate** to AI → MCP Servers
3. **Configure** your servers via the UI or JSON

---

## Installing Your First Server

Let's install the **Filesystem** server—a simple, safe starting point.

### Step 1: Test the Server

First, verify the server works:

```bash
# Run the filesystem server directly
npx -y @modelcontextprotocol/server-filesystem /tmp/test-mcp
```

You should see output like:
```
MCP Filesystem Server running on stdio
Allowed directories: ["/tmp/test-mcp"]
```

Press `Ctrl+C` to stop.

### Step 2: Configure Your Client

Add this to your client's configuration file:

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/Users/yourname/projects"
      ]
    }
  }
}
```

**Important**: Replace `/Users/yourname/projects` with your actual directory path!

### Step 3: Restart Your Client

- **Claude Desktop**: Quit completely (Cmd+Q / Alt+F4) and reopen
- **Cursor**: Restart the IDE
- **VS Code**: Reload the window (Cmd+Shift+P → "Reload Window")

### Step 4: Verify It Works

In your AI chat, try:

> "Can you list the files in my projects directory?"

If configured correctly, the AI will use the filesystem server to list your files.

---

## Configuration Deep Dive

### Configuration Structure

```json
{
  "mcpServers": {
    "server-name": {
      "command": "executable",
      "args": ["arg1", "arg2"],
      "env": {
        "ENV_VAR": "value"
      }
    }
  }
}
```

### Field Reference

| Field | Required | Description |
|-------|----------|-------------|
| `command` | ✅ | The executable to run (e.g., `npx`, `uvx`, `python`) |
| `args` | ✅ | Arguments to pass to the command |
| `env` | ❌ | Environment variables for the server |

### Using Environment Variables

**Never hardcode secrets!** Use environment variable references:

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

Then set the variable in your shell:
```bash
# Add to ~/.zshrc or ~/.bashrc
export GITHUB_TOKEN="ghp_xxxxxxxxxxxx"
```

### Multiple Servers

You can configure multiple servers:

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "."]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    }
  }
}
```

---

## Testing Your Setup

### Method 1: Ask Your AI

Simply ask your AI to use the server:

> "Using the filesystem server, show me the contents of my README.md file"

### Method 2: Use the MCP Inspector

The [MCP Inspector](https://github.com/modelcontextprotocol/inspector) is an official debugging tool:

```bash
npx @modelcontextprotocol/inspector npx -y @modelcontextprotocol/server-filesystem /tmp
```

This opens a web UI where you can:
- See available tools and resources
- Test tool calls manually
- Debug server responses

### Method 3: Check Logs

- **Claude Desktop**: View → Developer → Developer Tools → Console
- **Cursor**: Help → Toggle Developer Tools
- **VS Code**: Output panel → "Continue" channel

---

## Next Steps

Now that you have your first server running:

1. **Add more servers**: Check our [README](../README.md) for recommended servers by category

2. **Use a stack config**: Copy a ready-made configuration from [configs/](../configs/):
   - [minimal.json](../configs/minimal.json) - Essential servers
   - [fullstack-js.json](../configs/fullstack-js.json) - JavaScript developers
   - [devops.json](../configs/devops.json) - Platform engineers

3. **Read best practices**: Learn security and performance tips in [best-practices.md](best-practices.md)

4. **Build your own**: When ready, see [building-servers.md](building-servers.md)

---

## Common Issues

### Server Won't Start

```bash
# Check if npx can find the package
npx -y @modelcontextprotocol/server-filesystem --help

# Clear npm cache if needed
npm cache clean --force
```

### "Server not found" Error

1. Verify the server name matches exactly
2. Check the configuration JSON is valid
3. Restart your client completely

### Environment Variables Not Working

1. Verify the variable is set: `echo $GITHUB_TOKEN`
2. Restart your terminal after adding to `.zshrc`/`.bashrc`
3. Some clients require app restart, not just window reload

### Permission Denied

1. Check the directory path is accessible
2. Verify file permissions: `ls -la /path/to/directory`
3. On macOS, grant Full Disk Access if needed

---

## Getting Help

- **Official Docs**: [modelcontextprotocol.io](https://modelcontextprotocol.io)
- **GitHub Issues**: [modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers/issues)
- **Discord**: [MCP Discord Server](https://discord.gg/mcp)
- **Reddit**: [r/mcp](https://reddit.com/r/mcp)

---

**Next**: [Best Practices](best-practices.md) →
