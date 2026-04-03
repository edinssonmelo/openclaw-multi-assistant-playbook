# Promoting Facts into `MEMORY.md`

## Layers at a glance

| Layer | Lifespan | Typical content |
|-------|----------|-----------------|
| Raw / daily | Days | `memory/YYYY-MM-DD.md` |
| Nightly | Persistent until archived | `memory/YYYY-MM-DD-nightly.md` |
| Pointer | Continuously updated | `memory/latest.md` |
| Long-term | Months | `MEMORY.md` |

## Promotion rules

Promote a fact into `MEMORY.md` only if at least one of these conditions is true:

1. **Repetition:** the fact appears on two different nights and still holds.
2. **Explicit instruction:** a human operator says it should always be remembered.
3. **High impact:** it materially changes how the assistant should behave.
4. **Stability:** it is not just a short-lived plan or temporary note.

Do **not** promote secrets, sensitive personal data that belongs in a secrets manager, noise, unresolved contradictions, or literal duplicates.

## Human plus automation flow

- The nightly file can include a section called **"Promotion to MEMORY.md (candidates only)"** with checkboxes.
- An automated step such as `/promote-memory` can deduplicate and filter candidates, while a human reviews conflicts.

## Isolation across instances

Every volume keeps its own `MEMORY.md`. A global rule or infrastructure-wide policy can live in shared documentation tracked in Git instead of being duplicated into every long-term memory file.

## Relation to "awareness"

The model does not update its weights between sessions. **Operational awareness** comes from files on disk plus hooks and policies that control what the assistant reads at session start. More uncurated text usually makes the assistant worse, not better, so promotion should act as a **quality filter**.
