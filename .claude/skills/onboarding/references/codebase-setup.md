---
title: Codebase Setup — Wiring the Code into the Workspace
type: reference
last_updated: 2026-06-16
---

# Codebase Setup

This is how you give Claude direct, file-level access to the user's codebase from inside the
workspace — so this skill (and any skill they build later: QA, bug repro, ticket-writing, tracking)
can read real components, API routes, and database schemas without switching repos. Code context
living next to product context, in one place, is a big part of what makes the workspace feel like a
teammate.

Two jobs, kept separate:
- **Access** (always worth offering): wire the code in so Claude can read it on demand.
- **Bootstrapping context** (a fallback): when the user is light on other input, *read* the code to
  seed product context — but translated to product language, not a code tour. See the last section.

## The auth boundary

Same shape as connecting a tool: **you can't clone the user's private repos** — that needs their git
credentials (SSH key / token). So you *guide and prepare*; the user runs the clone. Be honest about it.

## Two ways to wire it in

The code lives under a top-level `Codebase/` folder. Pick based on how the user works:

| Approach | Command (example) | Best when |
|---|---|---|
| **Snapshot** (flat copy) | clone into `Codebase/<repo>`, then strip `.git` | They want a stable, self-contained copy; the repo isn't checked out locally |
| **Symlink** (live) | `ln -s ~/path/to/<repo> Codebase/<repo>` | They already have the repo checked out and want it always current |

**Snapshot steps** (run per repo — the clone is the user's to run, the rest you can do):

```bash
# user runs (their git auth):
git clone <repo-url> "Codebase/<repo-name>"
# you can run:
rm -rf "Codebase/<repo-name>/.git"            # flatten — it's a snapshot, not a nested repo
rm -rf "Codebase/<repo-name>/node_modules" "Codebase/<repo-name>/vendor"   # drop heavy deps
```

The snapshot goes stale; refresh it by re-cloning when the code has moved on. (A dedicated
`update-codebase` skill is a great thing to build later with skill-creator — it's out of scope for
onboarding.)

The symlink never goes stale, but it depends on the local checkout staying at that path — tell the
user as much (*"always current, but it breaks if you move the repo"*). One gotcha: don't run `du` on
the symlink to "check nothing was copied" — `du` follows the link and reports the *target's* full
size, which looks like a huge copy. Verify with `ls -l` on the link entry, not the target.

## Keep it local — gitignore it

Add `Codebase/` to the workspace's `.gitignore` (create the file if there isn't one). Two reasons:
the snapshot would bloat the workspace's own git history with huge, noisy diffs, and the code is the
engineering team's source of truth — it shouldn't be re-committed inside the PM workspace. Local-only
means each teammate runs their own clone/symlink, which is cleaner anyway.

## Works for any stack

The mechanic is stack-agnostic — one repo or several, frontend and/or backend, any language. Adapt
the example commands; the pattern (clone-or-symlink into `Codebase/`, flatten, drop deps, gitignore)
is the same. If there are multiple repos, give each its own subfolder under `Codebase/`.

## Using the codebase to bootstrap context (the fallback)

Only when the user is light on other input and `context/` is thin. The trap is filling `context/`
with technical minutiae that reads like an architecture doc instead of product knowledge. So:

- **Translate to product terms.** Read entry points, routes, models, and key components, then write
  *what the product does* — its main features, the user-facing flows, the data model in plain
  language — not a file-by-file tour.
- **Stay high-level and mark the source.** Note "derived from code — verify with the team." Code tells
  you what exists, not why it matters or whether it's still used.
- **Do it in a subagent.** A codebase is heavy; have a subagent read it and return a product-level
  summary, so the main thread stays a conversation (per `references/ingestion.md`).
- **Exception — technical users.** If the user is an engineer (or the workspace is for technical
  work), the architecture/services/runbooks *are* their context — capture them directly, no
  product-translation needed.
- **Read the repo's own prose docs first.** A `README`, a `docs/` folder, or `specs/` is often the
  richest product source in the whole repo — and it's already in the author's words. Ingest those as
  *real context* (light touch, not "derived from code"); reserve product-translation for the code
  itself. Keep the two clearly distinguished in what you write.
- **Surface contradictions; don't bury them.** If the docs say one thing and the code does another
  (a "no contact form" brief but a shipped contact form), flag it and ask the user which is current —
  that seam is exactly where real product clarity hides. Never silently overwrite one with the other.
- **Never touch or echo secrets.** Don't open `.env` or anything holding keys/tokens. Before saving
  any code-derived file, scan your own output for stray tokens, API keys, IPs, or absolute local
  paths and strip them — mined context is the most likely place a secret slips in.

## Common Pitfalls

1. **Don't try to clone for the user.** Private repos need their git auth — hand them the `git clone`
   command and run only the flatten/cleanup steps yourself.
2. **Don't commit the code into the workspace git.** Gitignore `Codebase/` first — otherwise you bloat
   the repo with massive diffs and duplicate the engineering team's source of truth.
3. **Don't leave `.git`/`node_modules`/`vendor` in a snapshot.** They make the snapshot huge and slow
   to scan, and a nested `.git` confuses tooling. Strip them.
4. **Don't dump code-derived technical detail into `context/`.** Translate to product language and
   keep it high-level; raw architecture belongs to an engineer's workspace, not a PM's product context.
5. **Don't treat the snapshot as current.** It's a point-in-time copy — note when it was taken, and
   re-clone before relying on it for anything that may have changed.
6. **Don't read or echo secrets.** Never open `.env` or files holding keys/tokens, and scan any
   code-derived file you write for stray tokens, API keys, IPs, or local paths before saving.
7. **Don't `du` a symlink to prove nothing was copied.** It follows the link and reports the
   target's full size — misleading. Check the link entry with `ls -l` instead.
