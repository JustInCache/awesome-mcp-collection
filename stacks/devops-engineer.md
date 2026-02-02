# DevOps Engineer Stack

Essential MCP servers for DevOps, SRE, and Platform Engineering workflows.

## Overview

| Focus | Servers | Setup Time |
|-------|---------|------------|
| Infrastructure | 8 servers | ~15 minutes |
| Cloud, K8s, IaC | AWS, GCP, Azure, K8s | Advanced |

---

## Recommended Servers

### Infrastructure as Code

| Server | Purpose | Priority |
|--------|---------|----------|
| **Terraform** | IaC registry and modules | ⭐⭐⭐ |
| **Pulumi** | Alternative IaC | ⭐⭐ |
| **Filesystem** | Read IaC files | ⭐⭐⭐ |

### Cloud Providers

| Server | Purpose | Priority |
|--------|---------|----------|
| **AWS** | EC2, S3, Lambda, CloudWatch | ⭐⭐⭐ |
| **Azure** | Azure services | ⭐⭐ |
| **GCP** | Google Cloud | ⭐⭐ |
| **Cloudflare** | Edge, Workers, CDN | ⭐⭐ |

### Container Orchestration

| Server | Purpose | Priority |
|--------|---------|----------|
| **Kubernetes** | Multi-cluster K8s management | ⭐⭐⭐ |
| **Docker** | Container operations | ⭐⭐⭐ |

### Monitoring & Observability

| Server | Purpose | Priority |
|--------|---------|----------|
| **Sentry** | Error tracking | ⭐⭐ |
| **Grafana** | Dashboards and metrics | ⭐⭐ |

### Version Control

| Server | Purpose | Priority |
|--------|---------|----------|
| **GitHub** | GitOps workflows | ⭐⭐⭐ |

---

## Quick Setup

### 1. Copy Configuration

Download: [devops.json](../configs/devops.json)

Or copy manually:

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "${HOME}/infrastructure"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "kubernetes": {
      "command": "mcp-k8s-go",
      "args": []
    },
    "aws": {
      "command": "npx",
      "args": ["-y", "@aws/mcp-server-aws"],
      "env": {
        "AWS_PROFILE": "${AWS_PROFILE:-default}",
        "AWS_REGION": "${AWS_REGION:-us-east-1}"
      }
    },
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    }
  }
}
```

### 2. Prerequisites

```bash
# Install Kubernetes MCP server
go install github.com/strowk/mcp-k8s-go@latest

# Ensure kubectl is configured
kubectl config current-context

# AWS CLI configured
aws sts get-caller-identity
```

### 3. Set Environment Variables

```bash
export GITHUB_TOKEN="ghp_xxxxxxxxxxxx"
export AWS_PROFILE="production"
export AWS_REGION="us-west-2"
```

---

## Use Cases

### "Debug why pods are crashing"

The AI can:
- List pods and their status (Kubernetes)
- Get pod logs and events (Kubernetes)
- Check recent deployments (GitHub)
- Analyze error patterns (Memory)

### "Set up a new EKS cluster"

The AI can:
- Find Terraform modules (Terraform)
- Generate IaC code (Filesystem)
- Review AWS best practices (AWS)
- Create PR with changes (GitHub)

### "Investigate a production incident"

The AI can:
- Check error tracking (Sentry)
- Query metrics (Grafana)
- Review recent changes (GitHub)
- Inspect running containers (Docker/K8s)

---

## Tips

1. **Use read-only permissions** for production clusters
2. **Configure multiple AWS profiles** for different environments
3. **Enable Kubernetes server** only when actively working with clusters
4. **Memory server** tracks infrastructure context across sessions

---

## Related Stacks

- [Security Engineer](security-engineer.md) - Security focus
- [Full-Stack Developer](fullstack-developer.md) - Application development
