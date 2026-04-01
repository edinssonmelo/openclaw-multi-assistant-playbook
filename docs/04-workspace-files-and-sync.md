# Workspace: archivos y estrategia de sync

## Archivos clave (por instancia)

| Archivo / carpeta | Rol |
|-------------------|-----|
| `SOUL.md` | Tono, límites, identidad pública del asistente |
| `USER.md` | Perfil del humano para quien trabaja esta instancia |
| `MEMORY.md` | Hechos estables (curados) |
| `AGENTS.md` | Instrucciones operativas largas |
| `workspace/memory/*.md` | Notas diarias, nightly, experimentación |
| `workspace/skills/` | Skills locales (si aplica) |

Algunos despliegues usan también `IDENTITY.md`, `TOOLS.md`, etc.; sincroniza con la versión OpenClaw que uses.

## Qué compartir entre instancias

**Sí (idéntico en todas):**

- Reglas de **capacidades agénticas** comunes (cómo usar el Executor, transparencia operativa, sandbox vs host).
- Políticas de **seguridad** y estilo (brevedad, asertividad).

**No (nunca copiar entre personas sin revisión):**

- `USER.md`, `MEMORY.md`, contenido íntimo en `SOUL.md`.
- Tokens, allowlists de Telegram/WhatsApp con IDs reales (mejor por `.env` o config en volumen).

## Patrón “repo Git + script de sync”

1. Versiona en Git los Markdown **compartidos** (plantillas + apéndice).
2. En el servidor, un script copia solo esos archivos a **cada** `.../openclaw/<id>/workspace/`.
3. El script **no** sobrescribe `USER.md`, `MEMORY.md`, `SOUL.md` salvo flag explícito.

Así evitas “drift” donde una instancia tiene reglas antiguas y otra actualizadas.

## File Browser / carpetas públicas

Si expones documentación en una ruta tipo `docs.example.com`, **no** asumas que es el workspace del agente. El workspace operativo sigue siendo el del volumen OpenClaw. Si quieres reflejar contenido, hazlo con un script de copia explícito.
