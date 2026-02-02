# MCP Best Practices

This guide covers security, performance, and usage best practices for working with MCP servers.

## Table of Contents

- [Security](#security)
- [Performance](#performance)
- [Configuration](#configuration)
- [Debugging](#debugging)
- [Production Considerations](#production-considerations)

---

## Security

### 🔐 Credential Management

**Never hardcode secrets in configuration files.**

#### ❌ Bad

```json
{
  "mcpServers": {
    "github": {
      "env": {
        "GITHUB_TOKEN": "ghp_xxxxxxxxxxxxxxxxxxxx"
      }
    }
  }
}
```

#### ✅ Good

```json
{
  "mcpServers": {
    "github": {
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

Then set in your shell profile:
```bash
# ~/.zshrc or ~/.bashrc
export GITHUB_TOKEN="ghp_xxxxxxxxxxxxxxxxxxxx"
```

### 🔒 Use Secret Managers for Teams

For team environments, use proper secret management:

| Solution | Best For |
|----------|----------|
| **1Password CLI** | Personal/small teams |
| **HashiCorp Vault** | Enterprise |
| **AWS Secrets Manager** | AWS-centric teams |
| **Doppler** | Multi-environment |

Example with 1Password CLI:
```bash
export GITHUB_TOKEN=$(op read "op://Private/GitHub Token/credential")
```

### 🛡️ Principle of Least Privilege

Only grant servers the minimum permissions needed:

| Server | Recommended Scope |
|--------|-------------------|
| **Filesystem** | Specific project directories only |
| **GitHub** | Read-only unless you need write |
| **Database** | Read-only user for exploration |

#### Filesystem Example

```json
{
  "mcpServers": {
    "filesystem": {
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/Users/me/projects/specific-project"
      ]
    }
  }
}
```

**Don't**: Allow access to your entire home directory  
**Do**: Limit to specific project directories

### 🔍 Audit Server Sources

Before installing any MCP server:

1. **Check the repository**: Is it actively maintained?
2. **Review the code**: Especially for `package.json` dependencies
3. **Verify the publisher**: Official vs. community
4. **Check issues/PRs**: Any security concerns raised?

### 🚫 What to Avoid

| Risk | Example | Mitigation |
|------|---------|------------|
| Overly broad file access | `filesystem /` | Limit to project dirs |
| Storing secrets in config | Hardcoded API keys | Use env variables |
| Running as root | `sudo` commands | Use regular user |
| Unverified servers | Random npm packages | Verify source first |

---

## Performance

### ⚡ Start Small

Don't enable too many servers at once:

| Setup | Servers | Startup Time | Memory |
|-------|---------|--------------|--------|
| Minimal | 3 | ~2s | ~100MB |
| Standard | 6 | ~5s | ~250MB |
| Full | 12+ | ~15s+ | ~500MB+ |

### 🎯 Choose Servers Strategically

Only enable servers you'll actually use:

```json
{
  "mcpServers": {
    // ✅ Use daily
    "filesystem": { ... },
    "github": { ... },
    
    // ⚠️ Only enable when needed
    // "kubernetes": { ... },
    // "aws": { ... }
  }
}
```

### 📊 Monitor Resource Usage

Check which servers are consuming resources:

```bash
# macOS/Linux - Find MCP server processes
ps aux | grep -E "(npx|uvx|mcp)" | grep -v grep

# Check memory usage
top -l 1 | grep -E "(npx|node|python)"
```

### 🔄 Server Lifecycle

Servers start when your client starts and run continuously. Consider:

- **Restart your client** periodically to clear memory
- **Disable unused servers** to reduce background load
- **Use remote servers** when available (offloads processing)

---

## Configuration

### 📁 Organize Your Configuration

For complex setups, use comments and organization:

```json
{
  "mcpServers": {
    // === Core Tools ===
    "filesystem": { ... },
    "memory": { ... },
    
    // === Development ===
    "github": { ... },
    "postgres": { ... },
    
    // === Optional (uncomment when needed) ===
    // "kubernetes": { ... }
  }
}
```

> Note: JSON doesn't officially support comments, but many MCP clients accept them.

### 🔀 Environment-Specific Configs

Maintain different configs for different contexts:

```
~/.config/mcp/
├── work.json      # Work projects with enterprise servers
├── personal.json  # Personal projects with fewer servers
└── minimal.json   # Debugging with minimal setup
```

### ✅ Validate Your Configuration

Before restarting your client, validate the JSON:

```bash
# Using jq
cat claude_desktop_config.json | jq .

# Using Python
python3 -m json.tool claude_desktop_config.json
```

### 🔄 Version Control Your Config

Track your configuration in a dotfiles repo:

```bash
# Create a symlink
ln -s ~/dotfiles/mcp/claude_desktop_config.json \
      ~/Library/Application\ Support/Claude/claude_desktop_config.json
```

---

## Debugging

### 🔍 Enable Verbose Logging

Many servers support debug mode:

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "DEBUG": "mcp:*",
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

### 🛠️ Use MCP Inspector

The official debugging tool:

```bash
# Install and run
npx @modelcontextprotocol/inspector npx -y @modelcontextprotocol/server-github
```

Features:
- View all available tools
- Test tool calls
- Inspect responses
- Debug errors

### 📋 Common Debugging Steps

1. **Check if server starts manually**:
   ```bash
   npx -y @modelcontextprotocol/server-filesystem /tmp
   ```

2. **Verify environment variables**:
   ```bash
   echo $GITHUB_TOKEN
   ```

3. **Check client logs**:
   - Claude Desktop: View → Developer Tools → Console
   - Cursor: Developer Tools panel

4. **Test with minimal config**:
   ```json
   {
     "mcpServers": {
       "test": {
         "command": "npx",
         "args": ["-y", "@modelcontextprotocol/server-everything"]
       }
     }
   }
   ```

### 🐛 Common Error Messages

| Error | Cause | Solution |
|-------|-------|----------|
| `ENOENT: no such file` | Wrong path | Check file/directory exists |
| `401 Unauthorized` | Invalid credentials | Verify API key/token |
| `Connection refused` | Server crashed | Check logs, restart |
| `Timeout` | Slow network/server | Increase timeout or check connectivity |

---

## Production Considerations

### 🏢 Enterprise Deployment

For team-wide MCP deployment:

1. **Centralized Configuration**
   - Use a config management tool (Ansible, Puppet)
   - Template configs with environment-specific values

2. **Secret Management**
   - Integrate with your organization's secret manager
   - Rotate credentials regularly
   - Audit access

3. **Network Considerations**
   - MCP servers may need internet access
   - Configure proxies if behind a firewall
   - Consider remote MCP servers for shared resources

### 📈 Scaling

| Scenario | Recommendation |
|----------|----------------|
| Individual developer | Local servers, 3-6 |
| Small team | Shared remote servers |
| Enterprise | Managed MCP infrastructure |

### 🔒 Compliance

For regulated environments:

- **Logging**: Enable audit logging for all MCP operations
- **Data Residency**: Ensure servers don't send data to unauthorized regions
- **Access Control**: Use SSO/SAML where supported
- **Approval Workflows**: Review new servers before deployment

### 📊 Monitoring

Consider monitoring:

- Server uptime and health
- Request latency
- Error rates
- Resource consumption

---

## Quick Reference

### Essential Commands

```bash
# Test a server
npx @modelcontextprotocol/inspector <server-command>

# Validate JSON config
cat config.json | jq .

# Find running MCP processes
ps aux | grep mcp

# Check npm package info
npm info @modelcontextprotocol/server-filesystem
```

### Security Checklist

- [ ] No hardcoded secrets in config
- [ ] Environment variables for all credentials
- [ ] Minimal directory access for filesystem
- [ ] Read-only where possible
- [ ] Verified all server sources
- [ ] Regular credential rotation

### Performance Checklist

- [ ] Only essential servers enabled
- [ ] Unused servers commented out
- [ ] Regular client restarts
- [ ] Resource monitoring in place

---

**← Previous**: [Getting Started](getting-started.md) | **Next**: [Troubleshooting](troubleshooting.md) →
