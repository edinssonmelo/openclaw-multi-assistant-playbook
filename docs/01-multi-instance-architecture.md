# Arquitectura: varias instancias aisladas

## Objetivo

Varios asistentes (p. ej. uno por miembro del hogar o por contexto laboral/personal) con:

- **Memoria y sesiones separadas** (volúmenes distintos).
- **Misma imagen** OpenClaw (o tag fijado), para operación homogénea.
- **Canales** (Telegram, Web, etc.) configurados **por instancia** para no mezclar conversaciones.

## Patrón de despliegue

```text
                    ┌─────────────────┐
                    │  Reverse proxy  │  (Traefik, Caddy, nginx, …)
                    │  TLS terminado  │  (a menudo delante: Cloudflare u otro CDN)
                    └────────┬────────┘
                             │ HTTP interno
         ┌───────────────────┼───────────────────┐
         ▼                   ▼                   ▼
   openclaw:A          openclaw:B          n8n / otros
   :18789               :18790              (misma red Docker)
```

Cada servicio `openclaw_*` expone un **puerto interno distinto** (p. ej. 18789, 18790, …). El proxy enruta por **hostname** (`assistant-a.example.com`, `assistant-b.example.com`).

## Datos en el host

Convención típica (ajusta a tu layout):

```text
/srv/data/openclaw/instance-a/   → montado en /home/node/.openclaw
/srv/data/openclaw/instance-b/   → idem
```

Dentro de cada uno: estado del gateway, workspace, credenciales de canales, base de memoria semántica, etc. **No compartir** un mismo volumen entre dos personas si quieres aislamiento fuerte.

## Red Docker

Los gateways y el **Agent Executor** deben compartir una red **interna** (p. ej. `net_internal`) para que el DNS `http://agent_executor:8765` resuelva desde OpenClaw y desde n8n.

## Escalar a N instancias

1. Duplicar el bloque de servicio en Compose con nuevo puerto, volumen y regla `Host()`.
2. Duplicar el workflow nocturno cambiando el identificador de instancia y el nombre del contenedor en los pasos que lean logs.
3. Mantener **una** política de `agent_executor` compartida o por entorno; separar allowlists si el riesgo lo exige.
