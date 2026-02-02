# Contributing to Awesome MCP Servers

First off, thank you for considering contributing! This collection thrives because of community members like you.

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How to Contribute](#how-to-contribute)
- [Submission Guidelines](#submission-guidelines)
- [Quality Standards](#quality-standards)
- [Pull Request Process](#pull-request-process)

---

## Code of Conduct

By participating in this project, you agree to maintain a respectful and inclusive environment. Be kind, be helpful, and be constructive.

---

## How to Contribute

### 🆕 Adding a New Server

1. **Fork** this repository
2. **Check** if the server already exists (use search)
3. **Add** your server to the appropriate category in `README.md`
4. **Follow** the formatting guidelines below
5. **Submit** a Pull Request

### 🐛 Reporting Issues

- Found a broken link? [Open an issue](../../issues/new)
- Server no longer maintained? [Open an issue](../../issues/new)
- Incorrect information? [Open an issue](../../issues/new)

### 📝 Improving Documentation

- Fix typos or clarify instructions
- Add configuration examples
- Improve category descriptions
- Add troubleshooting tips

---

## Submission Guidelines

### Required Information

Every server submission must include:

| Field | Required | Example |
|-------|----------|---------|
| Name | ✅ | GitHub MCP Server |
| Repository URL | ✅ | `[github/github-mcp-server](https://github.com/...)` |
| Language | ✅ | TS, Python, Go, Rust, C# |
| Description | ✅ | 50-100 characters describing the server |
| Stars (if notable) | 🟡 | ⭐ 5k+ |
| Official badge | 🟡 | 🎖️ (for official implementations) |

### Formatting Template

```markdown
| **Server Name** | [org/repo](https://github.com/org/repo) | ⭐ Xk+ | Lang | Brief description (50-100 chars) |
```

### Examples

**Good:**
```markdown
| **GitHub** 🎖️ | [github/github-mcp-server](https://github.com/github/github-mcp-server) | ⭐ 15k+ | TS | Full GitHub integration: repos, issues, PRs, Actions |
```

**Bad:**
```markdown
| GitHub | github.com/github/github-mcp-server | many stars | TypeScript | This is a server for GitHub that does many things including managing repositories and issues and pull requests and also actions and code scanning |
```

---

## Quality Standards

### ✅ Acceptance Criteria

Your submission will be accepted if:

- [ ] **Open Source**: The server is publicly available on GitHub/GitLab
- [ ] **Active**: Last commit within 6 months (exceptions for stable, mature projects)
- [ ] **Documented**: Has a README with installation instructions
- [ ] **Working**: Can be installed and used following the documentation
- [ ] **MCP Compliant**: Follows the Model Context Protocol specification
- [ ] **Unique**: Doesn't duplicate existing functionality without significant improvement

### ❌ Rejection Criteria

Your submission will be rejected if:

- Server repository is private or inaccessible
- No documentation or installation instructions
- Project appears abandoned (6+ months, no issues addressed)
- Security concerns (hardcoded secrets, known vulnerabilities)
- Doesn't implement MCP protocol correctly
- Spam or self-promotion without substance

### 🎖️ Official Badge Criteria

Only these servers receive the 🎖️ official badge:

- Maintained by Anthropic/MCP team
- Maintained by the service provider (e.g., AWS, Microsoft, Stripe)
- Officially endorsed in the service's documentation

---

## Pull Request Process

### 1. Create Your PR

```bash
# Fork and clone
git clone https://github.com/YOUR_USERNAME/awesome-mcp-collection.git
cd awesome-mcp-collection

# Create a branch
git checkout -b add-server-name

# Make your changes
# ... edit README.md ...

# Commit with a clear message
git commit -m "Add ServerName - brief description"

# Push and create PR
git push origin add-server-name
```

### 2. PR Title Format

```
Add: ServerName - Category
```

Examples:
- `Add: MongoDB Atlas - Databases`
- `Add: Vercel MCP - Cloud & Infrastructure`
- `Fix: Update GitHub server description`
- `Docs: Add troubleshooting guide`

### 3. PR Description Template

```markdown
## Server Information

- **Name**: 
- **Repository**: 
- **Category**: 
- **Language**: 

## Checklist

- [ ] Server is open source
- [ ] README exists with installation instructions
- [ ] I've tested the server works
- [ ] Added to correct category
- [ ] Follows formatting guidelines

## Why This Server?

Brief explanation of why this server is valuable...
```

### 4. Review Process

1. Maintainers will review within 7 days
2. You may be asked to make changes
3. Once approved, your PR will be merged
4. Your contribution will be credited

---

## Category Guidelines

### When to Create a New Category

- At least 5+ servers would fit the category
- Doesn't overlap significantly with existing categories
- Has clear, distinct use cases

### Proposing New Categories

1. Open an issue titled "Proposal: New Category - [Name]"
2. List at least 5 servers that would fit
3. Explain why existing categories don't work
4. Community discussion for 7 days
5. Maintainer decision

---

## Configuration Templates

When adding configuration examples:

### Format

```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "@scope/package-name"],
      "env": {
        "API_KEY": "${ENVIRONMENT_VARIABLE}"
      }
    }
  }
}
```

### Guidelines

- Use environment variables for secrets (`${VAR_NAME}`)
- Include comments for non-obvious configurations
- Test the configuration works before submitting
- Provide both minimal and full configuration examples

---

## Recognition

Contributors are recognized in several ways:

- **All Contributors**: Listed in GitHub contributors
- **Major Contributors**: Mentioned in README acknowledgments
- **Regular Contributors**: Invited as repository collaborators

---

## Questions?

- Open a [Discussion](../../discussions) for general questions
- Open an [Issue](../../issues) for specific problems
- Tag maintainers for urgent matters

---

Thank you for helping make this the best MCP server collection! 🙏
