# Full-Stack Developer Stack

The essential MCP servers for full-stack JavaScript/TypeScript developers.

## Overview

| Focus | Servers | Setup Time |
|-------|---------|------------|
| Web Development | 7 servers | ~10 minutes |
| Frontend + Backend | React, Node, PostgreSQL | Intermediate |

---

## Recommended Servers

### Core (Must Have)

| Server | Purpose | Priority |
|--------|---------|----------|
| **Filesystem** | Access project files | ⭐⭐⭐ |
| **GitHub** | Version control, PRs, issues | ⭐⭐⭐ |
| **Memory** | Persistent context | ⭐⭐⭐ |

### Database

| Server | Purpose | Priority |
|--------|---------|----------|
| **PostgreSQL** | Primary database | ⭐⭐⭐ |
| **Supabase** | BaaS alternative | ⭐⭐ |
| **Redis** | Caching, sessions | ⭐⭐ |

### Development

| Server | Purpose | Priority |
|--------|---------|----------|
| **Puppeteer** | E2E testing, screenshots | ⭐⭐ |
| **Sequential Thinking** | Complex problem solving | ⭐⭐ |
| **Fetch** | API testing, web content | ⭐⭐ |

---

## Quick Setup

### 1. Copy Configuration

Download: [fullstack-js.json](../configs/fullstack-js.json)

Or copy manually:

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "${HOME}/projects"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "POSTGRES_CONNECTION_STRING": "${DATABASE_URL}"
      }
    },
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    },
    "puppeteer": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-puppeteer"]
    }
  }
}
```

### 2. Set Environment Variables

```bash
# Add to ~/.zshrc or ~/.bashrc
export GITHUB_TOKEN="ghp_xxxxxxxxxxxx"
export DATABASE_URL="postgresql://user:pass@localhost:5432/mydb"
```

### 3. Restart Your Client

Fully quit and reopen Claude Desktop, Cursor, or your MCP client.

---

## Use Cases

### "Help me debug this API endpoint"

The AI can:
- Read your source code (Filesystem)
- Check recent commits (GitHub)
- Query the database (PostgreSQL)
- Test the endpoint (Fetch)

### "Write tests for my React component"

The AI can:
- Read the component code (Filesystem)
- Understand testing patterns (Memory)
- Generate and run E2E tests (Puppeteer)

### "Review my PR before merging"

The AI can:
- Read PR diff and comments (GitHub)
- Analyze code changes (Filesystem)
- Check for breaking changes (Memory context)

---

## Tips

1. **Limit filesystem scope** to specific project directories
2. **Use read-only database access** for safety
3. **Enable Puppeteer** only when doing frontend work
4. **Memory server** helps maintain context across sessions

---

## Related Stacks

- [Frontend Developer](frontend-developer.md) - Focus on React/Vue/Angular
- [Backend Developer](backend-developer.md) - API and server focus
- [DevOps Engineer](devops-engineer.md) - Infrastructure focus
