# `skills/` — skill conventions

A **skill** is a named, invokable capability a Claude session runs on
request (e.g. `/onboard`, `/sync`). Each skill is one folder holding a
`SKILL.md`; scripts and templates live alongside it when the skill has
deterministic plumbing.

```
skills/
└── <name>/
    ├── SKILL.md        ← required: frontmatter + behavior contract
    ├── <name>.sh|.py   ← optional: the deterministic mechanics
    └── templates/      ← optional: files the skill emits
```

## `SKILL.md` shape

Every `SKILL.md` opens with YAML frontmatter, then a body:

```markdown
---
name: <kebab-case, single word preferred>
description: <what it does, when it triggers, and 3–5 concrete
  trigger phrases the user might say — the router reads this to
  decide when to invoke the skill>
---

# /<name> — <one-line title>

<One paragraph: what this skill produces and the judgment it applies.>

## Behavior contract
- <Hard rules: what it always/never does. Consent model. What it
  refuses. Whether it writes files, edits code, or only reports.>

## Process
1. <Numbered steps.>

## What NOT to do
- <Common wrong turns.>

## Done when
- <The observable definition of finished.>
```

## Authoring principles

- **The description is the trigger surface.** The router matches on it.
  Front-load concrete phrases the user would actually say. Vague
  descriptions misfire.
- **Separate plumbing from judgment.** If part of the skill is
  deterministic (which files to touch, how to stamp a lockfile), put
  that in a script and let `SKILL.md` own the judgment. Three tiers:
  **none** (pure AI), **partial** (script plumbs, AI synthesizes),
  **full** (script end-to-end, AI just routes).
- **Domain-agnostic here; subject-aware in forks.** Starter skills in
  `rasa.domain.core` reference only the Element contract. A fork's own
  skills may reference the fork's subject freely.
- **Consent is explicit.** A skill that mutates files says so and
  never auto-commits — edits land in the working tree for the user to
  review.

Use the `new-skill` skill to scaffold a new one that already follows
these conventions.
