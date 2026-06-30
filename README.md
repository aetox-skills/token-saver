# ⚡ Token Saver — RTK Protocol

> **AI Agent Protocol:** Always prefix noise-producing bash commands with `rtk`.
> Recovers full error output via tee files when commands fail.
> **Saves 60-90%** of LLM tokens wasted on progress bars, passed test boilerplate, and long file listings.

Token Saver is an AI agent skill that wraps [RTK](https://github.com/rtk-ai/rtk) (Rust Token Killer, 67k⭐) — a CLI proxy that intercepts bash commands and strips redundant output before it reaches the LLM context.

## Why

Every bash command you run produces output. Most of it is noise:
- `npm install` → progress bars, dep tree, audit report
- `pytest` → 200 lines of green dots saying "passed"
- `git status` → 30 lines for 5 files
- `tsc` → wall of errors when you only need the relevant ones

That noise eats context tokens. RTK (via this skill) strips it. **Only failures pass through fully** via tee recovery.

## Quick Start

```bash
# Check RTK is installed
rtk --version

# Load this skill
skill("rtk")

# Use it
rtk git status
rtk npm install
rtk pytest
```

## Quick Reference

| Always `rtk` | Skip `rtk` |
|---|---|
| `git`, `pytest`, `cargo test`, `npm/pip install` | `echo`, `cp`, `mv`, `mkdir`, `which` |
| `find`, `docker`, `curl`, `json`, `tsc` | `git diff` (code review) |

Failure → **read the tee file**. See [`SKILL.md`](SKILL.md) for full protocol.

## Repo Structure

```
token-saver/
├── SKILL.md         # Agent skill — load with skill("rtk")
├── README.md        # This file
├── INSTALL.md       # Setup for different AI coding tools
├── CHANGELOG.md     # Version history
└── LICENSE          # Apache 2.0
```

## Related

- [RTK Official](https://github.com/rtk-ai/rtk) — the tool itself (67k⭐)
- [Aetox Skills](https://github.com/aetox-skills/aetox-skills) — skill catalog
