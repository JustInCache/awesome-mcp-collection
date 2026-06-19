[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/justincache-awesome-mcp-collection-badge.png)](https://mseep.ai/app/justincache-awesome-mcp-collection)

# 🔌 Awesome MCP Servers

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Last Updated](https://img.shields.io/badge/Updated-February%202026-blue.svg)](#)

> **The most comprehensive, curated collection of Model Context Protocol (MCP) servers.**
> 
> Unlike other lists, we focus on **quality over quantity** — featuring battle-tested, actively maintained servers with real-world configurations and developer-focused organization.

<p align="center">
  <a href="#-quick-start">Quick Start</a> •
  <a href="#-official-servers">Official Servers</a> •
  <a href="#-by-category">By Category</a> •
  <a href="#-by-stack">By Stack</a> •
  <a href="#-configurations">Configurations</a> •
  <a href="#-contributing">Contributing</a>
</p>

---

## 📖 What is MCP?

**Model Context Protocol (MCP)** is an open standard that enables AI assistants to securely connect with external tools, data sources, and services. Think of it as a **universal plugin system for AI** — allowing Claude, Cursor, VS Code Copilot, and other AI tools to interact with your development environment, databases, APIs, and more.

### Why MCP Matters

| Before MCP | With MCP |
|------------|----------|
| Copy-paste context manually | AI reads your codebase directly |
| Switch between tools constantly | Unified interface for everything |
| Limited to training data | Real-time access to live data |
| Generic responses | Context-aware, personalized help |

---

## ⚡ Quick Start

### 1. Choose Your Client

| Client | Platform | Setup Guide |
|--------|----------|-------------|
| **Claude Desktop** | macOS, Windows | [Official Guide](https://modelcontextprotocol.io/quickstart) |
| **Cursor** | macOS, Windows, Linux | [Cursor MCP Docs](https://docs.cursor.com/context/model-context-protocol) |
| **VS Code + Continue** | All platforms | [Continue Extension](https://continue.dev/docs) |
| **Windsurf** | macOS, Windows, Linux | [Windsurf Docs](https://docs.codeium.com/windsurf) |

### 2. Install Your First Server

```bash
# Using npx (recommended for TypeScript servers)
npx -y @modelcontextprotocol/server-filesystem /path/to/allowed/directory

# Using uvx (recommended for Python servers)
uvx mcp-server-git --repository /path/to/repo
```

### 3. Configure Your Client

Add to your client's configuration file:

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/Users/you/projects"]
    }
  }
}
```

📁 **Ready-to-use configs:** Check our [configs/](configs/) directory for complete setups by use case.

---

## 🎖️ Official Servers

These are **reference implementations** maintained by Anthropic and the MCP team. Start here.

| Server | Package | Description | Transport |
|--------|---------|-------------|-----------|
| **Filesystem** | `@modelcontextprotocol/server-filesystem` | Secure file operations with configurable access controls | stdio |
| **Fetch** | `@modelcontextprotocol/server-fetch` | HTTP client for web requests, returns markdown/JSON | stdio |
| **Git** | `@modelcontextprotocol/server-git` | Local Git repository operations and analysis | stdio |
| **Memory** | `@modelcontextprotocol/server-memory` | Persistent knowledge graph for cross-session context | stdio |
| **PostgreSQL** | `@modelcontextprotocol/server-postgres` | Read-only database access with schema inspection | stdio |
| **SQLite** | `@modelcontextprotocol/server-sqlite` | SQLite database operations with built-in analysis | stdio |
| **Brave Search** | `@modelcontextprotocol/server-brave-search` | Web search via Brave's privacy-focused API | stdio |
| **Puppeteer** | `@modelcontextprotocol/server-puppeteer` | Browser automation via Chrome DevTools Protocol | stdio |
| **Sequential Thinking** | `@modelcontextprotocol/server-sequentialthinking` | Dynamic multi-step reasoning with thought chaining | stdio |
| **Slack** | `@modelcontextprotocol/server-slack` | Slack workspace messaging and channel management | stdio |
| **Google Drive** | `@modelcontextprotocol/server-gdrive` | Google Drive file access and search | stdio |
| **Google Maps** | `@modelcontextprotocol/server-google-maps` | Location services, routing, and place details | stdio |
| **Sentry** | `@modelcontextprotocol/server-sentry` | Error tracking and performance monitoring | stdio |
| **Everything** | `@modelcontextprotocol/server-everything` | Reference implementation exercising all MCP features | stdio |

> 📦 **Install any official server:** `npx -y @modelcontextprotocol/server-<name>`

---

## 📂 By Category

### 🔧 Development & Version Control

Essential tools for software development workflows.

| Server | Repo | Stars | Lang | Description |
|--------|------|-------|------|-------------|
| **GitHub** 🎖️ | [github/github-mcp-server](https://github.com/github/github-mcp-server) | ⭐ 15k+ | TS | **The gold standard.** Full GitHub integration: repos, issues, PRs, Actions, code scanning. OAuth + remote MCP support. |
| **GitLab** | [modelcontextprotocol/server-gitlab](https://github.com/modelcontextprotocol/servers) | Official | TS | GitLab platform integration for projects, MRs, and CI/CD |
| **Playwright** 🎖️ | [microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp) | ⭐ 11k+ | TS | **Microsoft Official.** Browser automation using accessibility snapshots for reliability |
| **JetBrains** | [jetbrains/mcpProxy](https://github.com/JetBrains/mcpProxy) | ⭐ 2k+ | TS | Connect to IntelliJ, PyCharm, WebStorm, and all JetBrains IDEs |
| **Xcode** | [r-huijts/xcode-mcp-server](https://github.com/r-huijts/xcode-mcp-server) | 🍎 | TS | iOS/macOS development: project management, builds, simulators |
| **Code Executor** | [pydantic/mcp-run-python](https://github.com/pydantic/pydantic-ai) | Official | Python | Execute Python code safely in isolated sandboxes |
| **Docker** | [QuantGeekDev/docker-mcp](https://github.com/QuantGeekDev/docker-mcp) | ⭐ 500+ | Go | Container management, image operations, compose support |

### ☁️ Cloud & Infrastructure

Deploy, manage, and monitor cloud resources.

| Server | Repo | Stars | Lang | Description |
|--------|------|-------|------|-------------|
| **AWS** 🎖️ | [awslabs/mcp](https://github.com/awslabs/mcp) | ⭐ 4k+ | TS/Python | **AWS Official.** 20+ servers for S3, Lambda, DynamoDB, CloudWatch, and more |
| **Azure** 🎖️ | [Azure/azure-mcp](https://github.com/Azure/azure-mcp) | ⭐ 1k+ | TS | **Microsoft Official.** Storage, Cosmos DB, Monitor, DevOps integration |
| **GCP** | [googleapis/genai-toolbox](https://github.com/googleapis/genai-toolbox) | ⭐ 500+ | Go | Google Cloud database and service integration |
| **Kubernetes** | [strowk/mcp-k8s-go](https://github.com/strowk/mcp-k8s-go) | ⭐ 800+ | Go | Multi-cluster K8s management with 50+ tools |
| **Terraform** 🎖️ | [hashicorp/terraform-mcp-server](https://github.com/hashicorp/terraform-mcp-server) | ⭐ 2k+ | Go | **HashiCorp Official.** Registry, providers, modules, IaC generation |
| **Cloudflare** 🎖️ | [cloudflare/mcp-server-cloudflare](https://github.com/cloudflare/mcp-server-cloudflare) | ⭐ 1k+ | TS | Workers, KV, R2, D1, and edge infrastructure |
| **Pulumi** 🎖️ | [pulumi/mcp-server](https://github.com/pulumi/mcp-server) | ⭐ 300+ | TS | Infrastructure as Code operations and Cloud API |

### 💾 Databases

Query, manage, and analyze your data.

| Server | Repo | Stars | Lang | Description |
|--------|------|-------|------|-------------|
| **PostgreSQL** 🎖️ | [modelcontextprotocol/server-postgres](https://github.com/modelcontextprotocol/servers) | Official | TS | Read-only access with schema inspection |
| **MySQL** | [designcomputer/mysql_mcp_server](https://github.com/designcomputer/mysql_mcp_server) | ⭐ 400+ | Python | Full MySQL support with configurable permissions |
| **MongoDB** | [mongodb-js/mongodb-mcp-server](https://github.com/mongodb-js/mongodb-mcp-server) | ⭐ 600+ | TS | Atlas support, document operations, aggregations |
| **Supabase** 🎖️ | [supabase-community/supabase-mcp](https://github.com/supabase-community/supabase-mcp) | ⭐ 1k+ | TS | **Official.** Database, Auth, Edge Functions, Realtime |
| **Redis** 🎖️ | [redis/mcp-redis](https://github.com/redis/mcp-redis) | ⭐ 200+ | Python | Official Redis integration with search capabilities |
| **Neon** 🎖️ | [neondatabase/mcp-server-neon](https://github.com/neondatabase/mcp-server-neon) | ⭐ 300+ | TS | Serverless Postgres with branching support |
| **Prisma** 🎖️ | [prisma/mcp](https://github.com/prisma/mcp) | ⭐ 500+ | TS | Prisma Postgres, migrations, queries |
| **ClickHouse** 🎖️ | [ClickHouse/mcp-clickhouse](https://github.com/ClickHouse/mcp-clickhouse) | ⭐ 200+ | Python | Analytics database integration |
| **Pinecone** 🎖️ | [pinecone-io/assistant-mcp](https://github.com/pinecone-io/assistant-mcp) | ⭐ 300+ | Rust | Vector database for RAG applications |
| **Qdrant** 🎖️ | [qdrant/mcp-server-qdrant](https://github.com/qdrant/mcp-server-qdrant) | ⭐ 200+ | Python | Vector search and semantic memory |

### 🌐 Web & Search

Access the web, scrape content, and search the internet.

| Server | Repo | Stars | Lang | Description |
|--------|------|-------|------|-------------|
| **Brave Search** 🎖️ | [modelcontextprotocol/server-brave-search](https://github.com/modelcontextprotocol/servers) | Official | TS | Privacy-focused web search |
| **Exa** 🎖️ | [exa-labs/exa-mcp-server](https://github.com/exa-labs/exa-mcp-server) | ⭐ 1k+ | TS | AI-native web search with semantic understanding |
| **Firecrawl** | [firecrawl-mcp](https://docs.firecrawl.dev/mcp) | ⭐ 800+ | TS | Web scraping with dynamic content support |
| **Browserbase** 🎖️ | [browserbase/mcp-server-browserbase](https://github.com/browserbase/mcp-server-browserbase) | ⭐ 500+ | TS | Cloud browser automation with stealth mode |
| **Bright Data** 🎖️ | [luminati-io/brightdata-mcp](https://github.com/luminati-io/brightdata-mcp) | ⭐ 400+ | TS | Enterprise web scraping and data extraction |
| **Perplexity** | [tanigami/mcp-server-perplexity](https://github.com/tanigami/mcp-server-perplexity) | ⭐ 200+ | Python | Real-time web search with citations |
| **Apify** 🎖️ | [apify/actors-mcp-server](https://github.com/apify/actors-mcp-server) | ⭐ 300+ | TS | 3,000+ pre-built web scrapers |
| **Helium** | [connerlambden/helium-mcp](https://github.com/connerlambden/helium-mcp) | New | Python | Real-time news with bias scoring across 5,000+ sources, AI-powered options pricing, and live market data |

### 📊 Productivity & Collaboration

Connect to your daily tools.

| Server | Repo | Stars | Lang | Description |
|--------|------|-------|------|-------------|
| **Slack** 🎖️ | [modelcontextprotocol/server-slack](https://github.com/modelcontextprotocol/servers) | Official | TS | Messaging, channels, search |
| **Notion** | [makenotion/notion-mcp-server](https://github.com/notionhq/notion-mcp-server) | ⭐ 800+ | TS | Databases, pages, content management |
| **Linear** 🎖️ | [linear/mcp-server-linear](https://github.com/linear/linear-mcp-server) | ⭐ 400+ | TS | Issue tracking and project management |
| **Jira** | [sooperset/mcp-atlassian](https://github.com/sooperset/mcp-atlassian) | ⭐ 500+ | Python | Issues, projects, workflows, JQL search |
| **Confluence** | [sooperset/mcp-atlassian](https://github.com/sooperset/mcp-atlassian) | ⭐ 500+ | Python | Documentation and knowledge base |
| **Obsidian** | [calclavia/mcp-obsidian](https://github.com/calclavia/mcp-obsidian) | ⭐ 600+ | TS | Vault access, note management, search |
| **Google Workspace** | [taylorwilsdon/google_workspace_mcp](https://github.com/taylorwilsdon/google_workspace_mcp) | ⭐ 300+ | Python | Calendar, Drive, Gmail, Docs, Sheets |
| **Todoist** | Various | Community | TS | Task management integration |

### 💰 Finance & Fintech

Handle payments, trading, and financial data.

| Server | Repo | Stars | Lang | Description |
|--------|------|-------|------|-------------|
| **Stripe** 🎖️ | [stripe/agent-toolkit](https://github.com/stripe/agent-toolkit) | ⭐ 800+ | Python/TS | Payments, subscriptions, invoices |
| **Polygon.io** 🎖️ | [polygon-io/mcp_polygon](https://github.com/polygon-io/mcp_polygon) | ⭐ 200+ | Python | Stock, forex, and crypto market data |
| **Alpaca** | [laukikk/alpaca-mcp](https://github.com/laukikk/alpaca-mcp) | ⭐ 100+ | Python | Stock trading and portfolio management |
| **CoinGecko** | Community | Various | TS | Cryptocurrency market data |
| **Yahoo Finance** | [narumiruna/yfinance-mcp](https://github.com/narumiruna/yfinance-mcp) | ⭐ 100+ | Python | Stock data and financial analysis |

### 🔒 Security & DevSecOps

Scan, audit, and secure your applications.

| Server | Repo | Stars | Lang | Description |
|--------|------|-------|------|-------------|
| **Snyk** 🎖️ | [snyk/studio-mcp](https://github.com/snyk/mcp-snyk) | ⭐ 300+ | TS | Vulnerability scanning for AI-generated code |
| **Semgrep** 🎖️ | [semgrep/mcp](https://github.com/semgrep/mcp) | ⭐ 200+ | TS | Static analysis and security scanning |
| **GitGuardian** | [gitguardian/mcp-server](https://docs.gitguardian.com/) | Official | Python | Secret detection with 500+ detectors |
| **VirusTotal** | [BurtTheCoder/mcp-virustotal](https://github.com/BurtTheCoder/mcp-virustotal) | ⭐ 100+ | TS | URL/file scanning and threat analysis |
| **Shodan** | [BurtTheCoder/mcp-shodan](https://github.com/BurtTheCoder/mcp-shodan) | ⭐ 100+ | TS | Internet-connected device search |

### 🤖 AI & ML

Enhance AI capabilities and integrate with ML platforms.

| Server | Repo | Stars | Lang | Description |
|--------|------|-------|------|-------------|
| **OpenAI** | [mzxrai/mcp-openai](https://github.com/mzxrai/mcp-openai) | ⭐ 400+ | TS | Chat with GPT models directly |
| **LangFuse** 🎖️ | [langfuse/mcp-server-langfuse](https://github.com/langfuse/langfuse) | ⭐ 300+ | Python | LLM observability and prompt management |
| **Hugging Face** | [evalstate/mcp-hfspace](https://github.com/evalstate/mcp-hfspace) | ⭐ 200+ | TS | Access HF Spaces and models |
| **Replicate** | [awkoy/replicate-flux-mcp](https://github.com/awkoy/replicate-flux-mcp) | ⭐ 100+ | TS | Image generation via Replicate API |
| **Context7** 🎖️ | [upstash/context7](https://github.com/upstash/context7) | ⭐ 500+ | TS | Up-to-date documentation for LLMs |
| **NotFair** | [nowork-studio/NotFair](https://github.com/nowork-studio/NotFair) | ⭐ 2.9k+ | TS | Claude Code agent skills for SEO, Google Ads, and Meta Ads; connects via Google Ads MCP, Meta Ads MCP, Google Search Console MCP, and Google Analytics (GA4) MCP |

### 📱 Platform-Specific

Mobile, desktop, and specialized platforms.

| Server | Repo | Stars | Lang | Description |
|--------|------|-------|------|-------------|
| **iOS Simulator** | [mobile-next/mobile-mcp](https://github.com/mobile-next/mobile-mcp) | ⭐ 300+ | TS | iOS/Android automation and testing |
| **Android** | [hyperb1iss/droidmind](https://github.com/hyperb1iss/droidmind) | ⭐ 200+ | Python | Android device control via ADB |
| **Home Assistant** | [allenporter/mcp-server-home-assistant](https://github.com/allenporter/mcp-server-home-assistant) | ⭐ 400+ | Python | Smart home automation |
| **Blender** | [ahujasid/blender-mcp](https://github.com/ahujasid/blender-mcp) | ⭐ 600+ | Python | 3D modeling and animation |
| **Unity** | [CoderGamester/mcp-unity](https://github.com/CoderGamester/mcp-unity) | ⭐ 200+ | C# | Game development integration |

---

## 👨‍💻 By Stack

Choose the right servers based on your tech stack.

### Full-Stack JavaScript/TypeScript Developer

```json
{
  "mcpServers": {
    "github": { "command": "npx", "args": ["-y", "@modelcontextprotocol/server-github"] },
    "filesystem": { "command": "npx", "args": ["-y", "@modelcontextprotocol/server-filesystem", "."] },
    "postgres": { "command": "npx", "args": ["-y", "@modelcontextprotocol/server-postgres"] },
    "memory": { "command": "npx", "args": ["-y", "@modelcontextprotocol/server-memory"] }
  }
}
```

📁 See: [configs/fullstack-js.json](configs/fullstack-js.json)

### Python/Data Science Developer

```json
{
  "mcpServers": {
    "filesystem": { "command": "npx", "args": ["-y", "@modelcontextprotocol/server-filesystem", "."] },
    "jupyter": { "command": "uvx", "args": ["mcp-server-jupyter"] },
    "postgres": { "command": "npx", "args": ["-y", "@modelcontextprotocol/server-postgres"] },
    "memory": { "command": "npx", "args": ["-y", "@modelcontextprotocol/server-memory"] }
  }
}
```

📁 See: [configs/python-datascience.json](configs/python-datascience.json)

### DevOps/Platform Engineer

```json
{
  "mcpServers": {
    "github": { "command": "npx", "args": ["-y", "@modelcontextprotocol/server-github"] },
    "kubernetes": { "command": "mcp-k8s-go" },
    "terraform": { "command": "terraform-mcp-server" },
    "docker": { "command": "docker-mcp" }
  }
}
```

📁 See: [configs/devops.json](configs/devops.json)

### More Stacks

| Stack | Config File | Servers Included |
|-------|-------------|------------------|
| Frontend React/Vue | [configs/frontend.json](configs/frontend.json) | GitHub, Filesystem, Figma, Playwright |
| Mobile iOS/Android | [configs/mobile.json](configs/mobile.json) | Xcode, Simulator, Filesystem |
| Security Engineer | [configs/security.json](configs/security.json) | Snyk, Semgrep, Shodan, VirusTotal |
| Startup Founder | [configs/startup.json](configs/startup.json) | Stripe, Supabase, Linear, Slack |

---

## ⚙️ Configurations

Ready-to-use configuration files for popular setups.

| Config | Description | Download |
|--------|-------------|----------|
| `minimal.json` | Essential servers for any developer | [📥](configs/minimal.json) |
| `fullstack-js.json` | Complete JS/TS development stack | [📥](configs/fullstack-js.json) |
| `python-datascience.json` | Data science and ML workflows | [📥](configs/python-datascience.json) |
| `devops.json` | Infrastructure and DevOps tools | [📥](configs/devops.json) |
| `enterprise.json` | Enterprise-grade with security | [📥](configs/enterprise.json) |

### Configuration Locations

| Client | Config Path |
|--------|-------------|
| Claude Desktop (macOS) | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Claude Desktop (Windows) | `%APPDATA%\Claude\claude_desktop_config.json` |
| Cursor | Settings → MCP → Edit Config |
| VS Code + Continue | `.continue/config.json` |

---

## 📚 Documentation

| Guide | Description |
|-------|-------------|
| [Getting Started](docs/getting-started.md) | Complete setup guide for beginners |
| [Best Practices](docs/best-practices.md) | Security, performance, and usage tips |
| [Troubleshooting](docs/troubleshooting.md) | Common issues and solutions |
| [Building Servers](docs/building-servers.md) | Create your own MCP server |

---

## 🏆 Hall of Fame

Most impactful MCP servers by category.

| Category | Server | Why It's Essential |
|----------|--------|-------------------|
| 🥇 Version Control | GitHub | Complete GitHub integration with OAuth |
| 🥇 Browser Automation | Playwright | Microsoft-backed reliability |
| 🥇 Cloud | AWS Labs | Official AWS with 20+ services |
| 🥇 Database | Supabase | Full backend-as-a-service |
| 🥇 Search | Exa | AI-native semantic search |
| 🥇 Security | Snyk | Real-time vulnerability scanning |

---

## 🤝 Contributing

We welcome contributions! Please read our [Contributing Guidelines](CONTRIBUTING.md) before submitting.

### Quick Contribution

1. Fork this repository
2. Add your server to the appropriate category
3. Include: name, repo URL, language, and description
4. Submit a Pull Request

### Submission Criteria

- ✅ Open source with active maintenance
- ✅ Clear documentation and setup instructions
- ✅ Working installation (tested)
- ✅ Follows MCP specification
- ❌ No abandoned projects (6+ months inactive)
- ❌ No malicious or insecure implementations

---

## ☕ Support

If this repo saved you time or sparked an idea, consider buying me a coffee!

<a href="https://buymeacoffee.com/connectankush">
  <img src="https://img.shields.io/badge/Buy%20Me%20A%20Coffee-FFDD00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black" alt="Buy Me a Coffee">
</a>

---

## 📈 Stats

| Metric | Count |
|--------|-------|
| Total Servers Listed | 100+ |
| Official Servers | 15 |
| Categories | 12 |
| Configuration Templates | 10 |
| Last Updated | February 2026 |

---

## ⭐ Star History

If this list helped you, please give it a star! It helps others discover these resources.

---

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🔗 Resources

- [Official MCP Documentation](https://modelcontextprotocol.io)
- [MCP Specification](https://spec.modelcontextprotocol.io)
- [Official Servers Repository](https://github.com/modelcontextprotocol/servers)
- [MCP TypeScript SDK](https://github.com/modelcontextprotocol/typescript-sdk)
- [MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk)

---

<p align="center">
  Made with ❤️ by the community
  <br>
  <a href="https://github.com/JustInCache/awesome-mcp-collection/stargazers">⭐ Star</a> •
  <a href="https://github.com/JustInCache/awesome-mcp-collection/fork">🍴 Fork</a> •
  <a href="https://github.com/JustInCache/awesome-mcp-collection/issues">🐛 Issues</a>
</p>

> Last updated: 2026-06-18 14:53 UTC
