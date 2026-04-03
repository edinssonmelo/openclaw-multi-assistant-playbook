# Native Semantic Memory in OpenClaw

## Role

In addition to Markdown files, OpenClaw can maintain a **local semantic memory index**, for example SQLite under `~/.openclaw/memory/`, with embeddings produced by a model running inside the container such as `node-llama-cpp` with a GGUF embedding model.

## Expected behavior

- After real sessions with indexable content, retrieval quality improves through memory search and recall.
- In a **fresh instance**, the index may show **0 files / 0 chunks** until the first embedding bootstrap or indexing pass completes.

## What it does not replace

- The **nightly memory loop** and `MEMORY.md` still provide the human-readable, auditable memory narrative.
- Semantic memory is a **complement** for retrieving conversation fragments and prior documents inside the same assistant instance.

## Operations

Monitor semantic memory with whichever commands or diagnostics your OpenClaw version exposes, such as `memory status` or system UI panels. If you change embedding models, reindexing may be required depending on upstream behavior.
