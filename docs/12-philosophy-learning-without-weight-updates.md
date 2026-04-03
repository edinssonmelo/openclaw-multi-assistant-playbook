# Philosophy: Learning Without Weight Updates

## What this stack does **not** do

The language model does **not** retain knowledge across sessions in its parameters. Every conversation depends on **supplied context** plus the **files and indexes** you control.

## What it does do

1. **During the day:** write notes into `memory/YYYY-MM-DD.md`, use native semantic memory, and let approved tools write structured files.
2. **At night:** run an external job that condenses signals such as logs, heartbeat traces, and long-term memory excerpts into a structured **nightly memory** file.
3. **With curation:** filter candidates from the nightly file and either promote them into `MEMORY.md` or archive them.
4. **In the next session:** load `latest.md`, `MEMORY.md`, hooks, and shared rules so the assistant's operational identity is driven by **disk state plus reading policy**.

This is why the stack can be described as **self-improving** or **self-learning** in an operational sense. The assistant does not retrain itself, but the product still gets better because each cycle captures evidence, compresses it into useful memory, and makes that memory available to future sessions.

## Why this matters

- **Auditable:** you can inspect Git or the mounted volume and see what the system "knows."
- **Reversible:** editing or deleting Markdown corrects mistakes without any need to "untrain" a model.
- **Multi-assistant friendly:** each volume accumulates its own history without contaminating another assistant instance.

## Honest limits

- More text does not automatically improve outcomes. Without promotion and summarization, context becomes noisy.
- The Agent Executor is powerful. Security comes from **allowlists plus human review**, not blind trust in the model.

This playbook explains **how to orchestrate** those layers. Final quality still depends on prompt design, model selection, retrieval quality, and how carefully you curate `MEMORY.md`.
