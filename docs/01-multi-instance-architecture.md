# Architecture: Multiple Isolated Assistant Instances

## Goal

Run multiple assistants, for example one per user, team, role, or workflow context, with:

- **Separate memory and sessions** through distinct volumes.
- The same **OpenClaw image** or pinned tag for predictable operations.
- Per-instance **channels** such as Telegram or Web so conversations never mix.

This is a practical pattern for a **multi-assistant architecture on a server using containers**.

## Deployment pattern

```text
                    ┌─────────────────┐
                    │  Reverse proxy  │  (Traefik, Caddy, nginx, ...)
                    │  TLS terminated │  (often behind Cloudflare or another CDN)
                    └────────┬────────┘
                             │ Internal HTTP
         ┌───────────────────┼───────────────────┐
         ▼                   ▼                   ▼
   openclaw:A          openclaw:B          n8n / other jobs
   :18789               :18790             (same Docker network)
```

Each `openclaw_*` service exposes a different **internal port** such as 18789 or 18790. The reverse proxy routes by **hostname** such as `assistant-a.example.com` and `assistant-b.example.com`.

## Host-side data

Typical convention:

```text
/srv/data/openclaw/instance-a/   -> mounted to /home/node/.openclaw
/srv/data/openclaw/instance-b/   -> same pattern
```

Each directory contains gateway state, workspace files, channel credentials, semantic memory data, and related instance-specific state. Do **not** share the same volume across users or assistant roles if strong isolation matters.

## Docker networking

The OpenClaw gateways and the **Agent Executor** should share an internal Docker network such as `net_internal` so names like `http://agent_executor:8765` resolve from OpenClaw and from workflow tools such as n8n.

## Scaling to N instances

1. Duplicate the Compose service block with a new port, volume, and `Host()` rule.
2. Duplicate the nightly workflow and change the instance identifier and container name used in log-reading steps.
3. Keep one shared `agent_executor` policy or split policies by environment when risk or compliance requires it.
