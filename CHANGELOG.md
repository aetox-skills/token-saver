# Changelog

## v0.1.2 (2026-07-01)

- **Structural: changed from allow-list to default-on rule** — "try rtk for any long-output command" instead of listing specific commands
- Tier 1: high-frequency commands (git, test, install, containers, kubectl) as quick reference
- Tier 2: default-on covers all other ecosystems (linters, cloud, infra, build tools, system)
- Add: troubleshooting section (rtk gain fail, auto-rewrite not working, tee not created)

## v0.1.1 (2026-07-01)

- Fix: token savings claim 55-90% → 60-90% (match upstream)
- Fix: git compression 53-96% → 75-92% (match upstream)
- Fix: remove hardcoded version pin, use `rtk --version` verify instead
- Add: before/after examples for git, pytest, npm install, tsc
- Add: name collision warning (Rust Type Kit on crates.io)
- Reduce: README.md → overview only, SKILL.md is source of truth

## v0.1.0 (2026-07-01)

- Initial release
- SKILL.md: RTK Protocol with iron rules, tee recovery workflow, agent workflow diagram
- README.md: overview, quick start, repo structure
- INSTALL.md: cross-platform setup guide
- LICENSE: Apache 2.0
