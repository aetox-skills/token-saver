---
name: token-saver
description: Token Saver (RTK Protocol) — Load this skill before running bash commands this session. CLI proxy that filters/strips command output before it reaches LLM context. Saves 60-90% tokens on git, test, install, lint, build, cloud, containers. Default-on: try rtk for any long-output command, not just the listed ones. Includes Tee Recovery Protocol for failures. Platform-agnostic.
---

# ⚡ Token Saver — RTK Protocol

> **AI Agent Protocol:** Load this skill (`skill("token-saver")`) **before running any bash command** this session.
> **Default-on rule:** if a command produces long output, try `rtk` first. Only skip when output is known-trivial.
> **Saves 60-90%** of tokens wasted on progress bars, passed test boilerplate, long file lists.

## Core Rule (Default-On)

**Before every bash command, ask:**
> "Will this output be more than ~5 lines / progress bars / boilerplate?"

| If... | Then... |
|-------|---------|
| Output is **long or has noise** | Prefix with `rtk` (e.g. `rtk git status`, `rtk cargo test`, `rtk npm install`, `rtk terraform plan`, `rtk kubectl get pods`) |
| Output is **short or deterministic** (1-3 lines) | Run directly |
| You need **full raw output** | Use `rtk proxy <cmd>` (passthrough, still tracks stats) |
| Command **fails** | Read the tee file — full error is saved there |

**This covers ALL commands** — not just the ones in the table below. RTK supports 100+ command types out of the box.

---

## Tier 1: High-Frequency Commands

These are the most common commands where RTK saves the most. Use as a quick reference.

| Category | Try `rtk` | Why |
|----------|-----------|-----|
| **Git** | `git status/log/diff/add/commit/push` | compresses 75-92% |
| **Test** | `pytest`, `cargo test`, `go test`, `jest`, `vitest` | strip passed boilerplate, show only failures |
| **Install** | `npm/pnpm/pip install`, `uv pip install`, `bundle install` | progress bars = garbage |
| **Find** | `find ...` | tree output instead of flat list |
| **Containers** | `docker ps/images/build/logs`, `kubectl get pods/logs` | strip SHA/container IDs |
| **TypeScript** | `tsc`, `tsc --noEmit` | error output wall (if fail → tee recovery) |
| **Curl/API** | `curl ...` | auto detect JSON → schema |
| **JSON** | `json file.json` | compact / keys-only |

| Category | Skip `rtk` | Why |
|----------|------------|-----|
| **Trivial** | `echo`, `cp`, `mv`, `mkdir`, `which`, `Test-Path` | output = 1-2 tokens |
| **Code review** | `git diff` (when reviewing) | need full diff context |

---

## Tier 2: Also Supported (Just Use the Default-On Rule)

RTK filters 100+ commands across these ecosystems. You don't need to memorize them — just apply the **default-on rule**:

- **Linters:** `ruff check`, `eslint`, `biome`, `mypy`, `golangci-lint`, `rubocop`, `cargo clippy`
- **Cloud/Infra:** `terraform`/`tofu` plan/apply, `kubectl` (pods/services/logs), `aws` CLI, `helm`, `pulumi`
- **Build tools:** `cargo build`, `go build`, `next build`, `gradle`, `maven`
- **System:** `ls`, `tree`, `wc` (rtk has compact versions)
- **Logs:** any log file (rtk deduplicates repeated lines)

If you encounter a new tool: **assume `rtk` works until proven otherwise.** If it doesn't filter well, `rtk proxy` runs it raw.

---

## Before & After

### `git status` — 30 lines → 5 lines

```
# raw:                              # rtk git status:
On branch main                      ok main
Changes not staged for commit:
  modified: README.md (src/)
  modified: SKILL.md (src/)
Untracked files:
  token-saver.md
```

### `pytest` — 200 lines → 3 lines

```
# raw (200 lines of dots + tracebacks):    # rtk pytest:
test_session.py ............               FAILED: 2/47 tests
test_api.py ..........................      test_auth: assertion line 42
test_models.py ..........................   test_timeout: timeout error
test_auth.py ..F...
test_timeout.py ........F
```

### `npm install` — 60 lines → silence (pass) or 1 line (error)

```
# raw: added 1,234 packages, audit report...   # rtk npm install: ok
```

### `tsc --noEmit` — full error wall → summary

```
# raw: 200 lines of errors              # rtk tsc --noEmit:
src/auth.ts:12:3 - error TS2322         FAILED: 3 errors
src/auth.ts:45:1 - error TS2554          [tee: .../tee/xxx.log]
src/api.ts:88:5 - error TS18046         ← AI reads tee for full errors
```

---

## ⚡ Tee Recovery Protocol (CRITICAL)

When a filtered command **fails**, RTK saves the full unfiltered output to a tee file.

```
1. Run: rtk tsc --noEmit
2. Output: FAILED: 3 errors  [full output: /path/to/rtk/tee/xxx.log]
3. AI MUST: Read that file → see full error → fix code
4. NEVER: guess the error, ignore the tee file, or ask user to re-run
```

| OS | Tee path |
|----|----------|
| Windows | `C:\Users\<user>\AppData\Local\rtk\tee\` |
| Linux/Mac | `~/.local/share/rtk/tee/` |

```toml
[tee]
enabled = true
mode = "failures"
max_files = 20
max_file_size = 1048576
```

---

## Agent Workflow

```
Session start → load skill token-saver
         │
         ▼
About to run bash command?
   │
   ├─ Will output be >5 lines / progress / boilerplate?
   │     ├─ YES → rtk <command>
   │     └─ NO  → run directly
   │
   ├─ Need full raw output?
   │     └─ YES → rtk proxy <command>
   │
   └─ Command failed with rtk?
         └─ YES → Read tee file → fix from real error
```

---

## Prerequisites

```bash
rtk --version   # must show a version (any v0.x)
rtk gain        # verify it's the right rtk (if error → wrong package installed)
```

## Analytics

```bash
rtk gain              # summary stats
rtk gain --history    # recent command history
```

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| `rtk gain` fails | You installed the wrong `rtk` (Rust Type Kit). Install from `cargo install --git https://github.com/rtk-ai/rtk` |
| Auto-rewrite not working | Restart your AI tool, or use `rtk` prefix manually |
| Tee file not created | Check `rtk config` has `[tee] enabled = true` |
| Output too aggressive | Use `rtk proxy <cmd>` for raw passthrough |

---

> **Bottom line:** Default-on with tee fallback. Try `rtk` first for anything that looks noisy.
> Skip only when you're sure output is short. Read tee on failure. Never guess errors.
