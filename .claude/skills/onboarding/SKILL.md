---
name: onboarding
description: >-
  Personalize and build out a freshly-copied product-workspace template so Claude knows the user's
  product, team, strategy, tools, and codebase as well as they do. Works existing-sources-first:
  pulls from documents, links, connected tools, and (optionally) the codebase before asking anything;
  structures the workspace into navigable folders; connects the user's MCP tools and code; scaffolds
  their current initiatives; indexes the key docs in CLAUDE.md — and only then asks targeted
  questions to fill what's missing. RE-RUNNABLE: tracks what's done and resumes where it left off.
  Use whenever the user has just copied the template, says "onboard me", "set up / personalize my
  workspace", "make this workspace mine", "fill in my context", "deepen my workspace", "what should I
  add next", "help me connect my tools / Jira / Confluence / Amplitude / my codebase", or when
  CLAUDE.md still has {{PLACEHOLDER}}s or context/ and initiatives/ are empty. Trigger even if they
  just say "onboarding" or describe their product and ask you to remember it.
---

# Onboarding — Bring This Workspace to Life

Take a freshly-copied (or half-built) template and bring it to life: a workspace whose CLAUDE.md
knows this person's product, team, and tools, organized so things are findable, with their tools
connected and their current work scaffolded. This is usually someone's first taste of the
workspace — make it feel fast, concrete, and a little magical.

## What this skill is — and isn't

**It does:** **fill** the template with the user's context, **structure** it into navigable folders,
**connect** their tools and codebase, **scaffold** their current initiatives, and **index** the key
docs in CLAUDE.md — then ask targeted questions to fill the gaps. End state: a workspace that knows
their world and works well immediately.

**It does NOT** (that's the rest of the workshop, later): migrate everything out of their existing
tools, build other skills/automations, or wire up deep tool-execution references for skills that
write back to Jira. Keep that line crisp — a skill that tries to do everything does nothing well.

## Principles (non-negotiable)

1. **Existing sources first, questions last.** Always start with what already exists — *"What do you
   have? Paste it, drop a link, point me at a doc or your codebase."* — then pull from connected
   tools. Open-ended interviewing can run forever, so it's the **final, bounded** step, not something
   you sprinkle throughout. Ask only the few questions needed to make sense of what you're given,
   until the end.
2. **Discoverability, not minimalism.** This is Claude *Code*, not Claude *Projects*: it retrieves
   files *selectively*, so gather **richly** and make it **findable** — disciplined frontmatter +
   a current index in CLAUDE.md + a navigable folder structure. "Keep it indexed," not "keep it small."
3. **Coach; don't transcribe.** Use productive friction, challenge fuzzy answers, explain *why* you
   ask. The workspace's value is that Claude answers like a teammate.
4. **Seed structure, not judgment.** You can create files, valid frontmatter, folders, and
   forcing-function empty sections. You cannot invent the user's insights or metrics — mark unknowns
   and say how to find them. Never fabricate.
5. **Honest about the auth boundary.** You can do all declarative wiring; the human does OAuth and
   clones their own repos. Say so plainly, and never claim a connector is live until you've verified it.
6. **Verify what's actually there.** Don't assume scaffolding exists. Before each step, check what's
   present and degrade gracefully when it isn't.

---

## Phase 0 — Orient (every run starts here)

Re-runnability is the point. Before doing anything:

1. **Scan the workspace** — are the **identity tokens** in `CLAUDE.md` still unfilled (`{{ROLE}}`,
   `{{COMPANY}}`, `{{PRODUCT_NAME}}`, `{{TEAM}}`, `{{YOUR_SCOPE}}` …)? *Check those specific tokens,
   not just any `{{...}}`* — the template's own header comment contains the literal word
   `{{PLACEHOLDERS}}`, which is not a field. Is `context/` empty or seeded? Any initiative folders?
   Is there a `Codebase/`? Which connectors respond (`claude mcp list`)?
2. **Read the state file** `.claude/data/onboarding/state.json` if it exists.
3. **The workspace scan is ground truth; the state file is a hint.** When they disagree, trust what
   you observe on disk. The presence of a state file (or already-filled identity tokens) means this
   is *not* a first run — reconstruct state from the scan rather than starting over.
4. **Tell the user where they are and what this run will do** in a line or two, then proceed to the
   first unfinished step (or one they ask for).

**Expect zero connectors on a first run** — don't re-probe for them at every step; go straight to
what the user can paste, and bring in connector-pull from run 2 onward.

---

## The flow

Work the steps in order; each builds on the last. Read the linked reference when you start that step.

### Step 1 — Gather from what exists, and fill the workspace

This is the bulk of a first run. Lead with ingestion, not questions.

**a) Ask what exists.** *"Before I ask you anything: what do you already have? A deck, a one-pager,
a strategy doc, your landing page, a notes export, your LinkedIn — paste it, link it, or upload it.
The more you give me, the less I have to ask."* Also ask: *"Any templates — a PRD or user-story
format you like? I'll drop them in `templates/`."*

**b) Process what you get.** For anything substantial, hand it to a **subagent** to de-noise into
faithful markdown (preserving exact quotes) — but first **write the pasted text to a temp file**
(e.g. `.claude/data/onboarding/ingest-<n>.md`) and give the subagent the *path*; never re-summarize
the source yourself to fit a prompt. Short pastes: just structure them inline. See
`references/ingestion.md`.

**c) Fill three things from what you extracted:**
- **CLAUDE.md** — keep the coaching *engine* verbatim; fill the identity (role, company, product,
  who it's for, scope, metrics, funnel, strategy, team, terminology, tools, tech stack). Ask only the few
  short questions needed if a document didn't cover them — batch them. **Update** the skills and
  templates indexes to match what actually exists (don't preserve pointers to things that aren't
  there). Add a short elicited **goal + "how hard should I push you?"** note — those two calibration
  questions are fair to ask now; everything deeper waits for Step 4.
- **The folder structure** — create navigable `context/` subfolders as content lands, so things are
  findable and easy to extend: `General company context/`, `Strategy/`, `Bets/`, `General product
  context/`, `User Flows/`, `Team context/`, and a private `Yourself/`. Route each piece to the
  right folder. See `references/routing-rules.md`.
- **The resource index** — the bullet list under CLAUDE.md's `## Key Resources` table (right after
  the `<!-- As you create… -->` comment). Add a one-line pointer there *the moment* you create a
  file — an un-indexed file is invisible.

**Writing mode:** this is a true first run **only if** there's no `state.json` *and* the identity
tokens (`{{ROLE}}`, `{{PRODUCT_NAME}}` …) are still unfilled — then **auto-write** (don't slow the
magic) and end Step 1 with "here's everything I changed — tweak anything." Otherwise (a state file
exists, or those tokens are already filled) switch to **propose-a-diff-and-confirm** — never clobber
the user's edits. Mark any gap you intentionally leave with `{{TODO: …}}` (never a bare `{{ALLCAPS}}`
token) so it's distinguishable from an unfilled field. **Once CLAUDE.md is filled, delete the
template-note comment block at the very top** — it has done its job, and leaving it keeps a stale
`{{PLACEHOLDERS}}` reference that confuses future scans.

**If they have nothing to paste / answer tersely:** that's fine, never block. Use the
"dinner-party explanation" move (*"explain your product like you would to someone at a dinner
party — just talk, I'll shape it"*) and the batched short questions; if they stay terse after one
nudge, write a valid starter with `{{TODO: …}}` markers and move on. The gaps get filled in Step 4.

**If they're light on input but have a codebase:** offer to read it as a context source — but
**translate to product terms**: what the product does, its main features and user-facing flows, the
data model in plain language. Keep it high-level, mark it *derived from code — verify*, and don't
fill `context/` with technical minutiae (unless the user's role is technical — then their architecture
*is* their context). You don't need to read the whole codebase; sampling entry points, routes, and
models is usually enough. See `references/codebase-setup.md`.

→ `references/gathering-context.md` (what to gather, the document asks, the question bank).

### Step 2 — Connect the tools (and the codebase)

Get the user's tools — and their code — reachable. This matters early: Steps 3–4 are richer once
tools are live, and direct code access unlocks far better technical context.

**Tools.** Map the tools they named to marketplace plugins, then for each: install with
`/plugin install <name>@<marketplace>` (most live in `claude-plugins-official`, which is
auto-available; confirm the exact name in the `/plugin` Discover list), have the user **authenticate**
(`/mcp` → Authenticate → browser login — you can't do OAuth for them), and **`/reload-plugins`** to
activate (no full restart needed). Offer to add the `permissions.allow` entries to
`.claude/settings.local.json` (merge, never overwrite) — but say so plainly: this *widens*
permissions, so Claude Code will ask them to approve the write. Frame it as *"I'll add these so you
stop getting prompted — approve when asked,"* not a silent action. Then **verify** with
`claude mcp list` — only call a connector live when it shows connected.

**Codebase.** If the user has a codebase, offer to wire it in — it lets you and future skills read
real components, routes, and schemas without leaving the workspace. You can't clone their private
repos (that needs their git auth), so guide them: clone *or* symlink the repo(s) into a `Codebase/`
folder, strip `.git`/`node_modules`/`vendor`, and add `Codebase/` to `.gitignore` so it stays local.
See `references/codebase-setup.md`.

→ `references/mcp-setup.md` (per-tool map + auth boundary); `references/codebase-setup.md` (code wiring).

### Step 3 — Scaffold their current initiatives

Capture the work they're doing *right now* — lightly, not a full migration. From what they've shared
(and a connected tracker, if live), identify their active initiatives. For each, create
`initiatives/[slug]/README.md` (stable "what it IS") and `status.md` (live "where it IS now"),
pre-filled from what you have. **Confirm before saving.** Use the `examples/` initiative files as the
pattern, and add each initiative to the resource index.

### Step 4 — Fill the gaps (the bounded interview, last)

Now that the workspace is assembled, look at it and ask: *what's missing for Claude to do good
general product work?* Surface the **top few** gaps (most valuable first), explain why each matters,
and ask — batch the short ones, go one-at-a-time for anything needing a paragraph. Capture answers
in the user's **own words**, route them, index them. Then **stop and offer to continue later** — this
is bounded on purpose, and re-runs resume here. The sensitive "Yourself" bucket (manager feedback,
reviews) is offered privately, never pushed.

→ `references/gathering-context.md` for the question bank and cadence.

---

## Every run ends with three things

1. **A proof moment** — *"Ask me something only this workspace would know now — a key metric or open
   question, who owns X, what we decided on initiative Y — and watch me answer from what we just
   built."* Then answer from the new files.
2. **A recap** of what changed (files created/edited).
3. **One obvious next step**, and an invitation to re-run anytime.

Then **update `.claude/data/onboarding/state.json`**: which steps are done, the context-coverage map,
connector statuses, the run date. Update the relevant entry as each step completes, not only at the
end — so a session that stops mid-flow leaves accurate state for the next run.

## Writing rules

- **Frontmatter on every file** in `context/` and `initiatives/` — at least `title`, `type`,
  `last_updated`; add `initiative`, `goals`, `bet`, etc. when they apply.
- **The resource index = the bullet list under `## Key Resources` in CLAUDE.md.** Add a pointer
  there the moment you create a file. Treat "wrote a file" and "indexed it" as one action.
- **Capture the user's words** when writing back answers — prefer their phrasing for the salient things.

## Quality checks (before saying a step is done)

- No unfilled identity token left in CLAUDE.md and the template-note comment removed (intentional
  `{{TODO: …}}` holes are fine); the coaching engine is untouched; the skills/templates indexes
  reflect what actually exists.
- Every new file has valid frontmatter, sits in the right folder, and is in the resource index.
- You never claimed a connector was live without `claude mcp list` confirming it.
- You didn't fabricate a metric, ID, or fact — unknowns are marked with how to find them.
- The state file reflects reality after the run.

## Guardrails

- **Existing sources before questions.** If you're about to ask something a doc could answer, ask
  for the doc instead. Save the real interview for Step 4.
- **Never clobber edits.** Auto-write only on a true first run (no state file + identity tokens
  unfilled); otherwise propose a diff.
- **Use subagents for the heavy reads only** (ingesting documents, crawling connected tools, reading
  a codebase), and hand them a file path — never re-summarize the user's source to fit a prompt.
  Keep the conversation and the writing in the main thread.
- **Codebase → product language.** When you mine code for context, translate it to product terms and
  keep it high-level; don't drown `context/` in technical detail (unless the user's role is technical).
  Read the repo's own README/docs/specs first (richest product source), and surface any doc-vs-code
  contradiction for the user to resolve rather than silently picking one.
- **Never touch or echo secrets.** Don't open `.env` or anything holding keys/tokens; before saving
  code-derived context, scan your own output for stray tokens, keys, IPs, or local paths and strip them.
- **Don't pressure for numbers.** If a metric is unknown, write a clear `{{TODO: …}}` or omit it —
  never push the user to estimate, never guess, and never leave a half-empty section implying data
  that isn't there.
- **The "Yourself" bucket is private.** Offer it; explain the payoff; never push it in a shared room.
- **Don't over-fill.** Starter files are seeds with forcing-function sections, not encyclopedias.
