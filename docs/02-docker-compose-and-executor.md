# Docker Compose, Volumes, and Agent Executor

## Minimum services

1. **One OpenClaw container per assistant instance** using `ghcr.io/openclaw/openclaw` or the image you maintain.
2. **One `agent_executor` service** with a stable DNS name, built from your own `Dockerfile` or private image, exposing HTTP such as `:8765` and executing host commands according to policy.

## Gateway environment variables

- `OPENCLAW_GATEWAY_BIND=lan` - required when a **reverse proxy** in another container connects to the gateway. `loopback` often breaks Traefik or Caddy.
- `OPENCLAW_GATEWAY_PORT` - must be unique **per instance**.

## Typical gateway volume mounts

| Host mount -> container | Purpose |
|-------------------------|---------|
| `/srv/data/openclaw/<id>:/home/node/.openclaw` | Full gateway state |
| Optional MCP credentials such as `~/.google-mcp` | Persist OAuth for tools launched with `npx` or similar |
| Optional `public-site` static content | Use only if you enable integrated static publishing |

## Common Agent Executor mounts

| Mount | Purpose |
|-------|---------|
| `/var/run/docker.sock` | Controlled `docker logs` and `docker exec` through an allowlist |
| Infrastructure repo on the host such as `/srv/gitops/...` | `git status`, guided edits, or Aider workflows |
| The full `/srv/data/openclaw` tree | Allow `POST /write-memory` to write into `.../workspace/memory/` per instance |
| Aider virtual environment on the host | Reuse the same path inside the container if the venv has absolute shebangs |

**Python compatibility:** if the host virtual environment points to `/usr/bin/python3` and the executor image does not match, add a compatible symlink or rebuild the venv under a path available inside the container.

## Useful Executor endpoints

Illustrative names only; your implementation can differ:

- `POST /execute` - accepts shell instructions in JSON, often with an `auto` versus confirmation mode.
- `GET /healthz` - smoke check.
- `POST /write-memory` - writes Markdown into `workspace/memory/` for exactly one declared instance.
- `POST /nightly-context` - returns daily memory snippets, a `MEMORY.md` header, and trimmed logs in one call.
- `POST /deepseek-chat` or another provider-specific proxy - lets n8n avoid fragile auth header handling in code nodes.
- `POST /promote-memory` - performs conservative candidate promotion into `MEMORY.md`.

Use a strict **allowlist of command prefixes** such as `docker logs`, `docker exec <approved-name>`, and `cat /srv/...`. Block dangerous piping patterns and unrestricted shell access.

## Example

See [examples/docker-compose.openclaw.snippet.yml](../examples/docker-compose.openclaw.snippet.yml).
