# Glosario

- **Instancia / gateway:** Un proceso OpenClaw con su propio volumen de datos (`~/.openclaw` en el contenedor). Cada persona o rol puede tener una.
- **Workspace:** Directorio dentro del volumen donde viven `AGENTS.md`, `SOUL.md`, `USER.md`, `MEMORY.md`, `memory/*.md`, etc.
- **Sandbox del gateway:** El interior del contenedor OpenClaw. No es el host: rutas como `/srv/...` del host **no** existen ahí salvo que estén montadas explícitamente.
- **Agent Executor (patrón):** Servicio HTTP en la misma red Docker que OpenClaw que ejecuta comandos en el **host** (o en contenedores vía Docker socket) bajo una **allowlist**. OpenClaw lo llama como herramienta/skill.
- **Memoria diaria:** Archivos bajo `workspace/memory/YYYY-MM-DD*.md` (humano o agente durante el día).
- **Memoria nocturna / nightly:** Archivo `YYYY-MM-DD-nightly.md` generado por un job externo (p. ej. n8n) a partir de logs y contexto.
- **`latest.md`:** Puntero (symlink o copia) al archivo diario más útil para que el agente lea “lo reciente” sin adivinar el nombre.
- **Memoria larga (`MEMORY.md`):** Hechos estables curados a mano o por un paso de “promoción” conservador.
- **Memoria semántica nativa:** Índice local (p. ej. SQLite + embeddings) que OpenClaw usa para búsqueda/recall dentro de la instancia.
- **Sync compartido:** Copiar solo los archivos **idénticos** entre instancias (p. ej. apéndice de capacidades), sin tocar identidad ni memoria personal.
