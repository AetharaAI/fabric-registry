# Fabric A2A Registry

<p align="center">
  <img src="images/logo.png" alt="Fabric Logo" width="250"/>
</p>

# Fabric MCP Server - Universal Tool Server & Agent Gateway
*Weaving Agents and Tools into a Unified Protocol Fabric*

A production-grade MCP server that serves as both a **Universal Tool Server** and an **Agent-to-Agent Communication Gateway**. Fabric provides 20+ built-in tools plus the ability to route calls between agents, all using the Model Context Protocol (MCP) as the interface.

## Overview

**Fabric** is two things in one:
...

The canonical, community-maintained registry for Fabric Agent-to-Agent (A2A) nodes, agents, and tools.

## What This Is

- **A declaration layer** - Machine-readable manifests describing agents and their capabilities
- **No execution** - This registry does not run agents or tools
- **No secrets** - No API keys, credentials, or sensitive data
- **No auth logic** - Authentication is handled by the hosting infrastructure

## What This Is Not

- **Not a runtime** - Agents run on their own infrastructure
- **Not a hosting service** - We don't execute or proxy agent calls
- **Not a gatekeeper** - Open registry with community governance

## Quick Start

### For Clients

```bash
# Fetch the registry index
curl https://registry.mcpfabric.space/index.json

# Get agent details
curl https://registry.mcpfabric.space/agents/percy.json

# Get full agent spec
curl https://registry.mcpfabric.space/agents/percy.yaml
```

### Using the Python SDK

```python
from fabric_a2a import FabricClient

client = FabricClient(registry_url="https://registry.mcpfabric.space")

# List available agents
agents = client.registry.list_agents()

# Get agent capabilities
percy = client.registry.get_agent("percy")
print(percy.capabilities)
```

## Structure

```
.
├── index.json              # Registry entrypoint
├── schema/
│   ├── agent.schema.json   # Agent manifest schema
│   ├── tool.schema.json    # Tool manifest schema
│   └── registry.schema.json # Registry index schema
├── agents/
│   ├── percy.yaml          # Full agent spec
│   ├── percy.json          # Minimal metadata
│   ├── coder.yaml
│   ├── coder.json
│   └── ...
└── tools/
    ├── io.yaml             # Tool category spec
    ├── io.json             # Category metadata
    ├── web.yaml
    ├── web.json
    └── ...
```

### File Formats

- **YAML files** - Full specifications with input/output schemas, timeouts, etc.
- **JSON files** - Minimal metadata for quick discovery
- **index.json** - Registry entrypoint listing all resources

## Available Agents

| Agent | Description | Trust Tier | Capabilities |
|-------|-------------|------------|--------------|
| `percy` | General reasoning and planning | `org` | `reason`, `plan` |
| `coder` | Code generation and review | `org` | `code`, `review` |
| `vision` | Image analysis and generation | `org` | `analyze_image`, `generate_image` |
| `memory` | Long-term knowledge storage | `local` | `store`, `recall` |
| `orchestrator` | Multi-agent coordination | `org` | `coordinate` |

## Available Tools

| Category | Tools | Trust Tier |
|----------|-------|------------|
| `io` | File read/write, directory listing, search | `org` |
| `web` | HTTP requests, page fetching, URL parsing | `org` |
| `math` | Calculator, statistics | `public` |
| `text` | Regex, transform, diff | `public` |
| `system` | Command execution, environment, datetime | `local` |
| `data` | JSON, CSV, schema validation | `public` |
| `security` | Hash, base64 | `public` |
| `encode` | URL encoding | `public` |
| `docs` | Markdown processing | `public` |

## Trust Tiers

- **`local`** - Only available on local/private deployments (e.g., command execution)
- **`org`** - Available within organization boundaries
- **`public`** - Available to any client

## How to Contribute

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on adding agents and tools.

## Schema Validation

All manifests are validated against JSON Schema:

```bash
# Validate with ajv (npm install -g ajv-cli)
ajv validate -s schema/agent.schema.json -d agents/percy.yaml

# Or using Python
python -c "import yaml, jsonschema; ..."
```

## Hosting

This registry is designed to be hosted on GitHub Pages or any static file host.

## License

MIT - See [LICENSE](../LICENSE) for details.

---

**Version**: 1.0  
**Status**: Reference Implementation
