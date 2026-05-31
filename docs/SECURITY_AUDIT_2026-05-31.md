# Loom stack security audit — 2026-05-31

Scope: five published PyPI projects (`loom-tailcalls`, `loom-runner`, `flow-xray`, `loom-run`, `loom-ops`).  
Out of scope: PyPI API token management, contents of local `.env` files.

## Summary

| Area | Result |
|------|--------|
| Git tracked secrets | **PASS** — no `.env`, `*.sqlite`, or keys in tracked files (5 repos) |
| `src/` hardcoded secrets | **PASS** — no `sk-`, `ghp_`, `AKIA`, PEM in `src/` |
| PyPI wheels (downloaded 2026-05-31) | **PASS** — no `.env`, sqlite, tests, or PEM inside wheels |
| GitHub Actions publish | **PASS** — release / `workflow_dispatch` only; `contents: read` |
| GitHub Actions tests | **PASS** — no secrets on `pull_request` |
| Runtime high findings | **MITIGATED** — allowlist tightened, `run_tests` path guard, MCP env filter (see PRs) |

## Per-package

| PyPI | Git scan | Wheel | CI | Runtime notes |
|------|----------|-------|-----|----------------|
| `loom-tailcalls` | PASS | PASS (7 files) | publish OK | Library only; no network surface |
| `loom-runner` | PASS | PASS (12 files) | publish OK | SQLite at consumer path; no HTTP in package |
| `flow-xray` | PASS | PASS (16 files) | publish OK | Use `@trace(redact=...)` for sensitive fields |
| `loom-run` | PASS | PASS (29 files) | publish + tests OK | HTTP unauthenticated; bind `127.0.0.1` default; tools bounded |
| `loom-ops` | PASS | PASS (30 files) | publish + tests OK | HTTP + optional shell/Telegram/MCP |

## Phase 1 — Repository scan

**Tools:** `git ls-files`, `git check-ignore`, `rg` on `src/` (gitleaks/trufflehog not installed locally).

| Check | loom | loom-agent | flow-xray | loom-run | loom-ops |
|-------|------|------------|-----------|----------|----------|
| Tracked `.env` / sqlite | none | none | none | none | none |
| `.env` in `.gitignore` | yes | **was no** → fixed | yes | yes | yes |
| `sk-` in `src/` | none | none | none | none | none |

**History:** `git log -S 'sk-'` hits in `flow-xray` / `loom-ops` are doc-only commits (ENV examples), not live keys.

## Phase 2 — PyPI artifacts

Downloaded from PyPI into `/tmp/loom-audit`:

- `loom_tailcalls-0.2.1-py3-none-any.whl`
- `loom_runner-0.1.1-py3-none-any.whl`
- `flow_xray-0.3.4-py3-none-any.whl`
- `loom_run-0.2.0-py3-none-any.whl`
- `loom_ops-0.2.0-py3-none-any.whl`

No suspicious paths in any archive. Entry points: `loom-run`, `loom-ops` CLIs only (plus library modules).

## Phase 3 — GitHub Actions

| Repo | Publish trigger | PR secrets | permissions |
|------|-----------------|------------|-------------|
| loom-tailcalls | `release`, `workflow_dispatch` | tests: no secrets | `contents: read`, `id-token: write` |
| loom-runner | same | same | same |
| flow-xray | same | same | same |
| loom-run | twine publish; same triggers | tests + mcp-smoke: no secrets | `contents: read` |
| loom-ops | twine publish; same triggers | tests: no secrets | `contents: read` |

No `pull_request_target` publish workflows. Fork PRs cannot read publish secrets.

## Phase 4 — Runtime findings

### High (addressed or documented)

| ID | Finding | Status |
|----|---------|--------|
| R1 | `loom-run` / `loom-ops` FastAPI: no auth on `/chat`, `/supervise`, `/explain` | **Accepted** — document bind localhost + reverse proxy; optional `LOOM_*_API_KEY` deferred |
| R2 | `loom-ops` shell allowlist included `python -c` | **Fixed** — removed from [`ops.shell.allowlist.json`](https://github.com/kroq86/loom-ops/blob/main/ops.shell.allowlist.json) |

### Medium (addressed or documented)

| ID | Finding | Status |
|----|---------|--------|
| R3 | `loom-run` `run_tests(path)` path not validated | **Fixed** — workspace-relative guard |
| R4 | MCP child inherits full parent `os.environ` | **Fixed** — denylist prefixes before spawn |
| R5 | `.loom-ops/memory.jsonl` may store run context | **Documented** — treat as sensitive; rotate/delete file if leaked |

### Low

| ID | Finding | Status |
|----|---------|--------|
| R6 | `loom-agent` missing `.env` in `.gitignore` | **Fixed** |
| R7 | Docs show `sk-...` placeholders | **OK** — not real keys |

## Phase 5 — Hardening shipped

- `loom-ops`: allowlist + expanded [`docs/SECURITY.md`](https://github.com/kroq86/loom-ops/blob/main/docs/SECURITY.md)
- `loom-run`: `run_tests` path guard, MCP env filter, test
- `loom-agent`: `.gitignore` for `.env`
- `loom-stack`: this audit + [`docs/SECURITY.md`](SECURITY.md) hub
- `loom-ops`: CI `security.yml` secret-pattern scan on `src/`

## Operational recommendations

1. Enable **PyPI 2FA** on the publisher account.
2. Never run `loom-run serve` / `loom-ops serve` on `0.0.0.0` without TLS and auth.
3. Keep `LOOM_OPS_ALLOW_SHELL=0` unless allowlist is tailored to real runbooks.
4. Set `TELEGRAM_ALLOWED_CHAT_IDS` to explicit IDs (empty list already denied at startup).
5. Re-run this audit after major releases (wheel download + `rg` scan).

## Human gates (open)

- Optional Bearer token for HTTP (`LOOM_RUN_API_KEY` / `LOOM_OPS_API_KEY`) — not implemented in v0.2.x.
- Production shell allowlist entries — operator-defined per environment.
