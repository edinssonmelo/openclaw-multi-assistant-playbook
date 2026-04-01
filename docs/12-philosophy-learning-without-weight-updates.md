# Filosofía: “aprendizaje” sin actualizar pesos

## Qué **no** hace este stack

El modelo de lenguaje **no** retiene entre sesiones en sus parámetros. Cada conversación se alimenta de **contexto enviado** y de **archivos/índices** que tú controlas.

## Qué **sí** hace (learning loop ambiental)

1. **Durante el día:** notas en `memory/YYYY-MM-DD.md`, uso de memoria semántica nativa, herramientas que escriben archivos permitidos.
2. **Por la noche:** un job externo condensa **señales** (logs, extractos de memoria larga) en un **nightly** estructurado.
3. **Con curadoría:** candidatos del nightly pasan filtros y suben a `MEMORY.md` o quedan archivados.
4. **En la siguiente sesión:** el agente lee `latest.md`, `MEMORY.md`, hooks y reglas compartidas — la “identidad operativa” es **estado en disco + política de lectura**.

## Por qué importa

- **Auditable:** puedes leer Git o el volumen y ver qué “sabe” el sistema.
- **Reversible:** borrar o editar Markdown corrige errores; no hay que “desentrenar”.
- **Multi-asistente:** cada volumen acumula su propia historia sin contaminar al otro.

## Límites honestos

- Más texto ≠ mejor: sin promoción y sin síntesis, el contexto se llena de ruido.
- El Executor es poderoso: la seguridad es **allowlist + revisión humana**, no confianza ciega en el modelo.

Este playbook describe **cómo orquestar** esas capas; la calidad final depende de tus prompts, del modelo elegido y de cuánto curas `MEMORY.md`.
