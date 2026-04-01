# Ingress, Traefik y binding del gateway

## Problema frecuente

OpenClaw queda escuchando solo en `127.0.0.1` dentro del contenedor. El **reverse proxy** en otro contenedor no puede conectar.

**Solución:** configurar el gateway para escuchar en todas las interfaces del contenedor (`lan` o `0.0.0.0` según documentación de tu versión) y asegurar `OPENCLAW_GATEWAY_BIND=lan` (o equivalente).

## Traefik (Docker provider)

Etiquetas típicas:

- `traefik.enable=true`
- Regla `Host(\`assistant-a.${DOMAIN}\`)`
- `entrypoints=web` si TLS lo termina Cloudflare u otro túnel y llega HTTP interno
- `loadbalancer.server.port=<puerto interno del gateway>`

## Origen de confianza (`allowedOrigins`)

Si usas Control UI u otra SPA, configura los orígenes permitidos para WebSocket/API detrás de tu dominio público.

## Healthchecks

Usa el endpoint HTTP del gateway (`/healthz` o el que exponga tu build) en el `healthcheck` de Compose para que el proxy no enrute a contenedores aún no listos.

## Túnel (Cloudflare, etc.)

Patrón común: túnel → `http://traefik:80` en la red Docker. Cada hostname público debe existir en el panel del túnel y coincidir con la regla `Host()` de Traefik.
