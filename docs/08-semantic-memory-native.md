# Memoria semántica nativa (OpenClaw)

## Rol

Además de Markdown, OpenClaw puede mantener un **índice local** (p. ej. SQLite bajo `~/.openclaw/memory/`) con embeddings generados por un modelo GGUF descargado en el contenedor (p. ej. vía `node-llama-cpp`).

## Comportamiento esperado

- Tras sesiones reales con contenido indexable, el recall mejora (“memory search”).
- En una instancia **nueva** o recién creada, el índice puede mostrar **0 archivos / 0 chunks** hasta el primer bootstrap de embeddings.

## No sustituye

- El **nightly** y `MEMORY.md` siguen siendo la narrativa humana y auditable.
- La memoria semántica es un **complemento** para recuperar fragmentos de conversaciones y documentos previos dentro de la instancia.

## Operación

Monitorea el estado con los comandos/diagnósticos que exponga tu versión de OpenClaw (`memory status`, UI de sistema, etc.). Si cambias de modelo de embeddings, puede requerir reindexación según upstream.
