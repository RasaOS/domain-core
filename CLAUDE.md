# CLAUDE.md — `rasa.domain.core`

Per-repo working contract for Claude sessions opened inside this
folder. Extends `~/.claude/CLAUDE.md` and the workspace
`~/rAI/rasa-os/CLAUDE.md` (which is the `rasa.tenant.rasaos` tenant's
CLAUDE.md); does not override them.

## What you are when you're in this folder

You are working on **`rasa.domain.core`** — the stripped-down canonical
template every `domain` Element follows. The Element ships **zero
domain knowledge**; it's the SHAPE every domain shares.

Two distinct concerns:
- **The template** (this folder) — the canonical shape. Lives at
  `~/rAI/rasa-os/elements/domain-core/`. Pushed to `RasaOS/domain-core`
  (public).
- **The forks** (e.g., `domain-code`, `domain-legal`) — concrete domain
  Elements that adopt this shape and populate it with domain-specific
  skills, rules, agents.

When the template's shape changes (Phase 2 unification work + future
revisions), forked domains may need to absorb the change. Versioning
matters: domain-core v0.x → v1.0 is the lock-down moment.

## Source of truth

- **`~/rAI/rasa-os/canon/`** — authoritative for everything architectural.
  Current LOCKED is v1.2.0; current WORKING is v1.3.0 IN PROGRESS.
  Spec §6 defines the `domain` kind; ELEMENT_CONTRACT.md is the
  contract domain-core conforms to.
- **`~/rAI/rasa-os/CLAUDE.md`** — workspace orientation (the
  `rasa.tenant.rasaos` tenant's working contract); the role-split is
  there.
- **This folder's `README.md`** — what this Element is, how to fork it,
  what's deliberately stripped.
- **This folder's `rasa.json`** — formal declaration.

## Don'ts

- **Don't add domain-specific content here.** This is the template;
  domain-specific material goes in a fork (`domain-code`, `domain-legal`,
  etc.). If a piece of content is universal to ALL domains, it MAY
  land here — but that's a Phase-2 unification decision, not casual
  authoring.
- **Don't bin/init this Element into itself.** Like every Element
  source repo: `domain-core/.claude/` is for sessions; `domain-core/content/`
  is the source. They don't duplicate.
- **Don't conflate with `rasa.core`** (kind: `core`). Different
  Element. `rasa.core` is the L1 shared bones every Element depends on
  (vocabulary, output styles, stamp definitions). `rasa.domain.core`
  is the template for the `domain` KIND. They share a word, not a
  concern.
- **Don't ship LICENSE in v0.1.x without a decision pass.** Apache 2.0
  is the planned default (matches claude-kit historical lineage), but
  formal adoption belongs in Phase 2.
- **Don't bump the major version casually.** v0.x is the "still
  defining the shape" range. v1.0.0 is the lock-down moment that
  signals "this shape is stable; forks may pin and update predictably."

## How a version bump works

- **Patch (0.1.0 → 0.1.1)** — typo fix, README clarification, minimal
  bin/init bug fix. No structural change.
- **Minor (0.1.x → 0.2.0)** — new required file added to the template,
  new content/<subdir>/, new seed/ template, capability category. Forks
  may need to adopt but aren't broken.
- **Major (0.x.x → 1.0.0)** — first stable lock-down OR a breaking
  shape change after 1.0. Forks REQUIRED to migrate.

Each bump: edit `VERSION`, update `rasa.json#version`, write a
CHANGELOG.md entry. Commit + tag + push. Add a row to
`~/rAI/rasa-os/elements/CHANGELOG.md` (aggregated track #2). Update
`~/rAI/rasa-os/elements/REGISTRY.md` if the row's values change.

## Phase 2 — Unified template work (COMPLETE in v1.0.0)

Phase 2 reviewed `rasa.domain.code` (toolkit pattern) and
`rasa.domain.legal` (structural pattern) side-by-side. Found that
these aren't variants of one template — they're **two genuinely
different domain shape patterns**:

- **Toolkit domain** (domain-code) — ships skills/rules/agents in
  `content/`. 78 skill folders, 27 root rule files, 8 capabilities.
- **Structural domain** (domain-legal) — ships organizational
  templates (firm/, drives/, members/) in `content/`. 3 dirs, no
  skills. 4 cross-platform bin scripts including `register-user`.

Both ship the Element primitive (rasa.json + content/ + seed/ +
bin/init); the internal shape of `content/` is what differs. The
unified template enforces **only what's truly universal** and
documents both patterns as valid choices via `content/SHAPE.md`.

The 8 Phase-2 questions (decisions documented in CHANGELOG v1.0.0):

1. ✓ `bin/check-manifest` — **included** (pure-python form from domain-legal; cross-platform)
2. ✗ `bin/lint` — domain-code-specific; not in core
3. ✗ `tasks/` — domain-code-specific; not in core
4. ✗ `docs/` — domain-legal-specific; not in core
5. ✗ Universal seed templates beyond CLAUDE.md + rasa.lock.json — none more
6. ⚠ Universal `content/<subdirs>/` — none enforced; toolkit scaffold shipped as opt-in
7. ✓ LICENSE — **Apache-2.0**
8. ✗ Stripped seed example — two is enough

When Phase 2 closed, v0.1.0 → v1.0.0 with the locked unified shape.

## What changes post-v1.0.0 mean for forks

- **Patch** — bug fix, no fork action needed.
- **Minor** — additive (new canonical bin tool, new universal seed
  template, new optional scaffold). Forks may adopt opportunistically;
  not adopting is fine.
- **Major** — breaking change. Forks REQUIRED to migrate. Avoid
  without explicit user direction.

A fork pinning to `domain-core v1.0.0` is committing to the v1.0
shape forever; absorbing v1.x minors is optional.

## What success looks like for this Element

- A new domain (e.g., `rasa.domain.health`) can be authored by forking
  this Element + filling content/+seed/ — no other scaffolding needed.
- The shape decisions are documented (README + this CLAUDE.md +
  CHANGELOG) so forks understand what they're conforming to.
- When the shape evolves (post-v1.0), forks have a clear migration
  path (CHANGELOG calls out what changed + how to absorb).
- `domain-code` and `domain-legal` both align with the v1.0 locked
  shape — proof the unification is real, not aspirational.
