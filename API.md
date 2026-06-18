# TokensCost API Documentation

**Base URL:** `https://api.tokenscost.com/v1`

All requests require an API key passed in the request header. API access is available as a paid add-on at [tokenscost.com/pricing](https://tokenscost.com/pricing).

---

## Authentication

Include your API key in every request:

```http
Authorization: Bearer YOUR_API_KEY
```

---

## Rate Limits

| Plan | Requests/minute | Requests/day |
|---|---|---|
| Starter | 60 | 10,000 |
| Pro | 300 | 100,000 |
| Enterprise | Custom | Custom |

Rate limit headers are returned on every response:

```http
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 58
X-RateLimit-Reset: 1718000000
```

---

## Endpoints

### GET /models

Returns all tracked models with current pricing.

**Request**
```http
GET /v1/models
Authorization: Bearer YOUR_API_KEY
```

**Query Parameters**

| Parameter | Type | Description |
|---|---|---|
| `provider` | string | Filter by provider (e.g. `openai`, `anthropic`, `google`) |
| `limit` | integer | Number of results to return (default: 50, max: 200) |
| `offset` | integer | Pagination offset (default: 0) |

**Response**
```json
{
  "data": [
    {
      "id": "claude-sonnet-4-6",
      "provider": "anthropic",
      "name": "Claude Sonnet 4.6",
      "input_price_per_1m": 3.00,
      "output_price_per_1m": 15.00,
      "context_window": 200000,
      "currency": "USD",
      "updated_at": "2026-06-18T00:00:00Z"
    }
  ],
  "total": 120,
  "limit": 50,
  "offset": 0
}
```

---

### GET /models/:id

Returns pricing and metadata for a specific model.

**Request**
```http
GET /v1/models/claude-sonnet-4-6
Authorization: Bearer YOUR_API_KEY
```

**Response**
```json
{
  "id": "claude-sonnet-4-6",
  "provider": "anthropic",
  "name": "Claude Sonnet 4.6",
  "input_price_per_1m": 3.00,
  "output_price_per_1m": 15.00,
  "context_window": 200000,
  "supports_vision": true,
  "supports_tools": true,
  "currency": "USD",
  "updated_at": "2026-06-18T00:00:00Z"
}
```

---

### POST /calculate

Estimates cost for a given model and token counts.

**Request**
```http
POST /v1/calculate
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json
```

**Body**
```json
{
  "model_id": "claude-sonnet-4-6",
  "input_tokens": 10000,
  "output_tokens": 2000
}
```

**Batch Request** — calculate across multiple models at once:
```json
{
  "models": [
    { "model_id": "claude-sonnet-4-6", "input_tokens": 10000, "output_tokens": 2000 },
    { "model_id": "gpt-4o", "input_tokens": 10000, "output_tokens": 2000 }
  ]
}
```

**Response**
```json
{
  "model_id": "claude-sonnet-4-6",
  "input_tokens": 10000,
  "output_tokens": 2000,
  "input_cost": 0.03,
  "output_cost": 0.03,
  "total_cost": 0.06,
  "currency": "USD"
}
```

**Batch Response**
```json
{
  "results": [
    {
      "model_id": "claude-sonnet-4-6",
      "total_cost": 0.06,
      "currency": "USD"
    },
    {
      "model_id": "gpt-4o",
      "total_cost": 0.11,
      "currency": "USD"
    }
  ]
}
```

---

### GET /history/:id *(Add-on)*

Returns historical pricing for a specific model. Requires Historical Archive add-on.

**Request**
```http
GET /v1/history/claude-sonnet-4-6?from=2025-01-01&to=2026-01-01
Authorization: Bearer YOUR_API_KEY
```

**Query Parameters**

| Parameter | Type | Description |
|---|---|---|
| `from` | string (ISO 8601) | Start date (default: 90 days ago) |
| `to` | string (ISO 8601) | End date (default: today) |

**Response**
```json
{
  "model_id": "claude-sonnet-4-6",
  "history": [
    {
      "date": "2025-01-01",
      "input_price_per_1m": 3.00,
      "output_price_per_1m": 15.00
    },
    {
      "date": "2025-06-01",
      "input_price_per_1m": 3.00,
      "output_price_per_1m": 15.00
    }
  ]
}
```

---

## Error Codes

| Code | Meaning |
|---|---|
| `400` | Bad request — missing or invalid parameters |
| `401` | Unauthorized — invalid or missing API key |
| `403` | Forbidden — feature not available on your plan |
| `404` | Model not found |
| `429` | Rate limit exceeded |
| `500` | Internal server error |

**Error Response Format**
```json
{
  "error": {
    "code": 401,
    "message": "Invalid API key."
  }
}
```

---

## SDKs & Integrations

- REST API (this doc)
- MCP Server — plug TokensCost pricing data directly into Claude and other MCP-compatible agents
- JavaScript/TypeScript SDK *(coming soon)*
- Python SDK *(coming soon)*

---

## Changelog

| Date | Change |
|---|---|
| 2026-06-18 | v1 API launched |

---

## Support

[hello@tokenscost.com](mailto:hello@tokenscost.com) · [tokenscost.com](https://tokenscost.com)
