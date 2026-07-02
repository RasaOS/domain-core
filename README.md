# RasaOS Domain · Core

**Canonical name:** `rasa.domain.core`
**Repo / folder:** `domain-core`
**Kind:** `domain` (canon Spec §6)
**Contract:** Element Contract v1.3.0
**Version:** 1.1.0 (locked v1.0.0 shape + v1.1.0 starter/enforcement layer)
**Status:** **v1.1.0 — stable.** The shape locked at v1.0.0 (unified from side-by-side analysis of `rasa.domain.code`, toolkit pattern, and `rasa.domain.legal`, structural pattern). v1.1.0 adds the opt-in universal starter layer + template enforcement — additive; forks may pin to v1.0.0 and adopt minors opportunistically.

## What this is

The stripped-down canonical template every `domain` Element follows.
Fork this when authoring a new domain (`rasa.domain.legal`,
`rasa.domain.code`, `rasa.domain.health`, etc.) and populate
`content/skills/`, `content/rules/`, `content/agents/` + `seed/` with
the domain-specific material.

This Element holds **zero domain knowledge** — it's the SHAPE every
domain shares.

## What this isn't

- **Not `rasa.core`** (kind: `core`) — that's the L1 shared bones every
  Element depends on (vocabulary, output styles, stamp definitions).
  Different concerns; both named "core" for canonical positioning.
- **Not `element-template`** (workspace consumer-template) — that's the
  any-kind fork-and-go scaffold pre-canon-restructure. `domain-core` is
  specifically for the `domain` kind, post-canon-v1.3, canon-shaped.

## How to fork this as a new domain

```sh
# 1. Clone the template
git clone https://github.com/RasaOS/domain-core.git domain-<yourname>
cd domain-<yourname>

# 2. Re-point git remote
git remote set-url origin git@github.com:<your-org>/domain-<yourname>.git

# 3. Edit rasa.json
#    - name: rasa.domain.<yourname>
#    - version: 0.1.0  (reset)
#    - description: <your domain description>
#    - capabilities[]: your domain's capability names
#    - aliases[]: your domain's alternate names

# 4. Populate content/skills/, content/rules/, content/agents/
#    with your domain-specific markdown + scripts

# 5. Add seed/ templates that should land in install targets

# 6. Author seed/CLAUDE.md.template — the per-project CLAUDE.md
#    that ships into each install target

# 7. Bump version, commit, tag v0.1.0, push
```

## File layout (v1.0.0 locked shape, v1.1.0 content)

```
domain-core/
├── rasa.json              # Connection Contract — kind=domain, contract_version=1.3.0
├── VERSION                # semver, currently 1.1.0
├── README.md              # this file
├── CLAUDE.md              # per-repo working contract for sessions on this template
├── CHANGELOG.md           # version history
├── LICENSE                # Apache-2.0 (matches claude-kit + domain-legal lineage)
├── .gitignore             # macOS + editor cruft + .claude/rasa.lock.json
├── bin/
│   ├── init               # canonical installer: applies rasa.json policies + stamps lockfile
│   ├── check-manifest     # the INVENTORY gate: rasa.json is a complete inventory of content/+seed/ (pure-python)
│   └── check-shape        # the STRUCTURE gate: content/ conforms to the templated shape (pure-python)
├── content/              # universal opt-in starter layer (v1.1.0) — fork keeps/deletes
│   ├── README.md          # guide to the starter layer; fork-time (deleted on fork)
│   ├── SHAPE.md           # explains the shape patterns (toolkit/structural/hybrid); fork-time
│   ├── skills/            # domain-agnostic starter skills + authoring conventions
│   │   ├── README.md
│   │   ├── new-skill/     # meta: scaffold a new skill
│   │   ├── codify/        # meta: promote a session rule into durable storage
│   │   ├── sync/          # meta: reconcile installed content against upstream
│   │   ├── onboard/       # capability: orient a session to the installed domain
│   │   ├── handoff/       # capability: durable session checkpoint (+ .claude/HANDOFF.md pointer)
│   │   ├── resume/        # capability: read-side of handoff — pick up in-flight work
│   │   └── update-docs/   # capability: reconcile docs against reality
│   ├── rules/             # universal rules + authoring conventions
│   │   ├── README.md
│   │   ├── contract-rules.md   # Element <-> consumer discipline (lockfile is truth)
│   │   ├── output-rules.md     # house communication style
│   │   └── authoring-rules.md  # the enforced authoring lifecycle
│   ├── templates/         # enforced authoring skeletons (skill / rule / agent)
│   └── agents/            # subagent conventions (no starter agents — subject-specific)
│       └── README.md
└── seed/
    ├── CLAUDE.md.template       # per-project Claude session contract (placeholder-free)
    └── rasa.lock.json.template  # canonical lockfile shape (substituted at install)
```

## Required files per canon ELEMENT_CONTRACT §4

All present:

| File | Status |
|---|---|
| `rasa.json` | ✓ |
| `VERSION` | ✓ |
| `README.md` | ✓ |
| `CLAUDE.md` | ✓ |
| `CHANGELOG.md` | ✓ |
| `.gitignore` | ✓ |
| `content/` | ✓ (v1.1.0 universal opt-in starter layer) |
| `seed/` | ✓ (with 2 canonical templates) |
| `bin/init` | ✓ (minimal — applies policies + stamps lockfile) |

## v1.0.0 Phase-2 decisions

Phase 2 reviewed `rasa.domain.code` (toolkit pattern) + `rasa.domain.legal`
(structural pattern) side-by-side. The unified template ships **only
what's truly universal** across both shapes. The 8 open questions:

| # | Question | Decision | Why |
|---|---|---|---|
| 1 | `bin/check-manifest` required? | **YES** — included as canonical bin tool (pure-python, cross-platform — adapted from domain-legal) | Both implementations have it; validates manifest-driven install discipline |
| 2 | `bin/lint` part of shape? | **NO** — domain-specific | Only domain-code uses; for its skill markdown linting |
| 3 | `tasks/` part of shape? | **NO** — domain-specific | Domain-code uses for its own work-tracking; not part of Element contract |
| 4 | `docs/` part of shape? | **NO** — domain-specific | Domain-legal ships legal-conduct docs; domains decide |
| 5 | Universal seed templates beyond CLAUDE.md + rasa.lock.json? | **NO** — keep just 2 | Domain-code's 28 are engineering-toolkit-specific |
| 6 | Universal `content/<subdirs>/`? | **None enforced**, ship toolkit scaffold (`skills/`, `rules/`, `agents/`) as opt-in; document both patterns in `content/SHAPE.md` | Forks adapt or delete |
| 7 | LICENSE? | **YES — Apache-2.0** | Matches claude-kit + domain-legal lineage |
| 8 | Stripped seed example? | **NO** | Two templates are enough; forks fork |

## What domain-core v1.0.0 ships

**Universal (every domain Element):**
- Canon-required files (§4): `rasa.json`, `VERSION`, `README.md`,
  `CLAUDE.md`, `CHANGELOG.md`, `.gitignore`
- `LICENSE` (Apache-2.0)
- `content/` directory (required by §4 for `domain` kind)
- `seed/` with 2 canonical templates: `CLAUDE.md.template` + `rasa.lock.json.template`
- `bin/init` — canonical installer reading rasa.json policies
- `bin/check-manifest` — cross-platform manifest validator
- `content/SHAPE.md` — fork-time guide explaining the toolkit-vs-structural choice

## What v1.1.0 adds — the universal opt-in starter layer

All **opt-in** (`policy: opt-in`): registered for check-manifest
coverage, but **not** installed into a consumer project until a fork
flips the policy. Every file is domain-agnostic — it references only
the rasa Element contract (lockfile, `pinned_sha`, `overrides[]`,
install policies), never a subject. A fork keeps what fits its shape,
deletes the rest, adds its own.

- **Meta-skills** — `content/skills/new-skill/`, `.../codify/`, `.../sync/`
- **Capability skills** — `content/skills/onboard/`, `.../handoff/`, `.../resume/`, `.../update-docs/`
- **Universal rules** — `content/rules/contract-rules.md`, `.../output-rules.md`, `.../authoring-rules.md`
- **Enforced authoring templates** — `content/templates/` (skill / rule /
  agent skeletons); the shape is templated first, then followed —
  `bin/check-shape` is the enforcement and a release gate
- **Baseline conventions** — `content/README.md`, and `README.md` in
  `skills/`, `rules/`, `agents/` documenting how each is authored
- `content/agents/` ships conventions but **no** starter agents —
  subagents are inherently subject-specific

> The v1.0.0 empty `.gitkeep` scaffold was superseded by this layer.

## What domain-core deliberately does NOT ship

Per the Phase-2 decisions above, none of these are in the unified template:

- **Subject-specific** skills, rules, or agents (every domain authors
  its own — the v1.1.0 starter layer is *domain-agnostic* only, and all
  opt-in)
- `bin/lint` (domain-code-specific skill linting)
- `bin/sync-report`, `bin/register-user` (domain-legal-specific operational tools)
- `.cmd` Windows variants (domain-legal-specific market — but `bin/check-manifest` IS pure-python for cross-platform)
- `modes/`, `dashboard/`, `build/`, `tests/` content subdirs (domain-code-specific)
- `content/firm/`, `content/drives/`, `content/members/` (domain-legal-specific structural)
- `tasks/` folder (domain-code-specific work-tracking)
- `docs/` folder (domain-legal-specific conduct docs)
- ~~Skill/rule/agent templates~~ — *reversed in v1.1.0*: domain-agnostic
  authoring skeletons now ship at `content/templates/`, enforced by
  `bin/check-shape` (what stays out is *subject-specific* template
  content)
- More than 2 seed templates (forks add their own)

## Versioning intent (post-v1.0)

- **Patch (1.0.x)** — typo fix, README clarification, bin/init or check-manifest bug fix. No structural change.
- **Minor (1.0.x → 1.1.0)** — additive: a new canonical bin tool, a new universal seed template, a new optional `content/<subdir>/` scaffold. Forks may adopt opportunistically.
- **Major (1.x.x → 2.0.0)** — breaking shape change. Forks REQUIRED to migrate. Avoid without explicit user direction.

## Surfaced drift (not fixed here per role-split)

Phase-2 analysis caught: **`rasa.domain.code` is missing `CLAUDE.md` at root** (canon ELEMENT_CONTRACT §4 violation). Domain-legal has it; domain-code doesn't. Flagged in canon AUDIT; for the domain-code team to fix next time they touch the repo.

## See also

- Canon Spec §6 (Element kinds — `domain` definition)
- Canon ELEMENT_CONTRACT.md (every Element follows this)
- The `rasa.domain.code` Element / `RasaOS/domain-code` (engineering domain — most mature toolkit implementation)
- The `rasa.domain.legal` Element / `RasaOS/domain-legal` (legal domain — structural implementation)
- The workspace elements registry (`REGISTRY.md` — live snapshot of every Element under workspace management)

## License

Apache 2.0 — see `LICENSE` (shipped since v1.0.0, Phase-2 decision #7).
Matches the convention used by `domain-code` and `domain-legal` from
the claude-kit lineage.
