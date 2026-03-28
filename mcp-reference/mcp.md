# Mallary MCP (v1)

Mallary exposes a remote MCP server over Streamable HTTP for public API parity.

## Endpoint

- `POST /mcp`
- `GET /mcp` (session transport support)
- `DELETE /mcp` (session termination support)

Base URL: `https://mallary.ai`

## Authentication

Use your Mallary API key:

`Authorization: Bearer <mallary_api_key>`

Identity is always resolved from the API key owner.

## Enabled Tools (v1)

- `mallary_create_upload_url`
- `mallary_create_post`
- `mallary_get_job`
- `mallary_list_posts`
- `mallary_delete_post`
- `mallary_get_analytics`
- `mallary_disconnect_platform`
- `mallary_list_webhooks`
- `mallary_create_webhook`
- `mallary_delete_webhook`

## Tool Behavior

- MCP tools route through existing `/api/v1/*` handlers (no duplicated posting logic).
- `mallary_create_post` runs preflight first by default (`run_preflight=true`).
- `Idempotency-Key` is forwarded when `idempotency_key` is provided.
- Non-2xx API results are returned as structured MCP errors with:
  - `http_status`
  - `code`
  - `message`
  - `details` (when available)

## Notes

- Streamable HTTP may return event-stream payloads for tool responses.
- Configure via env vars:
  - `MCP_ENABLED` (`0`/`1`)
  - `MCP_PATH` (default `/mcp`)
  - `MCP_MAX_BODY_BYTES`
  - `MCP_ALLOWED_ORIGINS` (comma-separated)
