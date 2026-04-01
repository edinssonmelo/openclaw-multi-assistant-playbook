# Bucle nocturno de memoria (“learning loop” ambiental)

## Intención

Cada noche (o en otro horario) el sistema **acumula contexto útil en archivos** sin depender de que el modelo “recuerde” entre sesiones en sus pesos. Es un **bucle de aprendizaje operativo**: el entorno guarda lo que importa; el agente **lee** esas capas en la siguiente conversación.

## Señales de entrada (por instancia)

Orden sugerido:

1. **Logs del contenedor** del gateway (últimas N líneas, truncar en el orquestador).
2. **Memoria diaria** ya existente `workspace/memory/YYYY-MM-DD*.md` (opcional).
3. **Extracto de `MEMORY.md`** (p. ej. primeras 80 líneas) para anclar hechos estables y reducir contradicciones.

## Salidas

| Artefacto | Ubicación |
|-----------|-----------|
| Resumen nocturno | `workspace/memory/YYYY-MM-DD-nightly.md` |
| Puntero reciente | `workspace/memory/latest.md` (symlink o copia) |

### Formato recomendado del nightly

```markdown
# Nightly memory — YYYY-MM-DD — <instance-id>

## Hechos nuevos
- ...

## Patrones (qué funcionó / qué no)
- ...

## Pendientes (sin resolver)
- ...

## Resumen (3 líneas)
1. ...
2. ...
3. ...

## Promoción a MEMORY.md (solo candidatos)
- [ ] ...
```

## Pipeline canónico (n8n u otro)

```text
Schedule (ej. 02:00)
  → fetch logs (Executor allowlist: docker logs)
  → (opcional) fetch nightly-context unificado
  → POST al modelo (vía proxy en Executor con API key en env del contenedor)
  → POST /write-memory { instance, filename, content, update_latest_symlink }
  → (opcional) POST /promote-memory con candidatos filtrados
```

## Invariantes

- Si **no** hay nota diaria, el prompt debe permitir secciones vacías o “N/A” y **no inventar** avances del día.
- Etiquetar en el prompt los bloques de contexto: `[nota-diaria]`, `[memoria-larga]`, `[noche-anterior]`, `[logs-sistema]`.
- `latest.md` no debe crear retroalimentación circular con el nightly del **mismo** día (evita que el modelo se cite a sí mismo sin nueva evidencia).

## Credenciales

Mantén claves de proveedor LLM en el **entorno del Executor** o en secretos del orquestador; no las incrustes en workflows exportados a Git.

## Idempotencia

Re-ejecución la misma noche: sobrescribir el mismo `YYYY-MM-DD-nightly.md` o usar sufijo `-attempt2` según tu política; documenta la elección en el workflow.

## Diagrama

```text
Cron
  → Executor: lectura logs / contexto
  → Modelo: síntesis Markdown
  → Executor: write-memory
  → (opcional) promote-memory
```
