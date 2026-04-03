# Live Trace and Operational Transparency

## Goal

Show **observable progress** such as tools used, actions taken, and outcomes, without exposing private token-by-token chain-of-thought.

## OpenClaw and channel features

Useful capabilities, depending on version:

- **Telegram:** `channels.telegram.streaming: partial` for live message editing.
- **Session directives:** `/verbose`, `/reasoning`, or similar modes such as `on`, `stream`, and `full`, depending on upstream support.
- **Web Control UI:** cards or events for tools and outputs.

## Prompt protocol

For every tool action:

1. **Before:** one line saying what will happen and why.
2. **After:** Action, Result, and Next step in a compact format.
3. If it takes time, emit an intermediate status update every 20 to 40 seconds with real progress.
4. If it fails, explain the likely cause and one concrete fallback.

Do not expose private model reasoning. Do provide **operational traceability**.

## Heartbeat versus visible reasoning

A background heartbeat can remain **silent** in the user channel even if the model is generating reasoning internally. If you need periodic visible summaries, use product-level scheduling or an external workflow that explicitly sends updates to the chat.

Heartbeat matters because it provides lightweight operational signals about what the assistant is doing and how work is progressing. Those signals can feed later review, nightly summarization, or memory curation, which makes the overall assistant architecture more adaptive over time.

## Risks

- Complex streaming in Telegram can degrade reliability, so `streaming: off` may be better if consistency matters more than partial updates.
- Combining native product events with prompt-level transparency rules often gives the most stable experience.

See [templates/SHARED_AGENTS_APPENDIX.template.md](../templates/SHARED_AGENTS_APPENDIX.template.md).
