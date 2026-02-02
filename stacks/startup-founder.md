# Startup Founder Stack

Essential MCP servers for founders building MVPs and scaling startups.

## Overview

| Focus | Servers | Setup Time |
|-------|---------|------------|
| Full Product | 8 servers | ~20 minutes |
| Code + Business | Dev, Payments, PM | Intermediate |

---

## Recommended Servers

### Development

| Server | Purpose | Priority |
|--------|---------|----------|
| **Filesystem** | Access all code | ⭐⭐⭐ |
| **GitHub** | Version control, CI/CD | ⭐⭐⭐ |
| **Supabase** | Backend-as-a-Service | ⭐⭐⭐ |

### Business Tools

| Server | Purpose | Priority |
|--------|---------|----------|
| **Stripe** | Payments, subscriptions | ⭐⭐⭐ |
| **Linear** | Issue tracking | ⭐⭐ |
| **Slack** | Team communication | ⭐⭐ |

### Research & Growth

| Server | Purpose | Priority |
|--------|---------|----------|
| **Brave Search** | Market research | ⭐⭐ |
| **Memory** | Track decisions | ⭐⭐⭐ |

---

## Quick Setup

### 1. Copy Configuration

Download: [startup.json](../configs/startup.json)

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "${HOME}/startup"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}" }
    },
    "supabase": {
      "command": "npx",
      "args": ["-y", "supabase-mcp-server"],
      "env": {
        "SUPABASE_URL": "${SUPABASE_URL}",
        "SUPABASE_KEY": "${SUPABASE_KEY}"
      }
    },
    "stripe": {
      "command": "uvx",
      "args": ["stripe-agent-toolkit"],
      "env": { "STRIPE_SECRET_KEY": "${STRIPE_SECRET_KEY}" }
    },
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    }
  }
}
```

### 2. Set Environment Variables

```bash
export GITHUB_TOKEN="ghp_xxxxxxxxxxxx"
export SUPABASE_URL="https://xxx.supabase.co"
export SUPABASE_KEY="eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
export STRIPE_SECRET_KEY="sk_live_xxxxxxxxxxxx"
```

---

## Use Cases

### "Help me ship this feature today"

The AI can:
- Read and modify code (Filesystem)
- Create database tables (Supabase)
- Push to GitHub (GitHub)
- Create tasks for follow-up (Linear)

### "Set up subscription billing"

The AI can:
- Create Stripe products and prices (Stripe)
- Generate checkout code (Filesystem)
- Set up webhooks (Stripe + Supabase)

### "Research competitors"

The AI can:
- Search the web (Brave Search)
- Summarize findings (Memory)
- Create tasks for action items (Linear)

---

## Tips

1. **Start with Supabase** - fastest backend setup
2. **Use Stripe test mode** until ready for production
3. **Memory server** keeps track of product decisions
4. **Add Linear** when you need structured task management

---

## Related Stacks

- [Full-Stack Developer](fullstack-developer.md) - More dev-focused
- [Enterprise](enterprise.md) - Scaling up
