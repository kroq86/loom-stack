# Loom stack ecosystem

**Hub:** https://kroq86.github.io/loom-stack/

Composable Python packages for **durable async agent loops**: shape → checkpoint → trace → product CLIs.

## Core libraries (PyPI)

| PyPI package | GitHub | Role |
|--------------|--------|------|
| `loom-tailcalls` | [loom-tailcalls](https://github.com/kroq86/loom-tailcalls) | `@tailrec` / `@tailstream` — stack-safe loops |
| `flow-xray` | [flow-xray](https://github.com/kroq86/flow-xray) | Offline HTML execution traces |
| `loom-runner` | [loom-runner](https://github.com/kroq86/loom-runner) | SQLite checkpoint, `resume`, `explain` |

```bash
pip install loom-tailcalls flow-xray loom-runner
```

## Product CLIs

| PyPI package | GitHub | Use when |
|--------------|--------|----------|
| `loom-run` | [loom-run](https://github.com/kroq86/loom-run) | Dev assistant on a repo (read, search, tests, MCP) |
| `loom-ops` | [loom-ops](https://github.com/kroq86/loom-ops) | Ops runbooks, HITL approve, incident supervisor |

```bash
pip install "loom-run[api,openai]"
pip install "loom-ops[api,telegram]"
```

Ops flow details: [loom-ops/docs/STACK.md](https://github.com/kroq86/loom-ops/blob/main/docs/STACK.md).

## Ecosystem MCP (optional)

| PyPI package | GitHub | Used with |
|--------------|--------|-----------|
| `docs-memory-mcp` | [mcp-docs-memory](https://github.com/kroq86/mcp-docs-memory) | [loom-run](https://github.com/kroq86/loom-run) — semantic docs search |
| `rule-based-verifier` | [rule-based-verifier](https://github.com/kroq86/rule-based-verifier) | loom-run — tests, lint, repo read via MCP |
| `ops-runtime-mcp` | [data-engineering-runtime-lab](https://github.com/kroq86/data-engineering-runtime-lab) | [loom-ops](https://github.com/kroq86/loom-ops) — ops runtime tools |

## Reference

| Repo | PyPI | Notes |
|------|------|-------|
| [agents_architecture](https://github.com/kroq86/agents_architecture) | — | Production coordinator patterns (reference) |
| [loom-stack](https://github.com/kroq86/loom-stack) | — | This documentation site |

## Choose a path

| Goal | Start here |
|------|------------|
| Library author | `loom-runner` + `loom-tailcalls` |
| Local dev agent | `loom-run` |
| Incident / runbook agent | `loom-ops` |
| Debug a run | `flow-xray` + `loom-runner explain` |

## Stack flow

```text
@tailrec loop  →  loom-runner  →  flow-xray --trace
                      ↑
            loom-run / loom-ops CLIs
```
