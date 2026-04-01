# Docker Compose, volúmenes y Agent Executor

## Servicios mínimos

1. **Un contenedor OpenClaw por instancia** (`image: ghcr.io/openclaw/openclaw` o la que uses).
2. **Un servicio `agent_executor` (nombre DNS estable)** construido desde tu propio `Dockerfile` o imagen privada: expone HTTP (p. ej. `:8765`) y ejecuta comandos en el host según política.

## Variables de entorno del gateway

- `OPENCLAW_GATEWAY_BIND=lan` — necesario si un **reverse proxy** en otra caja contenedor habla con el gateway; `loopback` suele romper Traefik/Caddy.
- `OPENCLAW_GATEWAY_PORT` — único **por instancia**.

## Volúmenes típicos del gateway

| Montaje host → contenedor | Propósito |
|---------------------------|-----------|
| `/srv/data/openclaw/<id>:/home/node/.openclaw` | Estado completo del gateway |
| Opcional: credenciales MCP (`~/.google-mcp` u otro) | Persistir OAuth de herramientas que usen `npx` |
| Opcional: sitio estático `public-site` | Si usas publicación estática integrada |

## Agent Executor: montajes habituales

| Montaje | Propósito |
|---------|-----------|
| `/var/run/docker.sock` | `docker logs`, `docker exec` controlados por allowlist |
| Repo de infra en el host (`/srv/gitops/...`) | `git status`, edición asistida con Aider |
| **Todo el árbol** `/srv/data/openclaw` | Permitir `POST /write-memory` que escriba bajo `.../workspace/memory/` por instancia |
| Venv de Aider en el host | Misma ruta dentro del contenedor si el venv usa shebangs absolutos del host |

**Compatibilidad Python:** si el venv del host apunta a `/usr/bin/python3` y la imagen del executor no coincide, añade un symlink o reconstruye el venv dentro de una ruta compatible.

## Endpoints útiles del Executor (patrón de referencia)

Nombres ilustrativos; tu código puede variar:

- `POST /execute` — cuerpo JSON con instrucción shell; modo `auto` vs confirmación según política.
- `GET /healthz` — humo.
- `POST /write-memory` — escribe Markdown bajo `workspace/memory/` de **una** instancia declarada en el JSON (`instance: a|b`).
- `POST /nightly-context` — agrega en una sola llamada: fragmento de memoria diaria, cabecera de `MEMORY.md`, logs recortados.
- `POST /deepseek-chat` (u otro proveedor) — proxy HTTP para que n8n no gestione cabeceras de auth frágiles en nodos Code.
- `POST /promote-memory` — promoción conservadora de candidatos a `MEMORY.md`.

Define **allowlist** estricta de prefijos (`docker logs`, `docker exec <nombre permitido>`, `cat /srv/...`) y bloquea tuberías peligrosas.

## Ejemplo genérico

Ver [examples/docker-compose.openclaw.snippet.yml](../examples/docker-compose.openclaw.snippet.yml).
