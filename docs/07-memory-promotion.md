# Promoción a `MEMORY.md`

## Capas (resumen)

| Capa | Vida útil | Contenido típico |
|------|-----------|------------------|
| Cruda / diaria | Días | `memory/YYYY-MM-DD.md` |
| Nightly | Archivo permanente hasta archivo | `memory/YYYY-MM-DD-nightly.md` |
| Puntero | Siempre actualizado | `memory/latest.md` |
| Largo plazo | Meses | `MEMORY.md` |

## Reglas: qué promover

Promover a `MEMORY.md` solo si cumple al menos una condición:

1. **Repetición:** el hecho aparece en dos noches distintas y sigue siendo cierto.
2. **Decisión explícita** del humano (“recuérdalo siempre”).
3. **Alto impacto** en cómo debe actuar el asistente.
4. **Estabilidad:** no es un plan efímero del día.

**No promover:** secretos, datos personales sensibles que deban ir a un gestor, ruido, contradicciones sin resolver, duplicados literales.

## Flujo humano + automático

- El nightly puede incluir una sección **“Promoción a MEMORY.md (candidatos)”** con checkboxes.
- Un paso automatizado (`/promote-memory`) puede deduplicar y filtrar; el humano revisa cambios conflictivos.

## Aislamiento entre instancias

Cada volumen tiene su propio `MEMORY.md`. Un hecho “global” (p. ej. política de casa) puede vivir en documentación de infra en Git, no hace falta duplicarlo en todos los `MEMORY.md` salvo que cada asistente deba conocerlo en charla.

## Relación con “consciencia”

El modelo no aprende pesos entre sesiones. La **consciencia operativa** es: archivos en disco + hooks + política de qué leer al iniciar sesión. Más texto mal curado empeora el asistente; la promoción es un **filtro de calidad**.
