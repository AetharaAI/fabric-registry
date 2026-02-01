# Contributing to Fabric A2A Registry

Thank you for your interest in contributing to the Fabric A2A Registry! This document provides guidelines for adding agents and tools to the registry.

## Quick Start

1. Fork the repository
2. Create a feature branch (`git checkout -b add-my-agent`)
3. Add your agent/tool manifests
4. Validate against schemas
5. Submit a Pull Request

## Adding an Agent

### 1. Create the YAML Manifest

Create a new file in `agents/{agent_id}.yaml`:

```yaml
agent_id: my-agent
display_name: My Agent
version: 1.0.0
description: What this agent does
runtime: mcp

endpoint:
  transport: http
  uri: https://my-agent.example.com/mcp

capabilities:
  - name: my_capability
    description: What this capability does
    streaming: false
    modalities: [text]
    input_schema:
      type: object
      properties:
        param1:
          type: string
      required: [param1]
    output_schema:
      type: object
      properties:
        result:
          type: string
    max_timeout_ms: 30000

tags: [tag1, tag2]
trust_tier: org
author: your-org
```

### 2. Create the JSON Metadata

Create `agents/{agent_id}.json`:

```json
{
  "id": "my-agent",
  "type": "agent",
  "name": "My Agent",
  "description": "What this agent does",
  "version": "1.0.0",
  "runtime": "mcp",
  "endpoint": "https://my-agent.example.com/mcp",
  "capabilities": ["my_capability"],
  "tags": ["tag1", "tag2"],
  "trust_tier": "org",
  "author": "your-org",
  "status": "active",
  "manifest": "/agents/my-agent.yaml"
}
```

### 3. Update index.json

Add your agent to the `agents` array in `index.json`:

```json
{
  "agent_id": "my-agent",
  "path": "/agents/my-agent.yaml",
  "meta_path": "/agents/my-agent.json",
  "trust_tier": "org",
  "tags": ["tag1", "tag2"],
  "status": "active"
}
```

## Adding a Tool Category

### 1. Create the YAML Manifest

Create `tools/{category}.yaml`:

```yaml
category: my-category
description: What these tools do

tools:
  - tool_id: my-category.tool-name
    display_name: Tool Name
    description: What this tool does
    provider: builtin
    category: my-category
    capabilities:
      - name: do_something
        description: What this does
        streaming: false
        modalities: [text]
        input_schema:
          type: object
          properties:
            input:
              type: string
          required: [input]
        output_schema:
          type: object
          properties:
            output:
              type: string
        max_timeout_ms: 10000
```

### 2. Create the JSON Metadata

Create `tools/{category}.json`:

```json
{
  "id": "my-category",
  "type": "tool-category",
  "name": "My Category",
  "description": "What these tools do",
  "tools": ["my-category.tool-name"],
  "trust_tier": "public",
  "author": "your-org",
  "status": "active",
  "manifest": "/tools/my-category.yaml"
}
```

### 3. Update index.json

Add to the `tools` array in `index.json`.

## Validation

Before submitting, validate your manifests:

### Using Python

```python
import json
import yaml
from jsonschema import validate

# Load schemas
with open('schema/agent.schema.json') as f:
    agent_schema = json.load(f)

# Validate agent
with open('agents/my-agent.yaml') as f:
    agent = yaml.safe_load(f)
    validate(instance=agent, schema=agent_schema)

print("✓ Validation passed")
```

### Required Fields

**Agents:**
- `agent_id` - Unique identifier
- `display_name` - Human-readable name
- `version` - Semver version
- `description` - What the agent does
- `endpoint` - Connection details
- `capabilities` - List of capabilities

**Tools:**
- `tool_id` - Unique identifier (category.name format)
- `display_name` - Human-readable name
- `description` - What the tool does
- `provider` - `builtin` or `remote`
- `capabilities` - List of capabilities with input/output schemas

## Trust Tiers

Choose the appropriate trust tier:

- **`local`** - Local deployment only (e.g., system.execute)
- **`org`** - Organization boundary (most agents)
- **`public`** - Available to anyone (safe, read-only tools)

## Review Process

1. Automated validation runs on PR
2. Maintainers review for compliance
3. Community feedback period (if significant)
4. Merge and deploy

## Guidelines

### Do

- Provide clear, detailed descriptions
- Include input/output JSON schemas
- Set reasonable timeouts
- Use semantic versioning
- Test your agent/tool before submitting

### Don't

- Include secrets or credentials
- Submit broken or untested agents
- Use vague descriptions
- Break backward compatibility without version bump

## Questions?

- Open an issue for discussion
- Join our Discord/Slack community
- Check existing manifests for examples

## Code of Conduct

Be respectful, constructive, and inclusive. We're building this together.
