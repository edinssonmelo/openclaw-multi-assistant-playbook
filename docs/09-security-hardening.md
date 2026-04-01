# Endurecimiento de seguridad

## Gateway OpenClaw

Patrones recomendados (nombres de clave según tu `openclaw.json`):

- **`tools.exec.security: allowlist`** — solo comandos/patrones explícitos.
- **`tools.exec.ask: on-miss`** — pedir confirmación si la herramienta no está en lista.
- **No usar wildcards** en `tools.elevated.allowFrom.*` tipo `["*"]`; restringe a chats/usuarios conocidos.
- **`gateway.auth.rateLimit`** — mitigar abuso.
- **`gateway.trustedProxies`** — si hay Traefik/Caddy delante, configura proxies de confianza para IP real y rate limit correcto.

## Agent Executor

- Allowlist de **prefijos** de shell, no “shell libre”.
- Bloquear tuberías peligrosas y `rm -rf` en modo automático.
- Montar solo los paths necesarios del host; el socket Docker es **muy sensible**.

## n8n y proxies

Si n8n está detrás de reverse proxy, fija `N8N_PROXY_HOPS` (u equivalente) para que `X-Forwarded-For` sea confiable y desaparezcan warnings; revisa la documentación de tu versión.

## Secretos

- API keys en `.env` del host referenciado por Compose; **no** en Markdown del workspace.
- Workflows exportados a Git sin credenciales incrustadas; usa credenciales de n8n o env del Executor.

## Superficie de mensajería

- Allowlists por canal (Telegram user IDs, números de WhatsApp, etc.).
- Pairing explícito donde el producto lo requiera.
