# ⚡ Token Saver — RTK Protocol

> **Default-on protocol:** Try `rtk` for any command with long output. Skip only when output is trivially short.
> **Saves 60-90%** of tokens wasted on progress bars, boilerplate, and long listings.
> Recovers full error output via tee files when commands fail.

Token Saver is an AI agent skill that wraps [RTK](https://github.com/rtk-ai/rtk) (Rust Token Killer, 67k⭐) — a CLI proxy that intercepts bash commands and strips redundant output before it reaches the LLM context.

## Quick Start

```bash
rtk --version          # check installed
skill("rtk")           # load this skill
rtk git status         # use it
```

## Core Principle

1. **Long/noisy output → `rtk` prefix.** Always.
2. **Short output → run directly.**
3. **Failure → read the tee file** for full error output.

See [`SKILL.md`](SKILL.md) for the full protocol (tee recovery, before/after examples, troubleshooting).

## Related

- [RTK Official](https://github.com/rtk-ai/rtk) (67k⭐)
- [Aetox Skills Catalog](https://github.com/aetox-skills/aetox-skills)
