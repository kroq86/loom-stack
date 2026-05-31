# Loom stack — security

Hub for the five published packages. Full audit: [SECURITY_AUDIT_2026-05-31.md](SECURITY_AUDIT_2026-05-31.md).

## Packages

| PyPI | Security doc | Primary risks |
|------|--------------|---------------|
| [loom-tailcalls](https://pypi.org/project/loom-tailcalls/) | [loom-tailcalls README](https://github.com/kroq86/loom-tailcalls) | AST rewrite only; no I/O |
| [loom-runner](https://pypi.org/project/loom-runner/) | [loom-runner](https://github.com/kroq86/loom-runner) | SQLite checkpoints may hold tool payloads |
| [flow-xray](https://pypi.org/project/flow-xray/) | [flow-xray](https://github.com/kroq86/flow-xray) | Trace HTML may include args — use `redact=` |
| [loom-run](https://pypi.org/project/loom-run/) | [loom-run ENV](https://github.com/kroq86/loom-run/blob/main/docs/ENV.md) | HTTP + MCP + `run_tests` |
| [loom-ops](https://pypi.org/project/loom-ops/) | [loom-ops SECURITY](https://github.com/kroq86/loom-ops/blob/main/docs/SECURITY.md) | HTTP + optional shell + Telegram |

## Deploy checklist

1. **Local-first:** default HTTP bind `127.0.0.1` (`LOOM_RUN_HOST` / `LOOM_OPS_HOST`).
2. **Secrets:** use environment variables; never commit `.env`.
3. **Ops shell:** `LOOM_OPS_ALLOW_SHELL=1` only with a tight `ops.shell.allowlist.json`.
4. **MCP:** only connect servers you built or trust; config paths are absolute in examples.
5. **Traces / DB:** `runs.sqlite`, `ops.sqlite`, `*.html` traces — treat as confidential.

## Reporting

Open a private security issue on the affected GitHub repo or contact the maintainer listed on PyPI.
