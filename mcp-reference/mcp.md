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
- `mallary_attach_tiktok_post_url`
- `mallary_list_posts`
- `mallary_list_comments`
- `mallary_reply_to_comment`
- `mallary_delete_post`
- `mallary_get_analytics`
- `mallary_get_audience`
- `mallary_list_profiles`
- `mallary_create_profile`
- `mallary_rename_profile`
- `mallary_list_platforms`
- `mallary_disconnect_platform`
- `mallary_get_settings`
- `mallary_update_settings`
- `mallary_list_webhooks`
- `mallary_create_webhook`
- `mallary_delete_webhook`

## Tool Behavior

- MCP tools route through existing `/api/v1/*` handlers (no duplicated posting logic).
- `mallary_create_post` runs preflight first by default (`run_preflight=true`).
- `Idempotency-Key` is forwarded when `idempotency_key` is provided.
- Use `mallary_list_profiles` to list random public profile IDs.
- Publishing, post listing, comment listing, analytics, platform listing, settings, and disconnect tools accept `profile_id` for non-default connection profiles.
- `mallary_create_profile` and `mallary_rename_profile` manage Mallary connection profiles.
- `mallary_list_comments` and `mallary_reply_to_comment` are available on all Mallary plans.
- Non-2xx API results are returned as structured MCP errors with:
  - `http_status`
  - `code`
  - `message`
  - `details` (when available)

## Notes

- Streamable HTTP can return event-stream payloads for tool responses.
- Configure with these environment variables:
  - `MCP_ENABLED` (`0`/`1`)
  - `MCP_PATH` (default `/mcp`)
  - `MCP_MAX_BODY_BYTES`
  - `MCP_ALLOWED_ORIGINS` (comma-separated)
