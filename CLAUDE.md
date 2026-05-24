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

## Phase 2 — Unified template work

The current v0.1.0 is intentionally stripped. **Phase 2** (next session
on this Element) reviews `domain-code` + `domain-legal` side-by-side,
extracts the genuinely-shared shape, and locks it down here.

Open Phase-2 questions:
1. Is `bin/check-manifest` required for every domain (domain-code has;
   domain-legal doesn't)?
2. Is `bin/lint` part of the shape (domain-code only)?
3. Is `tasks/` (work-tracking folder structure) part of the domain
   shape, or just the canon-author tenant's concern?
4. Is `docs/` part of the shape (domain-legal has firm/conduct/policy
   docs; domain-code doesn't)?
5. What seed/ templates are universal? (rasa.lock.json.template,
   CLAUDE.md.template, AUDIT.md.template, etc. — domain-code has 20+;
   domain-legal has none)
6. What `content/<subdirs>/` are universal? (skills/, rules/, agents/
   are common; domain-legal adds drives/, firm/, members/ which are
   legal-specific)
7. License decision (Apache 2.0 like the historical claude-kit
   lineage? something else?)
8. What's a "stripped seed example" that ships in v1.0 to show forks
   how to author their seed templates?

When Phase 2 completes, v0.1.0 → v1.0.0 with the locked unified
shape.

## What success looks like for this Element

- A new domain (e.g., `rasa.domain.health`) can be authored by forking
  this Element + filling content/+seed/ — no other scaffolding needed.
- The shape decisions are documented (README + this CLAUDE.md +
  CHANGELOG) so forks understand what they're conforming to.
- When the shape evolves (post-v1.0), forks have a clear migration
  path (CHANGELOG calls out what changed + how to absorb).
- `domain-code` and `domain-legal` both align with the v1.0 locked
  shape — proof the unification is real, not aspirational.
