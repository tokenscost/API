# TokensCost MCP Server

The TokensCost MCP server exposes live model pricing data to any MCP-compatible AI agent or tool — including Claude, Cursor, and other MCP clients.

## What It Exposes

| Tool | Description |
|---|---|
| `get_model_pricing` | Returns current input/output pricing for a specific model |
| `list_models` | Lists all tracked models with pricing, optionally filtered by provider |
| `calculate_cost` | Estimates cost for a given model and token counts |
| `compare_models` | Returns side-by-side pricing for a list of models |

## Setup

### Prerequisites
- Node.js 18+
- A TokensCost API key ([get one here](https://tokenscost.com/pricing))

### Install

```bash
npm install -g @tokenscost/mcp-server
```

### Configure

Add to your MCP client config (e.g. `claude_desktop_config.json` for Claude Desktop):

```json
{
  "mcpServers": {
    "tokenscost": {
      "command": "tokenscost-mcp",
      "env": {
        "TOKENSCOST_API_KEY": "your_api_key_here"
      }
    }
  }
}
```

### Environment Variables

| Variable | Required | Description |
|---|---|---|
| `TOKENSCOST_API_KEY` | Yes | Your TokensCost API key |
| `TOKENSCOST_API_URL` | No | Override API base URL (default: `https://api.tokenscost.com/v1`) |

## Tool Reference

### get_model_pricing

```json
{
  "name": "get_model_pricing",
  "input": {
    "model_id": "claude-sonnet-4-6"
  }
}
```

**Returns:** `input_price_per_1m`, `output_price_per_1m`, `context_window`, `updated_at`

---

### list_models

```json
{
  "name": "list_models",
  "input": {
    "provider": "anthropic"
  }
}
```

**Returns:** Array of models with current pricing. `provider` is optional.

---

### calculate_cost

```json
{
  "name": "calculate_cost",
  "input": {
    "model_id": "gpt-4o",
    "input_tokens": 5000,
    "output_tokens": 1000
  }
}
```

**Returns:** `input_cost`, `output_cost`, `total_cost` in USD

---

### compare_models

```json
{
  "name": "compare_models",
  "input": {
    "model_ids": ["claude-sonnet-4-6", "gpt-4o", "gemini-1.5-pro"]
  }
}
```

**Returns:** Side-by-side pricing for all specified models

---

## Support

[hello@tokenscost.com](mailto:hello@tokenscost.com) · [tokenscost.com](https://tokenscost.com)
