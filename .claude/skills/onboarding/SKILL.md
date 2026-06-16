---
name: onboarding
description: >-
  Personalize and build out a freshly-copied product-workspace template so Claude knows the user's
  product, team, strategy, tools, and codebase as well as they do. Connects the user's tools FIRST
  (and collects every access pointer — which Figma files, which Confluence spaces, which repos), then
  explores all sources in parallel, structures the workspace into navigable folders, scaffolds the
  user's current initiatives, indexes the key docs in CLAUDE.md — and only then asks targeted
  questions to fill the gaps. It BUILDS the workspace; it does not do product work. RE-RUNNABLE:
  tracks what's done and resumes where it left off. Use whenever the user has just copied the
  template, says "onboard me", "set up / personalize my workspace", "make this workspace mine",
  "fill in my context", "deepen my workspace", "what should I add next", "help me connect my tools /
  Jira / Confluence / Amplitude / my codebase", or when CLAUDE.md still has {{PLACEHOLDER}}s or
  context/ and initiatives/ are empty. Trigger even if they just say "onboarding" or describe their
  product and ask you to remember it.
---

# Onboarding — Bring This Workspace to Life

Take a freshly-copied (or half-built) template and bring it to life: a workspace whose CLAUDE.md
knows this person's product, team, and tools, organized so things are findable, with their tools
connected and their current work scaffolded. This is usually someone's first taste of the
workspace — make it feel fast, concrete, and a little magical.

## What this skill is — and isn't

**It does:** **connect** the user's tools, **gather** from everything they have, **structure** it into
navigable folders, **scaffold** their current initiatives, and **index** the key docs in CLAUDE.md —
then ask targeted questions to fill the gaps. End state: a workspace that knows their world.

**It does NOT do the work.** It reads a tool only far enough to capture what exists *into the
workspace*, then stops. It does **not** analyze the data, run queries, propose experiments or
investigations, recommend closing/triaging tickets, or do any product work — that's other skills and
the rest of the workshop. It also doesn't migrate everything out of their tools or build other
skills. A skill that tries to do everything does nothing well — stay in this lane.

## Principles (non-negotiable)

1. **Build the workspace; don't do the work.** Your job is to *capture and organize* — connect tools,
   pull context in, structure it. The moment you find yourself analyzing a funnel, suggesting an
   experiment, or recommending the user act on a ticket, you've drifted. Read what exists, file it,
   move on. Every next step you propose is a *workspace-building* move, never a product task.
2. **Connect first, then ingest, then interview.** Get access sorted *before* exploring (you can't
   gather from a tool you can't reach). Then ingest from sources. Open-ended interviewing can run
   forever, so it's the **final, bounded** step — ask only logistics until then.
3. **Discoverability, not minimalism.** This is Claude *Code*, not Claude *Projects*: it retrieves
   files *selectively*, so gather **richly** and make it **findable** — disciplined frontmatter +
   a current index in CLAUDE.md + a navigable folder structure. "Keep it indexed," not "keep it small."
4. **Coach; don't transcribe.** Use productive friction, challenge fuzzy answers, explain *why* you ask.
5. **Seed structure, not judgment — and stamp recency.** Create files, frontmatter, folders, and
   forcing-function empty sections; never fabricate. Mark every ingested fact with its date/"as of"
   period, and never let stale data read as current.
6. **Honest about the auth boundary.** You do the declarative wiring; the human does OAuth and clones
   their own repos. Never claim a connector is live until you've verified it.

---

## Phase 0 — Orient (every run starts here)

1. **Scan the workspace** — are the **identity tokens** in `CLAUDE.md` still unfilled (`{{ROLE}}`,
   `{{COMPANY}}`, `{{PRODUCT_NAME}}`, `{{TEAM}}` …)? *Check those specific tokens, not just any
   `{{...}}`* — the template's header comment contains the literal word `{{PLACEHOLDERS}}`, which is
   not a field. Is `context/` seeded? Any initiative folders? A `Codebase/`? What does `claude mcp list` show?
2. **Read** `.claude/data/onboarding/state.json` if it exists. **The scan is ground truth; state is a
   hint.** A state file (or filled identity) means this isn't a first run — resume, don't restart.
3. **On a first run, give a short, plain-language intro** so a first-timer knows what they're in for:
   > "I'll turn this empty workspace into one that knows your product like a teammate — so every
   > future chat starts informed instead of from scratch. Four stages: **(1)** connect your tools so I
   > can reach your real work, **(2)** pull in what already exists, **(3)** set up your current
   > initiatives, **(4)** ask you a few things to fill the gaps. The first pass takes a little while;
   > we can always deepen on a re-run. Ready?"
4. **On a first run, make it theirs — detach the template remote.** If `git remote -v` shows `origin`
   pointing at the template repo (e.g. `product-workspace-template`), this is a *clone* of the public
   template, not their own repo. Before writing any real context, tell them and offer to
   `git remote remove origin` (or repoint it to their own private repo) — otherwise their company data
   could get pushed back to the public template. ("Use this template" on GitHub avoids this entirely.)
5. Then proceed to the first unfinished step (or one they ask for).

---

## The flow

### Step 1 — Connect & point (get ALL access sorted before exploring)

The lesson that makes onboarding fast: wire up *everything* in one upfront pass, so exploration then
happens all at once — not connect-one → explore → connect-next → explore.

**a) Inventory the sources — by *function*, not a fixed list.** Don't recite tool names ("Jira or
Linear? Figma?"). Ask what they use for each *job*, so you catch whatever they actually have: *"Where
do you keep docs? What do you use for tickets/roadmap? For analytics? For design? For meeting notes?
Where's your code?"* Map each tool they name to its connector, and record it in CLAUDE.md's Tools line.
Then walk the tiers so they know what's worth pointing at (detail in `references/gathering-context.md`):
- **Big containers** — a whole Confluence/Notion space, the codebase/repos, an analytics workspace.
- **Company/strategy level** — strategy deck, KPI dashboard, company overview.
- **Initiative level** — PRDs, specs, tracking plans for what they're building now.

**b) Connect the tools + collect their pointers.** For each tool: guide the user to install the plugin
**through the `/plugin` manager** (run `/plugin` → **Discover** tab → search → install — the
`/plugin install <name>@claude-plugins-official` one-liner is a CLI shortcut that may not work in the
VS Code extension), have them authenticate, `/reload-plugins`, and **verify it actually works**
(`claude mcp list`) before relying on it. Crucially, **collect the pointers each tool needs to be useful in the same pass** — connecting
Figma does nothing without the specific *file URLs*; Confluence needs the *space(s)*; the tracker
needs the *project key*; the codebase needs the *repo(s)* (see `references/codebase-setup.md`). Gather
all of these now so Step 2 can run against everything at once. See `references/mcp-setup.md`.

**c) If they have no tools / nothing to connect:** fine — skip to Step 2 and work from what they can
paste or talk through. Never block.

**Done when:** every tool they named is verified working (or explicitly deferred), and you hold the
pointers (files/spaces/projects/repos) needed to explore each.

### Step 2 — Gather & fill (explore everything in parallel)

Now pull it all in — **in parallel**, because this is the slow part.

- **Fan out subagents — one per source/space** (Confluence space A, space B, the codebase, the
  analytics funnel) — each de-noising its source into faithful markdown, **stamping recency** (the
  source's date / "as of" period), and returning a compact product-level summary. Parallel means
  wall-clock is the slowest single source, not the sum. Hand each subagent a file path or pointer;
  never re-summarize a source yourself to fit a prompt. See `references/ingestion.md`.
- **Bound each crawl.** Prioritize *recent* and key/linked docs; don't read an entire massive space
  exhaustively, and don't go deeper than "what's needed for context." If a space is huge, ask which
  section to focus on. (You're capturing what exists — not analyzing it.)
- **Keep canonical docs whole.** A foundational doc — the current strategy, a core PRD — gets stored
  **1:1** (full text, with a short summary on top), not compressed away. De-noising is for noisy/
  voluminous sources, not your source-of-truth.
- **Fill three things** from what you gathered (+ anything pasted):
  - **CLAUDE.md** — engine verbatim; fill identity (role, company, product, who it's for, scope,
    metrics, funnel, strategy, team, terminology, tools, tech stack). **Update** the skills/templates
    indexes to match what exists. Add a short elicited **goal + "how hard should I push you?"** note.
  - **The folder structure** — create navigable `context/` subfolders as content lands: `General
    company context/`, `Strategy/`, `Bets/`, `General product context/`, `User Flows/`, `Team
    context/`, and a private `Yourself/`. Route per `references/routing-rules.md`.
  - **The resource index** — the bullet list under CLAUDE.md's `## Key Resources` table — add a
    pointer the moment you create a file.

**Writing mode:** true first run **only if** no `state.json` *and* identity tokens unfilled → auto-write,
then "here's everything I changed — tweak anything." Otherwise propose-a-diff — never clobber edits.
Mark intentional gaps `{{TODO: …}}` (never bare `{{ALLCAPS}}`). Once filled, **delete the
template-note comment** at the top of CLAUDE.md. If they have nothing to share, pivot to the
"dinner-party explanation" (just talk through the product) — never block on missing material.

→ `references/gathering-context.md`, `references/ingestion.md`, `references/routing-rules.md`.

### Step 3 — Scaffold their current initiatives

Capture what they're building *now* — lightly, not a migration. For each active initiative, create
`initiatives/[slug]/README.md` (stable "what it IS") and `status.md` (live "where it IS now"). **If a
working doc already exists** — a PRD, spec, or tracking plan, in Confluence/the repo/pasted — **pull
it into the folder as its own file** (`prd.md`, etc.); the initiative folder is where working docs
live. **Confirm before saving.** Add each initiative to the resource index.

### Step 4 — Fill the gaps (the bounded interview, last)

Look at the assembled workspace and ask: *what's missing for good general product work?* Surface the
**top few** gaps, explain why each matters, ask — batch short ones, one-at-a-time for paragraphs.
Capture answers in the user's words, route, index. Then **stop and offer to continue later.** If a
metric is unknown, write a clear `{{TODO: …}}` — never pressure for an estimate. The "Yourself"
bucket is offered privately, never pushed.

→ `references/gathering-context.md`.

---

## Every run ends with three things

1. **A proof moment** — *"Ask me something only this workspace would know now — who owns X, what we
   decided on initiative Y — and watch me answer."* Then answer from the new files. (Demonstrate
   knowledge; don't volunteer to go *do* analysis.)
2. **A recap** of what changed.
3. **One next step — and it is always a workspace-building move**: connect the next tool, seed a thin
   context area, scaffold another initiative, or re-run to deepen. **Never** an analytical or product
   task (don't propose digging into the data, running an analysis, or closing tickets).

Then **update `.claude/data/onboarding/state.json`** as each step completes (not only at the end).

## Writing rules

- **Frontmatter on every file** (`title`, `type`, `last_updated`; plus `initiative`, `goals`, `bet`
  when they apply) — and a **recency line** (date / "as of" period) on anything with data.
- **The resource index = the bullets under `## Key Resources` in CLAUDE.md.** Index a file the moment
  you create it.
- **Capture the user's words** for the salient things.

## Quality checks (before saying a step is done)

- You verified each connector with `claude mcp list` before relying on it; you hold the pointers each tool needs.
- No unfilled identity token in CLAUDE.md; template-note comment removed; engine untouched; skills/templates indexes match reality.
- Every file has valid frontmatter, sits in the right folder, is indexed, and (if it has data) states its timeframe.
- Canonical docs (strategy, core PRDs) were kept whole; existing initiative working docs were pulled into their folders.
- You didn't fabricate, didn't present stale data as current, and didn't drift into analyzing/acting on what you read.
- The next step you proposed is a workspace-building move, not product work.

## Guardrails

- **Build the workspace; don't do the work.** Read tools to fill the workspace, then stop — no
  analysis, no queries, no proposed experiments, no "close/triage these tickets." That's other skills.
- **Connect and verify before you explore**, and gather all access pointers (Figma files, Confluence
  spaces, project keys, repos) up front so exploration runs in one parallel pass.
- **Never clobber edits.** Auto-write only on a true first run; otherwise propose a diff.
- **Use subagents for the heavy reads — in parallel, one per source** — and hand them a path/pointer;
  never re-summarize a source to fit a prompt. Bound each crawl; prefer recent docs.
- **Recency, not staleness.** Stamp every ingested fact with its date; flag anything that looks old
  rather than presenting it as current.
- **Keep canonical docs whole; codebase → product language; never touch or echo secrets** (don't open
  `.env`; scan code-derived output for stray tokens/keys/IPs/paths before saving).
- **Don't pressure for numbers.** Unknown metric → a clear `{{TODO: …}}` or leave it out; never a guess.
- **The "Yourself" bucket is private.** Offer it; never push it in a shared room.
- **Don't over-fill.** Starter files are seeds with forcing-function sections, not encyclopedias.
