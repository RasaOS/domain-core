# CHANGELOG — `rasa.domain.core`

Reverse-chronological. Each entry is a version bump.

---

## 0.1.0 — 2026-05-24

### Initial release — stripped-down domain template

First version. Canon-shaped per ELEMENT_CONTRACT.md v1.3.0. Holds
zero domain knowledge — it's the SHAPE every domain Element shares.

### Required files at Element root (canon §4)

All present:

- `rasa.json` — kind=domain, contract_version=1.3.0
- `VERSION` (0.1.0)
- `README.md` (orient new forks)
- `CLAUDE.md` (per-repo session contract)
- `CHANGELOG.md` (this file)
- `.gitignore` (macOS + editor cruft)

### Skeleton structure

- `content/skills/.gitkeep` (empty — forks add their skills here)
- `content/rules/.gitkeep` (empty — forks add their rules here)
- `content/agents/.gitkeep` (empty — forks add their subagents here)
- `seed/CLAUDE.md.template` (per-project session contract template)
- `seed/rasa.lock.json.template` (canonical lockfile shape)
- `bin/init` (minimal install script — applies rasa.json policies + stamps lockfile)

### What's intentionally NOT here

This v0.1.0 is intentionally stripped. Phase 2 (next session on this
Element) reviews `domain-code` + `domain-legal` to extract the
genuinely-shared shape beyond canon required-files and lock it down
in v1.0.0. Open Phase-2 questions listed in `CLAUDE.md` "Phase 2 —
Unified template work" section.

Specifically deferred to Phase 2:

- `bin/check-manifest` (domain-code has; domain-legal doesn't)
- `bin/lint` (domain-code only)
- `tasks/` (work-tracking folder — may or may not be part of the
  domain shape)
- `docs/` (domain-legal has; domain-code doesn't)
- Universal seed/ templates (beyond CLAUDE.md.template + rasa.lock.json.template)
- LICENSE (Apache 2.0 planned to match claude-kit lineage; formal
  adoption in Phase 2)

### Versioning intent

- v0.x = "still defining the shape" range
- v1.0.0 = first stable lock-down (Phase 2 completion)
- Post-v1.0 minor = new required file added; forks adopt opportunistically
- Post-v1.0 major = breaking shape change; forks REQUIRED to migrate
