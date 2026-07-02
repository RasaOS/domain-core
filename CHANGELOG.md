# CHANGELOG — `rasa.domain.core`

Reverse-chronological. Each entry is a version bump.

---

## 1.1.0 — 2026-07-01 — Universal opt-in starter layer

Additive, non-breaking. Forks pinned to v1.0.0 are unaffected; they may
adopt any of this opportunistically (keep what fits, delete the rest).
The template's core promise is intact: **zero domain knowledge**. Every
starter file references only the rasa Element contract (lockfile,
`pinned_sha`, `overrides[]`, install policies) — no subject material.

### Added

- **`content/README.md`** — fork-time guide to the universal starter
  layer: what ships, why it's domain-agnostic, how forks adopt/extend/
  delete it. Registered `policy: opt-in` (not installed into consumers;
  deleted at fork time alongside `SHAPE.md`).
- **Universal starter skills** under `content/skills/` (opt-in):
  - `new-skill/` — meta: scaffold a new skill following canonical conventions.
  - `codify/` — meta: promote a session-emergent rule into durable storage with its rationale.
  - `sync/` — meta: reconcile installed content against upstream; classify drift, respect overrides, advance `pinned_sha` only after applying.
  - `onboard/` — capability: read-only briefing orienting a session to the installed domain.
  - `handoff/` — capability: durable session checkpoint (done / in-flight / blocked / next / state). Now also refreshes a stable `.claude/HANDOFF.md` pointer linking the dated doc — the read target `resume` picks up.
  - `resume/` — capability: read-side counterpart to `handoff`. Reads the `.claude/HANDOFF.md` pointer (else discovers the newest dated handoff root-relative), calibrates it against git state since it was written, and leads with the one next action. Strictly read-only; distinct from `onboard` (which orients to the installed domain, not the unfinished work). Designed + adversarially verified via a multi-agent workflow.
  - `update-docs/` — capability: reconcile docs against reality (discover → reconcile → report → apply-on-approval), ported from `rasa.domain.code` and fully stripped of subject material — it discovers whatever docs a project has instead of assuming a fixed list. Forks extend the in-scope inventory with their own subject docs.
  - `README.md` — skill-authoring conventions (SKILL.md shape, plumbing-vs-judgment split, consent).
- **Universal starter rules** under `content/rules/` (opt-in):
  - `contract-rules.md` — the Element ↔ consumer discipline: lockfile is the source of truth, installed content is borrowed not owned, overrides must be recorded, changes flow one direction.
  - `output-rules.md` — house communication style (blunt/calibrated, faithful reporting, altitude, structured-ask → structured-response).
  - `README.md` — rule-authoring conventions + load order.
- **`content/agents/README.md`** (opt-in) — subagent conventions. Ships **no** starter agents by design: subagents are inherently subject-specific, so none is domain-agnostic enough to belong in the template.
- **Template enforcement layer** — the shape is templated first, then followed:
  - `content/templates/` (opt-in) — the enforced authoring skeletons: `SKILL.md.template`, `rule.md.template`, `agent.md.template` + README. Copy, fill every TODO — don't freehand. Authoring-time, not install-time (forks typically keep the entry opt-in).
  - **`bin/check-shape`** — the STRUCTURE gate, complementing `bin/check-manifest`'s INVENTORY gate. Pure-python, cross-platform, fork-safe (validates only what exists — a structural-shape domain with no `skills/` passes clean). Hard-fails on: skill dirs without `SKILL.md`, missing/mismatched frontmatter `name`, unfilled TODO trigger surfaces (skill/agent descriptions, rule H1s), missing required sections (`Behavior contract` / `Process` / `What NOT to do` / `Done when`), rule files not named `<topic>-rules.md`, agents missing `name`/`description`/`tools`. Warns on skills absent from the `rasa.json` enumerating note. Hardened by adversarial fixture testing: fence-quoted headings don't satisfy section checks, hyphenated skill names don't mask registry warnings, non-UTF-8 files get a diagnostic instead of a crash. Accepts an optional root arg for validating a fork from anywhere.
  - `content/rules/authoring-rules.md` — the structured process, codified so every fork inherits it: template → fill → validate (`check-shape` + `check-manifest`, both release gates) → register → record.
  - `new-skill` now scaffolds by copying `SKILL.md.template`; the skills/rules/agents READMEs point at the templates as the single source of shape; `contract-rules.md`'s pre-tag checklist gains the `check-shape` gate.
  - Dogfooded: the validator immediately caught real drift in our own content (`update-docs` shipped with variant headings from its domain-code port — normalized to the canonical sections).

### Changed

- `VERSION`: 1.0.0 → 1.1.0
- `rasa.json#version`: 1.0.0 → 1.1.0
- `rasa.json#description` + `element.description` — reframed around the v1.1.0 starter layer.
- `rasa.json#element.files[]` — added the `content/README.md` entry; refreshed the `skills/`/`rules/`/`agents/` notes to describe the starter set they now carry. All remain `policy: opt-in`.
- **`seed/rasa.lock.json.template`** — added `"version": "{{ELEMENT_VERSION}}"` under `element`. `bin/init` already substituted `{{ELEMENT_VERSION}}`, but no seed file consumed it — the substitution was dead. Now the stamped lockfile records the installed Element version, not just the pinned SHA. (Reconciles a v1.0.0 init/manifest inconsistency.)
- **`seed/CLAUDE.md.template`** — reworked the seeded per-project contract: added a "Getting oriented" on-ramp (`/onboard` for a new session, `.claude/HANDOFF.md`/`/resume` for in-flight work), rewrote the update section to reference the starter-layer `/sync` skill and `overrides[]` discipline, and clarified in the header that identity is intentionally deferred to `.claude/rasa.lock.json` (never hardcoded, so it can't drift). rasa.json seed note reframed: placeholder-free is by-design (lockfile is the live source of truth), not a taxonomy gap.
- Removed `content/{skills,rules,agents}/.gitkeep` — superseded now the directories carry real starter files.
- `README.md` / `content/SHAPE.md` — updated to describe the starter layer instead of empty `.gitkeep` scaffold.

### Fixed

- **`bin/init`** — the `opt-in` element.files handler created the target
  directory before checking the policy, leaving empty `.claude/skills/`,
  `.claude/agents/`, and `content/` dirs in every consumer project.
  Opt-in now materializes nothing in the target. (Cosmetic, pre-existing
  since v1.0.0.)
- **Stale seed placeholder** — `seed/CLAUDE.md.template` no longer ships the dead `# (replace with actual /sync invocation once the domain Element authors one)` line; the starter layer now provides `/sync`, and the template references it.
- **Stale doc references** — `CLAUDE.md` v0.x-era instructions retired
  (the "don't ship LICENSE without a decision" Don't, pre-1.0 version-bump
  examples); `bin/init` header comment no longer claims "ships zero
  content"; `README.md` "does NOT ship" list clarified (subject-specific
  content only — the agnostic starter layer is the exception).

### Fork guidance

A fork keeps whatever starter files fit its shape pattern, deletes the
rest, adds its own, and flips the relevant `content/` policies from
`opt-in` to `directory-mirror`/`file-replace` to install into consumer
projects. Run `bin/check-manifest` after any add/remove.

---

## 1.0.0 — 2026-05-24 — LOCKED unified shape (Phase 2 complete)

First stable lock-down. Unified shape derived from side-by-side
analysis of `rasa.domain.code` (toolkit pattern) and `rasa.domain.legal`
(structural pattern). Forks may pin here; subsequent minor bumps
are additive.

### Added

- **`LICENSE`** — Apache-2.0 (matches claude-kit + domain-legal historical lineage).
- **`bin/check-manifest`** — cross-platform pure-python script (adapted from `rasa.domain.legal/bin/check-manifest`). Verifies rasa.json is a complete inventory of `content/` + `seed/` (coverage check) and that every rasa.json `from` entry points to an existing file (dangling check). Run before tagging an Element release; wire into CI.
- **`content/SHAPE.md`** — fork-time guide documenting the two shape patterns:
  - **Pattern 1 — Toolkit domain** (skills/rules/agents/build/tests like `rasa.domain.code`)
  - **Pattern 2 — Structural domain** (firm/drives/members like `rasa.domain.legal`)
  - **Pattern 3 — Hybrid** (mix of both; document clearly in fork's `content/README.md`)
  - The template itself is shape-agnostic; forks pick at fork time.
- **`content/{skills,rules,agents}/.gitkeep`** — toolkit-shape opt-in scaffold. Registered in rasa.json as `policy: opt-in` so they're tracked + check-manifest passes, but NOT installed automatically. Structural-shape forks delete + remove entries.

### Changed

- `rasa.json#version`: 0.1.0 → 1.0.0
- `VERSION`: 0.1.0 → 1.0.0
- `rasa.json#description` — rewritten around Phase 2 lock-down framing.
- `rasa.json#rasa` — added `role: "domain-template"` + `shape_pattern: "agnostic"` declarations.
- `rasa.json#element.files[]` — populated with 4 opt-in scaffold entries (SHAPE.md + skills/ + rules/ + agents/) for check-manifest coverage. Forks change policies as they populate.
- `README.md` — Phase 2 decisions enumerated; v1.0.0 file layout; what-ships vs what-doesn't tables; versioning intent post-1.0; surfaced drift (domain-code missing CLAUDE.md) flagged.
- `CLAUDE.md` — Phase 2 section marked COMPLETE; toolkit vs structural framing locked in.
- `CHANGELOG.md` — this entry.

### Phase-2 decision table

| # | Question | Decision |
|---|---|---|
| 1 | `bin/check-manifest` required? | **YES** (pure-python form) |
| 2 | `bin/lint` in shape? | **NO** (domain-code-specific) |
| 3 | `tasks/` in shape? | **NO** (domain-code-specific) |
| 4 | `docs/` in shape? | **NO** (domain-legal-specific) |
| 5 | More universal seed templates? | **NO** (just CLAUDE.md.template + rasa.lock.json.template) |
| 6 | Universal `content/<subdirs>/`? | **None enforced**; toolkit scaffold shipped opt-in |
| 7 | LICENSE? | **YES — Apache-2.0** |
| 8 | Stripped seed example? | **NO** (two templates suffice) |

### Subtle install-policy gap (still open)

`skip-if-exists` policy doesn't substitute placeholders; `init-only-with-sha` does but is documented as lockfile-specific. Current workaround: `seed/CLAUDE.md.template` is placeholder-free (Element identity available in `.claude/rasa.lock.json`). A future canon revision could add a `seed-with-substitution` policy to fill the taxonomy hole. Not blocking v1.0.0.

### Surfaced drift (NOT fixed here, per role-split)

- **`rasa.domain.code` is missing `CLAUDE.md` at root** (canon ELEMENT_CONTRACT §4 violation). Domain-legal has it; domain-code doesn't. Flagged in canon AUDIT for the domain-code team to fix next time they touch the repo.
- **`rasa.domain.code` CLAUDE.md still uses legacy "kit" terminology** in its `docs/` (caught earlier in domain-code audit). Not Phase-2's job to fix.

### Smoke-tested

- `bin/check-manifest` → OK (5 tracked files under content/ + seed/, all registered)
- `bin/init /tmp/dc-test` → clean install (2 files: CLAUDE.md + .claude/rasa.lock.json; opt-in scaffolds correctly NOT copied)

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
