# Nightly Memory Loop for Operational Learning

## Intent

Every night, or on another schedule, the system stores **useful context on disk** instead of pretending the model permanently remembers across sessions. This is an **operational learning loop** and a practical **self-learning strategy**: the environment preserves what matters, and the assistant reads those memory layers in the next conversation.

In plain terms, this loop helps the product improve over time. The assistant can learn from recent work, recurring patterns, failures, and successful actions because those signals are summarized, saved, and reused.

## Input signals per instance

Suggested order:

1. **Gateway container logs** from the last N lines, trimmed in the orchestrator.
2. Existing **daily memory** under `workspace/memory/YYYY-MM-DD*.md`, if present.
3. A short **`MEMORY.md` excerpt**, for example the first 80 lines, to anchor stable facts and reduce contradictions.

## Outputs

| Artifact | Location |
|----------|----------|
| Nightly summary | `workspace/memory/YYYY-MM-DD-nightly.md` |
| Recent pointer | `workspace/memory/latest.md` as a symlink or copy |

### Recommended nightly format

```markdown
# Nightly memory - YYYY-MM-DD - <instance-id>

## New facts
- ...

## Patterns (what worked / what did not)
- ...

## Open items
- ...

## Summary (3 lines)
1. ...
2. ...
3. ...

## Promotion to MEMORY.md (candidates only)
- [ ] ...
```

## Canonical pipeline

```text
Schedule (for example 02:00)
  -> fetch logs (Executor allowlist: docker logs)
  -> optional unified nightly-context fetch
  -> call the model through an Executor proxy with API keys in container env
  -> POST /write-memory { instance, filename, content, update_latest_symlink }
  -> optional POST /promote-memory with filtered candidates
```

## Invariants

- If there is **no** daily note, the prompt should allow empty sections or `N/A` and must **not invent** progress.
- Label context blocks clearly in the prompt, for example `[daily-note]`, `[long-term-memory]`, `[previous-nightly]`, and `[system-logs]`.
- `latest.md` should not create circular feedback with the nightly file from the **same** day unless new evidence exists.

## Credentials

Keep LLM provider keys in the **Executor environment** or in orchestrator secrets. Do not hardcode them in workflow exports committed to Git.

## Idempotency

If the workflow runs more than once on the same night, either overwrite the same `YYYY-MM-DD-nightly.md` file or use a suffix such as `-attempt2`. Document that policy in the workflow.

## Diagram

```text
Cron
  -> Executor: read logs / collect context
  -> Model: generate Markdown summary
  -> Executor: write-memory
  -> optional promote-memory
```
