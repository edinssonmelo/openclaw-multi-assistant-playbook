# OpenClaw Multi-Assistant Playbook

Publish-ready documentation for building a **self-hosted multi-assistant architecture** with [OpenClaw](https://docs.openclaw.ai) or a compatible stack. This repository documents how to run **multiple isolated AI assistants** on a server with **containers**, **disk-backed memory**, an **agent executor bridge** to the host, and a clear **self-improving architecture** based on heartbeat, nightly memory synthesis, and long-term memory curation, without claiming model weight updates across sessions.

This playbook is intentionally generic so it can be reused for **containerized LLM agents**, **multi-agent server deployments**, **Docker Compose AI assistant setups**, **persistent memory orchestration**, and **self-learning AI system** patterns that improve agent behavior over time through files, retrieval, and automation.

> **Note:** this is a field-tested homelab and self-hosting playbook, not an official OpenClaw reference. Adapt paths, Docker network names, proxy rules, and security policies to your environment. Always validate against the upstream OpenClaw documentation for your version.

## What this repo covers

| Area | Document |
|------|----------|
| Terms and memory layers | [docs/00-glossary.md](docs/00-glossary.md) |
| Multi-instance isolation, ports, and topology | [docs/01-multi-instance-architecture.md](docs/01-multi-instance-architecture.md) |
| Docker Compose, volumes, `agent_executor`, and networks | [docs/02-docker-compose-and-executor.md](docs/02-docker-compose-and-executor.md) |
| Traefik or reverse proxy ingress, `bind: lan`, and health checks | [docs/03-ingress-and-gateway-binding.md](docs/03-ingress-and-gateway-binding.md) |
| Workspace files: `SOUL`, `USER`, `MEMORY`, `AGENTS`, and cross-instance sync | [docs/04-workspace-files-and-sync.md](docs/04-workspace-files-and-sync.md) |
| Gateway sandbox vs host access, autonomous mode, and execution rules | [docs/05-agent-behavior-and-executor.md](docs/05-agent-behavior-and-executor.md) |
| Nightly memory loop: logs, summarization, `write-memory`, and promotion | [docs/06-nightly-memory-learning-loop.md](docs/06-nightly-memory-learning-loop.md) |
| When to promote facts into `MEMORY.md` | [docs/07-memory-promotion.md](docs/07-memory-promotion.md) |
| Native semantic memory with SQLite and local embeddings | [docs/08-semantic-memory-native.md](docs/08-semantic-memory-native.md) |
| Security hardening: allowlists, rate limits, trusted proxies | [docs/09-security-hardening.md](docs/09-security-hardening.md) |
| Validation checklist for infra, memory, conversation quality, and agent behavior | [docs/10-evaluation-checklist.md](docs/10-evaluation-checklist.md) |
| Live trace, streaming updates, and operational transparency | [docs/11-live-trace-and-transparency.md](docs/11-live-trace-and-transparency.md) |
| Philosophy: operational learning without model retraining | [docs/12-philosophy-learning-without-weight-updates.md](docs/12-philosophy-learning-without-weight-updates.md) |

## Templates

- [templates/SHARED_AGENTS_APPENDIX.template.md](templates/SHARED_AGENTS_APPENDIX.template.md)
- [templates/operatorModePrompt.template.md](templates/operatorModePrompt.template.md)

## Example

Generic Docker Compose snippet:
[examples/docker-compose.openclaw.snippet.yml](examples/docker-compose.openclaw.snippet.yml)

## Core idea in one sentence

Run **multiple AI assistants** as **isolated containerized instances**, each with its own volume and gateway, connect them to a constrained **Agent Executor** on the same Docker network, and use curated Markdown plus a **nightly memory workflow** to create durable, auditable operational memory.

## Self-learning strategy

This repository also documents a simple **self-learning architecture** for AI assistants. The system improves over time by collecting interaction signals, keeping lightweight heartbeat-style operational traces, summarizing the day into nightly memory, promoting stable facts into long-term memory, and reloading those memory layers in later sessions. In other words, the product gets better through **persistent state, retrieval, and policy**, not through hidden retraining.

## Why this is useful

- Supports **multi-assistant server architecture** without mixing sessions or memory across roles.
- Keeps **persistent memory** on disk instead of pretending the model permanently learns between chats.
- Describes a practical **self-improving AI assistant architecture** that uses heartbeat, nightly processing, and memory promotion to improve future behavior.
- Makes the system **auditable**, because behavior is grounded in files, prompts, logs, and explicit policies.
- Works well for **self-hosted AI assistant infrastructure** using Docker, reverse proxies, and workflow automation.

## How to use this repo

1. Start with [docs/00-glossary.md](docs/00-glossary.md) and [docs/12-philosophy-learning-without-weight-updates.md](docs/12-philosophy-learning-without-weight-updates.md).
2. Adapt [examples/docker-compose.openclaw.snippet.yml](examples/docker-compose.openclaw.snippet.yml) to your domain, UID/GID, and server paths under `/srv` or an equivalent layout.
3. Copy the templates from `templates/` into the workspace of each assistant instance and customize only the per-instance identity files such as `SOUL.md`, `USER.md`, and `MEMORY.md`.
4. Implement the nightly memory workflow described in [docs/06-nightly-memory-learning-loop.md](docs/06-nightly-memory-learning-loop.md) using n8n or another orchestrator.

## Publishing this folder by itself

See [PUBLISHING.md](PUBLISHING.md).

## License

[LICENSE](LICENSE) - MIT.
