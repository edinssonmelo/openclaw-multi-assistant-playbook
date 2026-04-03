# Workspace Files and Sync Strategy

## Key files per instance

| File / folder | Role |
|---------------|------|
| `SOUL.md` | Tone, boundaries, and public assistant identity |
| `USER.md` | Profile of the user or operator for that instance |
| `MEMORY.md` | Stable curated facts |
| `AGENTS.md` | Long-form operational instructions |
| `workspace/memory/*.md` | Daily notes, nightly memory, experiments |
| `workspace/skills/` | Local skills if used in the deployment |

Some deployments also use `IDENTITY.md`, `TOOLS.md`, or similar files. Align the set of workspace files with the OpenClaw version you run.

## What to share across instances

**Yes, keep identical across all instances:**

- Shared **agent capability rules**, such as how to use the Agent Executor, how to explain sandbox versus host access, and how to report operational progress.
- Shared **security and style policies**, such as brevity, assertiveness, and escalation rules.

**No, do not copy across users or roles without review:**

- `USER.md`, `MEMORY.md`, and any sensitive identity content in `SOUL.md`.
- Tokens, chat allowlists, or real account identifiers. Store those in `.env` files or volume-local configuration instead.

## Recommended pattern: Git repo plus sync script

1. Version only the **shared Markdown artifacts** in Git, such as templates and a shared capability appendix.
2. On the server, use a script to copy only those files into each `.../openclaw/<id>/workspace/`.
3. Make sure the script does **not** overwrite `USER.md`, `MEMORY.md`, or `SOUL.md` unless an explicit flag enables that behavior.

This reduces configuration drift across assistant instances while preserving isolation.

## File browser or public docs

If you expose documentation through a path such as `docs.example.com`, do **not** assume that it is the assistant's live workspace. The operational workspace still lives in the OpenClaw volume. If you want public or mirrored content, use an explicit sync or copy step.
