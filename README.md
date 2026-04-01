# OpenClaw multi-assistant playbook

Documentación **genérica y redactada para publicar**: arquitectura, estrategia y mejoras para ejecutar **varias instancias aisladas** de [OpenClaw](https://docs.openclaw.ai) (o compatible) con **memoria en disco**, un **puente agéntico** hacia el host (Agent Executor), y un **bucle nocturno de “aprendizaje”** vía orquestación externa (p. ej. n8n) — sin pretender que el modelo aprenda en los pesos entre sesiones.

> **Aviso:** esto es un playbook de patrones probados en un homelab; adapta rutas, nombres de red Docker y políticas de seguridad a tu entorno. No hay garantías; revisa el upstream OpenClaw para tu versión.

## Qué encontrarás aquí

| Área | Documento |
|------|-------------|
| Términos y capas de memoria | [docs/00-glossary.md](docs/00-glossary.md) |
| Varias instancias, aislamiento, puertos | [docs/01-multi-instance-architecture.md](docs/01-multi-instance-architecture.md) |
| Compose, volúmenes, `agent_executor`, redes | [docs/02-docker-compose-and-executor.md](docs/02-docker-compose-and-executor.md) |
| Traefik / proxy, `bind: lan`, health | [docs/03-ingress-and-gateway-binding.md](docs/03-ingress-and-gateway-binding.md) |
| Workspace: `SOUL`, `USER`, `MEMORY`, `AGENTS`, sync entre instancias | [docs/04-workspace-files-and-sync.md](docs/04-workspace-files-and-sync.md) |
| Sandbox del gateway vs host; modo autónomo; prioridades | [docs/05-agent-behavior-and-executor.md](docs/05-agent-behavior-and-executor.md) |
| Bucle nocturno: logs + síntesis + `write-memory` + promoción | [docs/06-nightly-memory-learning-loop.md](docs/06-nightly-memory-learning-loop.md) |
| Cuándo subir hechos a `MEMORY.md` | [docs/07-memory-promotion.md](docs/07-memory-promotion.md) |
| Memoria semántica nativa (SQLite + embeddings locales) | [docs/08-semantic-memory-native.md](docs/08-semantic-memory-native.md) |
| Endurecimiento: allowlist, rate limit, proxies de confianza | [docs/09-security-hardening.md](docs/09-security-hardening.md) |
| Checklist de humo, memoria, conversación, agencia | [docs/10-evaluation-checklist.md](docs/10-evaluation-checklist.md) |
| Live trace, Telegram streaming, transparencia operativa | [docs/11-live-trace-and-transparency.md](docs/11-live-trace-and-transparency.md) |
| Filosofía: “aprendizaje” = estado en disco + política | [docs/12-philosophy-learning-without-weight-updates.md](docs/12-philosophy-learning-without-weight-updates.md) |

**Plantillas listas para adaptar:**

- [templates/SHARED_AGENTS_APPENDIX.template.md](templates/SHARED_AGENTS_APPENDIX.template.md)
- [templates/operatorModePrompt.template.md](templates/operatorModePrompt.template.md)

**Ejemplo genérico de fragmento Compose:** [examples/docker-compose.openclaw.snippet.yml](examples/docker-compose.openclaw.snippet.yml)

## Idea central en una frase

**Varios asistentes** = varios **volúmenes** y **gateways** (o perfiles equivalentes), más **un ejecutor con allowlist** en la misma red Docker que hable con el host, más **archivos Markdown curados** y un **job nocturno** que condensa señales (logs, memoria diaria) en notas estructuradas y, opcionalmente, promueve a memoria larga.

## Cómo usar este repo

1. Lee [docs/00-glossary.md](docs/00-glossary.md) y [docs/12-philosophy-learning-without-weight-updates.md](docs/12-philosophy-learning-without-weight-updates.md).
2. Ajusta [examples/docker-compose.openclaw.snippet.yml](examples/docker-compose.openclaw.snippet.yml) a tu dominio, UID/GID y rutas bajo `/srv` o equivalente.
3. Copia las plantillas de `templates/` al **workspace** de cada instancia y personaliza identidad solo en archivos por instancia (`SOUL.md`, `USER.md`, …).
4. Implementa el bucle nocturno según [docs/06-nightly-memory-learning-loop.md](docs/06-nightly-memory-learning-loop.md) con tu orquestador favorito.

## Publicar solo esta carpeta

Ver [PUBLISHING.md](PUBLISHING.md).

## Licencia

[LICENSE](LICENSE) — MIT.
