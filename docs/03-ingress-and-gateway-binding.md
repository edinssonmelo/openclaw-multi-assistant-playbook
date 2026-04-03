# Ingress, Traefik, and Gateway Binding

## Common problem

OpenClaw may listen only on `127.0.0.1` inside the container. A **reverse proxy** running in another container cannot connect to it.

**Fix:** configure the gateway to listen on all container interfaces, typically `lan` or `0.0.0.0` depending on your version, and set `OPENCLAW_GATEWAY_BIND=lan` or the equivalent setting.

## Traefik with the Docker provider

Typical labels:

- `traefik.enable=true`
- `Host(\`assistant-a.${DOMAIN}\`)`
- `entrypoints=web` if TLS is already terminated by Cloudflare or another tunnel and Traefik receives internal HTTP
- `loadbalancer.server.port=<gateway-internal-port>`

## Trusted origins (`allowedOrigins`)

If you use a Control UI or another SPA, configure allowed origins correctly for WebSocket and API traffic behind your public domain.

## Health checks

Use the gateway HTTP endpoint such as `/healthz`, or the equivalent exposed by your build, in the Compose `healthcheck` so the reverse proxy does not route traffic to containers that are not ready yet.

## Tunnel setup

A common pattern is: tunnel -> `http://traefik:80` on the Docker network. Every public hostname must exist in the tunnel configuration and match the Traefik `Host()` rule.
