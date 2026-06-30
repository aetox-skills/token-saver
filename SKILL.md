---
name: rtk
description: RTK Protocol — Load this skill before running bash commands this session. CLI proxy that filters/strips command output before it reaches LLM context. Saves 55-90% tokens on git, test, install, find, docker, curl, json, tsc. Includes Tee Recovery Protocol for failure recovery. Platform-agnostic: works with any AI coding tool.
---

# ⚡ Token Saver — RTK Protocol

> **AI Agent Protocol:** Load this skill (`skill("rtk")`) **before running any bash command** this session.
> RTK intercepts bash commands and strips noise before output reaches the LLM.
> **Saves 55-90%** of tokens wasted on progress bars, passed test boilerplate, long file lists.

## Iron Rules

### ✅ Always use `rtk` prefix

| Command | Use | Reason |
|---------|-----|--------|
| `git status/log/diff/add/commit/push` | `rtk git ...` | compresses 53-96% |
| `pytest`, `cargo test`, `go test`, `jest`, `vitest` | `rtk pytest/cargo test ...` | strip passed boilerplate, show only failures |
| `npm/pnpm/pip install` | `rtk npm/pnpm/pip install` | progress bars = garbage tokens |
| `find ...` | `rtk find ...` | tree output instead of flat list |
| `docker ps/images/build` | `rtk docker ...` | strip SHA/container IDs |
| `curl ...` | `rtk curl ...` | auto detect JSON → schema output |
| `json file.json` | `rtk json ...` | compact / keys-only mode |
| `tsc --noEmit`, `tsc` | `rtk tsc` | large error output (if fail → tee recovery) |

### ❌ Skip rtk

| Command | Reason |
|---------|--------|
| `echo`, `Write-Output`, `which`, `Test-Path` | output = 1-2 tokens |
| `mkdir`, `New-Item`, `Remove-Item` | output = nothing |
| `git diff` when reviewing code | need full diff context, not just changed lines |

### 🚨 Bypass when raw output needed

```bash
rtk proxy <cmd>   # passthrough — no filtering, still tracks stats
```

---

## ⚡ Tee Recovery Protocol (CRITICAL)

When a filtered command **fails**, RTK saves the full unfiltered output to a tee file.

**Flow:**
```
1. Run: rtk tsc --noEmit
2. Output: FAILED: 3 errors  [full output: /path/to/rtk/tee/xxx.log]
3. AI MUST: Read that file → see full error → fix code
4. NEVER: guess the error, ignore the tee file, or ask user to re-run
```

**Tee file paths by OS:**
| OS | Path |
|----|------|
| Windows | `C:\Users\<user>\AppData\Local\rtk\tee\` |
| Linux/Mac | `~/.local/share/rtk/tee/` |

**Default config:**
```toml
[tee]
enabled = true
mode = "failures"    # save full output only on failure
max_files = 20
max_file_size = 1048576
```

**Applies to any filtered command that fails:**
`rtk tsc`, `rtk pytest`, `rtk cargo test`, `rtk go test`, `rtk jest`, etc.

---

## Agent Workflow

```
┌─────────────────────────────────────────────────────┐
│  1. Load skill rtk at session start                 │
├─────────────────────────────────────────────────────┤
│  2. About to run bash command?                      │
│     ├─ Long output / noise → prefix with rtk        │
│     └─ Small output / data → run directly           │
├─────────────────────────────────────────────────────┤
│  3. Command failed?                                 │
│     ├─ YES → Read tee file for full error output    │
│     └─ NO  → ✓ Tokens saved, continue               │
├─────────────────────────────────────────────────────┤
│  4. Fix code based on real error from tee file      │
└─────────────────────────────────────────────────────┘
```

**Before any command, ask:**
- Is this output **noise** or **data**? → noise = rtk, data = raw or `rtk proxy`
- If rtk fails → tee recovery — **always read the tee file**

---

## Prerequisites

```bash
rtk --version   # must show v0.34.x+
rtk gain        # check token savings
```

## Analytics

```bash
rtk gain              # summary stats
rtk gain --history    # recent command history
rtk gain --daily      # day-by-day breakdown
```

---

> **Bottom line:** RTK spends tokens where it should (failures → full error via tee)
> and saves where it should (passes → compact summary).
> A good agent = uses rtk, recovers from tee, never guesses errors.
