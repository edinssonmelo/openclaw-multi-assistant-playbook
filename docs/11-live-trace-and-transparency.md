# Live trace y transparencia operativa

## Objetivo

Mostrar **progreso observable** (herramientas, pasos, resultados) sin exponer cadena de pensamiento interna token a token.

## OpenClaw / canales

Capacidades útiles (según versión):

- **Telegram:** `channels.telegram.streaming: partial` para edición en vivo del mensaje.
- **Directivas de sesión:** `/verbose`, `/reasoning` con modos `on`, `stream`, `full` según documentación upstream.
- **Web Control UI:** eventos de herramientas y salidas en tarjetas.

## Protocolo en prompts (apéndice compartido)

Para cada herramienta:

1. **Antes:** una línea — qué harás y por qué.
2. **Después:** Acción (resumida), Resultado (1–2 líneas), Siguiente paso.
3. Si tarda, estado intermedio cada ~20–40 s con progreso real.
4. Si falla: causa probable + alternativa concreta.

No mostrar razonamiento privado del modelo; sí trazabilidad **operativa**.

## Heartbeat vs razonamiento visible

El heartbeat periódico puede seguir siendo **silencioso** en el canal aunque el modelo genere reasoning. Si necesitas resúmenes periódicos al usuario, usa **cron** del producto o un workflow externo con entrega explícita al chat, no solo heartbeat.

## Riesgos

- Streams complejos en Telegram a veces degradan; puedes bajar a `streaming: off` para consistencia.
- Combinar eventos nativos + protocolo en prompts da la experiencia más estable.

Ver plantilla: [templates/SHARED_AGENTS_APPENDIX.template.md](../templates/SHARED_AGENTS_APPENDIX.template.md).
