# Agent Behavior: Sandbox, Executor, and Autonomous Mode

## Two worlds

An LLM inside OpenClaw may appear to "know" the server, but without tools that reach the host it **does not**. Make that explicit in prompts and system instructions:

| Context | What it can do |
|---------|----------------|
| OpenClaw container sandbox | Read and write the workspace, load skills, and call MCP tools configured **inside** the sandbox |
| Real host, Docker, or infrastructure git repo | Access only through the **Agent Executor** or an explicitly approved SSH path |

## Execution pattern

```text
OpenClaw (LLM) -> HTTP tool -> Agent Executor -> shell/docker on the host
```

The JSON request usually contains `instruction` and `mode`, such as `auto` or interactive. The Executor allowlist determines what runs without extra confirmation.

## Autonomous mode

Reduce friction for obvious low-risk read-only tasks:

- Chain 2 to 4 safe diagnostic commands such as `git status`, `docker ps`, tool versions, or health checks.
- Require explicit confirmation before destructive actions such as `rm -rf`, `git reset --hard`, database changes, secret handling, DNS edits, or tunnel reconfiguration.

## Priority before acting

1. Does this help the user's goal?
2. Is real system data required?
3. Is the action actually useful, or just tool theater?

## Useful prompt prohibitions

- Do not claim host state without a real execution result.
- Do not mix unrelated topics in the same reply unless the user did.
- Do not produce long reports with no immediate value.

## Aider integration

For multi-file infrastructure changes, route through the Executor to an Aider binary with `cwd` set to the infrastructure repository. Provider API keys usually enter through environment variables on the Executor service.

## Ready-to-adapt text

Use and customize [templates/operatorModePrompt.template.md](../templates/operatorModePrompt.template.md) and [templates/SHARED_AGENTS_APPENDIX.template.md](../templates/SHARED_AGENTS_APPENDIX.template.md).
