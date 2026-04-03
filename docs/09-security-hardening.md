# Security Hardening

## OpenClaw gateway

Recommended patterns, with key names adjusted to your `openclaw.json` schema:

- **`tools.exec.security: allowlist`** - allow only explicit commands or patterns.
- **`tools.exec.ask: on-miss`** - request confirmation when a tool call falls outside the list.
- Do **not** use broad wildcards such as `["*"]` in `tools.elevated.allowFrom.*`; restrict access to known chats or users.
- **`gateway.auth.rateLimit`** - reduce abuse and accidental overload.
- **`gateway.trustedProxies`** - if Traefik or Caddy sits in front, configure trusted proxies so real client IPs and rate limits remain accurate.

## Agent Executor

- Use an allowlist of **shell prefixes**, not free-form shell access.
- Block dangerous pipes and destructive commands in automatic mode.
- Mount only the host paths the Executor actually needs. The Docker socket is **high sensitivity**.

## n8n and reverse proxies

If n8n runs behind a reverse proxy, set `N8N_PROXY_HOPS` or the equivalent option for your version so forwarded IPs are trusted correctly and warning noise disappears.

## Secrets

- Keep API keys in host `.env` files referenced by Compose, or in orchestrator secrets, **not** in Markdown inside the assistant workspace.
- Export workflows to Git without embedded credentials. Use n8n credentials or Executor environment variables instead.

## Messaging surface

- Keep channel-specific allowlists for Telegram user IDs, WhatsApp numbers, or equivalent identifiers.
- Require explicit pairing or enrollment when the product workflow needs it.
