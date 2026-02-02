# Configuration Templates

Ready-to-use MCP configuration files for different developer personas and use cases.

## Available Configurations

| Config | Description | Best For |
|--------|-------------|----------|
| [minimal.json](minimal.json) | Essential 3 servers | Everyone (start here) |
| [fullstack-js.json](fullstack-js.json) | JS/TS development | React, Node, Next.js developers |
| [python-datascience.json](python-datascience.json) | Data science & ML | Python developers, data scientists |
| [devops.json](devops.json) | Infrastructure & DevOps | Platform engineers, SREs |
| [enterprise.json](enterprise.json) | Enterprise with security | Large teams, compliance needs |
| [frontend.json](frontend.json) | Frontend focus | React, Vue, Angular developers |
| [startup.json](startup.json) | Startup tools | Founders, full-stack indie hackers |
| [security.json](security.json) | Security focus | Security engineers, pentesters |
| [mobile.json](mobile.json) | Mobile development | iOS, Android developers |

## How to Use

### 1. Find Your Config Location

| Client | Location |
|--------|----------|
| Claude Desktop (macOS) | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Claude Desktop (Windows) | `%APPDATA%\Claude\claude_desktop_config.json` |
| Cursor | Settings → Features → MCP → Edit Config |
| VS Code + Continue | `~/.continue/config.json` |

### 2. Copy the Configuration

```bash
# Example: Copy minimal config to Claude Desktop (macOS)
cp configs/minimal.json ~/Library/Application\ Support/Claude/claude_desktop_config.json
```

### 3. Set Environment Variables

Most configs use environment variables for sensitive data:

```bash
# Add to ~/.zshrc or ~/.bashrc
export GITHUB_TOKEN="ghp_xxxxxxxxxxxx"
export DATABASE_URL="postgresql://user:pass@localhost/db"
export BRAVE_API_KEY="your-brave-api-key"
```

### 4. Restart Your Client

Fully quit and reopen your MCP client.

## Customizing Configurations

### Adding a Server

```json
{
  "mcpServers": {
    "existing-server": { ... },
    "new-server": {
      "command": "npx",
      "args": ["-y", "@scope/server-name"],
      "env": {
        "API_KEY": "${YOUR_ENV_VAR}"
      }
    }
  }
}
```

### Disabling a Server

Comment it out or remove it:

```json
{
  "mcpServers": {
    "active-server": { ... }
    // "disabled-server": { ... }
  }
}
```

## Tips

1. **Start with minimal.json** and add servers as needed
2. **Never commit secrets** - always use environment variables
3. **Test changes** by restarting your client after modifications
4. **Validate JSON** before saving: `cat config.json | jq .`
