---
name: sync
description: Reconcile installed Element content against its upstream source. Reads .claude/rasa.lock.json for the pinned SHA, fetches the Element repo at the tracked branch HEAD, diffs every Element-managed file, classifies drift (upstream-only change / local override / both changed / new file / removed file), and proposes per-file pulls. Never auto-commits, never overwrites a recorded override without approval, never touches project-owned files like CLAUDE.md. On approval, advances pinned_sha. Triggered when the user wants to pull Element updates — e.g. "/sync", "pull latest from the domain", "am I up to date with the Element", "sync my .claude".
---

# /sync — Reconcile installed content against upstream

Bring a consumer project's installed Element content up to date with
its source, and — for a fork — absorb upstream `rasa.domain.core`
changes. One direction only: **upstream → here**. Pushing a change back
is a deliberate manual contribution, reviewed before it propagates.

Honest about conflicts: surface drift, don't silently merge.

## Behavior contract

- **Read `.claude/rasa.lock.json` first.** It names the Element repo,
  the tracked branch, the current `pinned_sha`, and the `overrides[]`.
  If it's missing, this project wasn't installed from a rasa Element —
  stop and explain rather than guessing.
- **Fetch fresh.** Clone/shallow-clone the Element repo at the tracked
  branch HEAD into a temp dir. Never trust a cached copy to be current.
- **Diff every Element-managed file** (those declared in the Element's
  `rasa.json` `element.files[]`, resolved through their policies).
  Classify each:
  - **Upstream changed, local unchanged** → safe pull; propose.
  - **Upstream unchanged, local changed** → local edit. If it's in
    `overrides[]`, respect it silently-but-surfaced. If it's *not*
    recorded, flag it loudly — it's unmanaged drift (see
    `contract-rules.md`).
  - **Both changed** → conflict; show both diffs, let the user choose.
  - **New upstream file** → propose adding.
  - **File removed upstream** → propose removing; deletes are
    destructive, confirm explicitly.
- **Project-owned files are off-limits.** `CLAUDE.md` and the project's
  own history/docs were seeded once and are the project's forever. Sync
  never touches them, regardless of drift.
- **Respect recorded overrides.** A path in `overrides[]` is
  intentionally diverged. Never overwrite it without explicit approval,
  even on an upstream change.
- **Never auto-commit.** Apply approved pulls to the working tree; the
  user reviews with `git diff`.
- **Advance the pin only after applying.** Once the user accepts the
  pulls, update `pinned_sha` (and `installed_at`) to the fetched HEAD.
  A pin that moves without the diff being applied is a lie.

## Process

1. Read the lockfile. Bail clearly if absent.
2. Fetch the Element at the tracked branch HEAD.
3. Build the file list from the Element's `rasa.json`; diff and
   classify each against the local copy.
4. Present a per-file plan grouped by classification. Conflicts and
   deletes require explicit choices.
5. Apply approved changes to the working tree.
6. Update `pinned_sha` + `installed_at`. Leave everything uncommitted.
7. Summarize: pulled, skipped, conflicted, overridden.

## What NOT to do

- Don't merge silently or "resolve" a conflict without the user.
- Don't overwrite a recorded override.
- Don't touch `CLAUDE.md` or project history.
- Don't advance the pin without applying the diff. Don't auto-commit.

## Done when

The working tree reflects the approved pulls, `pinned_sha` matches what
was actually applied, overrides are intact, and the user has a clear
summary of what changed and what was deliberately left alone.
