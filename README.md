# RasaOS Domain · Core

**Canonical name:** `rasa.domain.core`
**Repo / folder:** `domain-core`
**Kind:** `domain` (canon Spec §6)
**Contract:** Element Contract v1.3.0
**Version:** 0.1.0
**Status:** v0.1.0 stripped-down template. **Phase 2 will lock down a unified shape across `domain-code` + `domain-legal`** and version it here.

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

## File layout (v0.1.0 stripped)

```
domain-core/
├── rasa.json              # Connection Contract — kind=domain, contract_version=1.3.0
├── VERSION                # semver, currently 0.1.0
├── README.md              # this file
├── CLAUDE.md              # per-repo working contract for sessions on this template
├── CHANGELOG.md           # version history
├── .gitignore             # macOS + editor cruft + .claude/rasa.lock.json
├── bin/
│   └── init               # minimal install script: applies rasa.json policies + stamps lockfile
├── content/
│   ├── skills/.gitkeep    # empty — forks add per-domain skill .md files here
│   ├── rules/.gitkeep     # empty — forks add per-domain rule .md files here
│   └── agents/.gitkeep    # empty — forks add per-domain subagent .md files here
└── seed/
    ├── CLAUDE.md.template       # per-project Claude session contract template
    └── rasa.lock.json.template  # canonical lockfile shape (gets stamped with SHA at install)
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
| `content/` | ✓ (skeleton — empty subdirs) |
| `seed/` | ✓ (with 2 canonical templates) |
| `bin/init` | ✓ (minimal — applies policies + stamps lockfile) |

## What's deliberately NOT here (Phase 2 work)

The v0.1.0 stripped template intentionally ships with:

- **No skills** in `content/skills/` (every domain authors its own)
- **No rules** in `content/rules/` (every domain authors its own)
- **No agents** in `content/agents/` (every domain authors its own)
- **No `bin/check-manifest`** (domain-code has one; not yet canonized as required)
- **No `bin/lint`** (domain-code has one; not canonical)
- **No `tasks/` folder** (domain-code has one for its own work-tracking; not part of the domain shape)
- **No `docs/` folder** (domain-legal has one for legal-conduct docs; not part of the shape)
- **No LICENSE** (not in canon required-files list; defer to per-domain decision)

**Phase 2 (next):** review `domain-code` and `domain-legal` side-by-side,
extract the genuinely-shared shape (beyond canon required-files),
lock it down here, version it in `domain-core`. Decisions like
"is `bin/check-manifest` required for every domain?" + "is `tasks/`
part of the domain shape?" + "what skills are universal vs
domain-specific?" land in Phase 2.

## See also

- Canon Spec §6 (Element kinds — `domain` definition)
- Canon ELEMENT_CONTRACT.md (every Element follows this)
- `~/rAI/rasa-os/elements/domain-code/` (engineering domain — most mature implementation)
- `~/rAI/rasa-os/elements/domain-legal/` (legal domain — second implementation)
- `~/rAI/rasa-os/elements/REGISTRY.md` (live snapshot of every Element under workspace management)

## License

Apache 2.0 (planned — to be added before first external fork. Matches
the convention used by `domain-code` and `domain-legal` historical
forks from the claude-kit lineage).
