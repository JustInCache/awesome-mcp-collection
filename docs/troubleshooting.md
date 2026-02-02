# Troubleshooting MCP Servers

A comprehensive guide to diagnosing and fixing common MCP issues.

## Table of Contents

- [Quick Diagnostics](#quick-diagnostics)
- [Installation Issues](#installation-issues)
- [Connection Issues](#connection-issues)
- [Authentication Issues](#authentication-issues)
- [Server-Specific Issues](#server-specific-issues)
- [Client-Specific Issues](#client-specific-issues)
- [Getting Help](#getting-help)

---

## Quick Diagnostics

### 5-Minute Checklist

Run through this checklist before diving deeper:

- [ ] **Node.js version**: `node --version` → Should be v18+
- [ ] **Config file exists**: Check the correct location for your client
- [ ] **Config is valid JSON**: `cat config.json | jq .`
- [ ] **Server runs manually**: `npx -y @modelcontextprotocol/server-<name>`
- [ ] **Client restarted**: Fully quit and reopen (not just close window)
- [ ] **Environment variables set**: `echo $YOUR_VAR`

### Diagnostic Commands

```bash
# Check Node.js
node --version
npm --version

# Check Python (for Python servers)
python3 --version
pip3 --version

# Check if npx works
npx -y @modelcontextprotocol/server-everything

# Validate JSON config
python3 -m json.tool /path/to/config.json

# Find running MCP processes
ps aux | grep -E "(mcp|npx.*server)" | grep -v grep
```

---

## Installation Issues

### "npx: command not found"

**Cause**: Node.js not installed or not in PATH.

**Solution**:
```bash
# Install Node.js via nvm (recommended)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
source ~/.zshrc  # or ~/.bashrc
nvm install 20
nvm use 20
```

### "Cannot find module"

**Cause**: Package not installed or npm cache corrupted.

**Solution**:
```bash
# Clear npm cache
npm cache clean --force

# Remove node_modules cache
rm -rf ~/.npm/_npx

# Try again
npx -y @modelcontextprotocol/server-filesystem /tmp
```

### "EACCES: permission denied"

**Cause**: npm trying to write to protected directory.

**Solution**:
```bash
# Fix npm permissions
mkdir -p ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.zshrc
source ~/.zshrc
```

### "uvx: command not found"

**Cause**: uv not installed (needed for Python servers).

**Solution**:
```bash
# Install uv
curl -LsSf https://astral.sh/uv/install.sh | sh
source ~/.zshrc  # or ~/.bashrc
```

---

## Connection Issues

### "Server failed to start"

**Cause**: Invalid configuration or missing dependencies.

**Diagnosis**:
```bash
# Test server directly
npx -y @modelcontextprotocol/server-github

# Check for error messages in output
```

**Common Fixes**:

1. **Verify command path**:
   ```json
   {
     "command": "npx",  // Not "node" or full path
     "args": ["-y", "@modelcontextprotocol/server-github"]
   }
   ```

2. **Check args format**:
   ```json
   {
     "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path"]
     // ✅ Each argument is separate
   }
   ```
   Not:
   ```json
   {
     "args": ["-y @modelcontextprotocol/server-filesystem /path"]
     // ❌ All in one string
   }
   ```

### "Timeout waiting for server"

**Cause**: Server takes too long to start or hangs.

**Solution**:
```bash
# Test server startup time
time npx -y @modelcontextprotocol/server-github

# If slow, might be downloading packages
# First run is always slower
```

### "Connection reset"

**Cause**: Server crashed after starting.

**Diagnosis**:
```bash
# Run server with debug logging
DEBUG=* npx -y @modelcontextprotocol/server-github 2>&1 | head -100
```

---

## Authentication Issues

### "401 Unauthorized" / "403 Forbidden"

**Cause**: Invalid or missing API credentials.

**Diagnosis**:
```bash
# Check if environment variable is set
echo $GITHUB_TOKEN

# Check if it's exported
env | grep GITHUB
```

**Solution**:

1. **Generate a new token**:
   - GitHub: Settings → Developer settings → Personal access tokens
   - Make sure to grant required scopes

2. **Export the variable**:
   ```bash
   # Add to ~/.zshrc or ~/.bashrc
   export GITHUB_TOKEN="ghp_xxxxxxxxxxxx"
   source ~/.zshrc
   ```

3. **Restart your MCP client** (full restart, not just window close)

### "Token format invalid"

**Cause**: Token copied incorrectly or wrong format.

**Check Token Format**:

| Service | Token Format |
|---------|--------------|
| GitHub | `ghp_` or `github_pat_` prefix |
| GitLab | `glpat-` prefix |
| Slack | `xoxb-` prefix |
| Brave | 20+ character string |

### Environment Variable Not Working

**Cause**: Client not reading from shell environment.

**Solutions**:

1. **Use full expansion** in config:
   ```json
   {
     "env": {
       "GITHUB_TOKEN": "${GITHUB_TOKEN}"
     }
   }
   ```

2. **Launch client from terminal**:
   ```bash
   # macOS
   open -a "Claude"
   
   # This ensures shell environment is inherited
   ```

3. **Set in client config directly** (less secure):
   ```json
   {
     "env": {
       "GITHUB_TOKEN": "ghp_actual_token_here"
     }
   }
   ```

---

## Server-Specific Issues

### Filesystem Server

**"Path not allowed"**
```bash
# Check the allowed path in your config
# Make sure the path exists and is readable
ls -la /your/configured/path
```

**"File not found"**
```bash
# Verify the file exists
ls -la /full/path/to/file

# Check for symlinks
readlink -f /path/to/file
```

### GitHub Server

**"Bad credentials"**
```bash
# Test your token
curl -H "Authorization: token $GITHUB_TOKEN" https://api.github.com/user
```

**"Resource not accessible"**
- Check token scopes: `repo`, `read:org`, etc.
- For org repos, ensure org access is granted

### PostgreSQL Server

**"Connection refused"**
```bash
# Test connection directly
psql "$DATABASE_URL"

# Check if PostgreSQL is running
pg_isready
```

**"SSL required"**
```json
{
  "env": {
    "DATABASE_URL": "postgresql://user:pass@host/db?sslmode=require"
  }
}
```

### Brave Search Server

**"Invalid API key"**
- Get key from: https://api.search.brave.com/
- Free tier available (limited requests)

---

## Client-Specific Issues

### Claude Desktop

**Config Location**:
```bash
# macOS
~/Library/Application Support/Claude/claude_desktop_config.json

# Windows
%APPDATA%\Claude\claude_desktop_config.json
```

**View Logs**:
1. Open Claude Desktop
2. View → Developer → Developer Tools
3. Check Console tab for errors

**Full Restart**:
```bash
# macOS - ensure fully quit
pkill -f Claude
open -a Claude

# Windows
taskkill /f /im Claude.exe
start Claude
```

### Cursor

**Config Location**:
- Settings → Features → MCP → Edit Config

**View Logs**:
- Help → Toggle Developer Tools

**Common Issue**: Cursor may cache old configs
```bash
# Try clearing Cursor cache
rm -rf ~/Library/Application\ Support/Cursor/Cache
```

### VS Code + Continue

**Config Location**:
```
~/.continue/config.json
```

**Check Continue Logs**:
- View → Output → Select "Continue" from dropdown

---

## Advanced Debugging

### Use MCP Inspector

The official debugging tool:

```bash
# Install and run with any server
npx @modelcontextprotocol/inspector npx -y @modelcontextprotocol/server-github

# Opens web UI at http://localhost:5173
```

Features:
- List all available tools
- Test tool calls interactively
- View raw JSON-RPC messages
- Debug responses

### Enable Verbose Logging

Add debug environment variables:

```json
{
  "mcpServers": {
    "github": {
      "env": {
        "DEBUG": "mcp:*",
        "NODE_DEBUG": "mcp"
      }
    }
  }
}
```

### Capture Network Traffic

For remote servers:
```bash
# Use mitmproxy to inspect traffic
mitmproxy --mode reverse:http://localhost:3000 -p 8080
```

---

## Getting Help

### Before Asking for Help

1. **Collect information**:
   ```bash
   # System info
   uname -a
   node --version
   npm --version
   
   # Error messages (sanitize secrets!)
   # Relevant config (without tokens)
   ```

2. **Try the minimal config**:
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

3. **Search existing issues** on the server's GitHub repo

### Where to Get Help

| Resource | Best For |
|----------|----------|
| [GitHub Issues](https://github.com/modelcontextprotocol/servers/issues) | Bug reports |
| [MCP Discord](https://discord.gg/mcp) | Real-time help |
| [r/mcp](https://reddit.com/r/mcp) | Community discussion |
| [Stack Overflow](https://stackoverflow.com/questions/tagged/mcp) | Technical Q&A |

### How to Report Issues

Include:
1. **OS and version**: macOS 14.3, Windows 11, Ubuntu 22.04
2. **Client and version**: Claude Desktop 1.2.3, Cursor 0.45
3. **Server and version**: @modelcontextprotocol/server-github@1.0.0
4. **Configuration** (sanitized): Remove all tokens/secrets
5. **Steps to reproduce**: Exact steps that trigger the issue
6. **Expected vs actual**: What should happen vs what happens
7. **Error logs**: From client developer tools

---

## Quick Reference

### Error → Solution Table

| Error | Likely Cause | Quick Fix |
|-------|--------------|-----------|
| `command not found` | Missing dependency | Install Node.js/Python |
| `ENOENT` | Wrong path | Check file/dir exists |
| `401/403` | Bad credentials | Regenerate token |
| `Connection refused` | Server crashed | Check logs, restart |
| `Timeout` | Slow start | Wait, check network |
| `Invalid JSON` | Config syntax error | Validate with `jq` |
| `Module not found` | Corrupted cache | `npm cache clean --force` |

### Reset Everything

Nuclear option when nothing works:

```bash
# Clear npm cache
npm cache clean --force
rm -rf ~/.npm/_npx

# Reset client config (backup first!)
mv ~/.config/mcp ~/.config/mcp.bak

# Restart with fresh minimal config
```

---

**← Previous**: [Best Practices](best-practices.md) | **Next**: [Building Servers](building-servers.md) →
