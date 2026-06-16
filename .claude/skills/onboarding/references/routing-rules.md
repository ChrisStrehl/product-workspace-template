---
title: Routing Rules — Where Incoming Information Goes
type: reference
last_updated: 2026-06-14
---

# Routing Rules

This is the decision logic for where any piece of information lands, so folders stay coherent and nothing gets lost. Get routing right and the workspace stays a clean, navigable brain. Get it wrong and you end up with strategy buried in an initiative folder, an encyclopedia in CLAUDE.md, and files nobody can find.

Read this when you're about to write something into the workspace — filling CLAUDE.md, seeding `context/`, or scaffolding `initiatives/`.

## The decision frame

Before you write anything, run it through three questions in order. Each one narrows where the content goes.

### (a) Every chat, or on demand?

The first fork: does Claude need this in *every single conversation*, or only when a task touches it? If it's identity or a rule that shapes how Claude behaves regardless of topic, it belongs in **CLAUDE.md** — the file that loads into every thread. If it's knowledge a task will *retrieve when relevant*, it belongs in **a file** that sits on disk until something needs it. The default answer is "a file" — very little truly needs to be in every chat. See "The CLAUDE.md test" below.

### (b) Knowledge, a thing being built, or how-we-learn?

The second fork sorts files into the three core areas:

| Goes to | When the content is... | Examples |
|---|---|---|
| `context/` | Cross-cutting knowledge that's true across initiatives and changes slowly | Strategy, metrics definitions, the funnel, competitor landscape, a persona, company background |
| `initiatives/[slug]/` | Scoped to one specific thing you're building | A PRD, that initiative's tickets, its QA results, its impact analysis, its live status |
| `discovery/` | About *how you learn* rather than a conclusion | Open research questions, an interview guide, an opportunity-solution tree, a tech spike |

The test that separates `context/` from `initiatives/`: would this still matter if the initiative were cancelled tomorrow? If yes, it's cross-cutting knowledge → `context/`. If it dies with the initiative, it lives in that initiative's folder.

### (c) Which context/ folder?

If it landed in `context/`, sort it into the folder that matches its subject. The four *gathering buckets* (Company / Product org / Team / Yourself) are a lens for **what to collect** — they map onto the template's `context/` folders like this. Create a folder when its first file appears, and match whatever names the user already has — the buckets are the lens, the folders are where results live.

| Gathering bucket | Lands in `context/` folder | Holds |
|---|---|---|
| Company | `General company context/`, plus `Strategy/` + `Bets/` for direction | What the company does, mission, market, business model, segmentation, competitors, strategy, bets |
| Product org | `General product context/`, plus `User Flows/` | Product vision, principles, top-level metrics, the funnel, where user insight comes from, user flows |
| Team | `Team context/` | Who you work with, each person's strengths, stakeholders, past retros and "scars" |
| Yourself | `Yourself/` *(sensitive — keep private)* | Manager feedback, review themes, PM wisdom you keep forgetting |

### Worked examples

> **"Here's our 2026 strategy deck."** Cross-cutting knowledge, true across every initiative, changes slowly → `context/Strategy/`. Add a one-line pointer to CLAUDE.md's resource index; do **not** paste the strategy *into* CLAUDE.md.

> **"This is the PRD for the new onboarding flow we're building."** Scoped to one thing being built → `initiatives/onboarding-flow/` as the PRD. Its live progress goes in that folder's `status.md`, not the PRD.

> **"I'm not sure why activation dropped in March — want to dig in."** That's an open question about how you'll learn, not a conclusion → `discovery/`. Once you reach an answer worth keeping, the *finding* graduates to `context/General product context/`.

## The CLAUDE.md test

CLAUDE.md is a **map, not an encyclopedia.** It holds only what's needed in *every* conversation: the user's identity (role, company, product, scope), the coaching engine, the structural rules (frontmatter, where things live), the templates index, and the resource index that points to everything else. Everything else is a context file.

The test for any candidate line: *"Would Claude need this to behave correctly in a conversation that has nothing to do with the topic?"* The funnel definition fails that test — a chat about hiring doesn't need it, so it's a context file. The user's role passes — it shapes how Claude talks in every chat, so it stays. When in doubt, keep CLAUDE.md lean and push the detail to a file with a pointer.

## Discoverability replaces minimalism

Here's the *why* behind gathering richly. In Claude **Projects**, all knowledge loads into every thread, so more knowledge means a heavier, slower, more distracted context — Projects force you to keep things tiny. This workspace runs in Claude **Code**, which retrieves *selectively*: it opens only the files a task actually needs. A 40-file `context/` folder costs nothing on a task that touches two of them.

So the constraint flips. The risk isn't "too much" — it's "can't be found." Gather richly, then make every file findable through disciplined frontmatter and a current resource index. The rule is **"keep it indexed," not "keep it small."** An un-indexed file is invisible no matter how good it is.

## Frontmatter standard

Every file in `context/` and `initiatives/` carries YAML frontmatter. The minimum is always `title`, `type`, `last_updated`. Add the optional keys when they apply — they're what make retrieval and cross-linking work.

Minimum:

```yaml
---
title: Activation Funnel
type: reference
last_updated: 2026-06-14
---
```

Add when applicable: `initiative` (the slug this file belongs to), `confluence_id` / `jira_key` (so Claude can trace back to the source tool), `goals`, `bet` (the strategy bet this supports).

`type` values:

| `type` | Use for |
|---|---|
| `reference` | General knowledge, metrics, indexes, coordinates |
| `strategy` | Direction, goals, transformation phases |
| `bet` | A strategic bet and its success criteria |
| `prd` | A product requirements doc for an initiative |
| `epic` | An epic-level scope doc |
| `user-flow` | How users move through the product |
| `tracking-plan` | Events and metrics instrumentation |
| `qa-results` | Test/QA outcomes for an initiative |
| `impact-analysis` | Post-ship measured impact |
| `status` | An initiative's live heartbeat (`status.md`) |
| `briefing` | A prepared brief for a meeting/stakeholder |

This is a guide, not a rigid schema — if a file genuinely doesn't fit a listed `type`, pick the closest and move on rather than inventing a sprawling taxonomy.

## Resource-index upkeep

**The "resource index" is the bullet list under the `## Key Resources` table in CLAUDE.md** — specifically the bullets right after the `<!-- As you create… -->` comment. There is no separate `## Resource Index` heading: do **not** create one. When this reference (and the worked examples above) say "add a pointer to the resource index," it means appending a bullet to that existing list.

When you create a file, add a one-line pointer to CLAUDE.md's resource index **in the same moment** — not "later." The pointer is a path plus a half-sentence on what's inside, e.g. `context/General product context/activation-funnel.md — funnel stages, drop-off rates, definitions`. The reason this is non-negotiable: Claude Code retrieves by scanning what's indexed and named; a file that isn't pointed to from the map won't be opened, so the work of creating it is wasted. Treat "wrote a file" and "indexed the file" as one atomic action.

## What to discard as noise

Not everything pasted deserves to be saved. Discard:

- **Duplicates** — the same fact already captured elsewhere. One home per fact; link, don't copy.
- **Ephemeral chatter** — scheduling back-and-forth, "thanks, looks good," anything with no lasting signal.
- **Already-answered content** — if a doc only restates what's in the workspace, skip it. Save *new* learnings only.

When in doubt about a borderline item, prefer keeping a tight quote over a vague paraphrase — but don't hoard. The goal is a brain, not a junk drawer.

## The quality bar

For what a "good" file actually looks like — one that *interprets* rather than dumps, carries a Data Nuances section, and expresses impact concretely — see the `examples/` files. Pattern-match the bar there before writing your own.

## Common Pitfalls

1. **Don't turn CLAUDE.md into an encyclopedia.** If you paste strategy, metrics, or background prose into it, you bloat every future conversation and bury the rules that actually need to load each time. Push detail to a context file and leave a pointer.

2. **Don't keep it small to stay safe.** Treating Claude Code like Claude Projects and trimming knowledge defeats the whole investment — Code retrieves selectively, so thin context just means Claude knows less. Gather richly; manage it with the index, not by deleting.

3. **Don't write a file without indexing it.** An un-indexed file is invisible to retrieval, so the effort is silently wasted. Add the resource-index pointer in the same breath as creating the file.

4. **Don't file initiative-scoped material in context/.** Putting a PRD or a single initiative's tickets in `context/` pollutes the cross-cutting knowledge layer and makes both layers harder to trust. Ask "would this survive the initiative's cancellation?" — if no, it belongs in the initiative folder.

5. **Don't ship files with missing or wrong frontmatter.** Without `title`/`type`/`last_updated`, retrieval and cross-linking degrade and the file ages invisibly. Set frontmatter at creation, and pick the closest `type` rather than leaving it blank.

6. **Don't save noise to look thorough.** Duplicates and ephemeral chatter dilute signal and make the real knowledge harder to find. Saving less, but only what's new and load-bearing, makes the workspace smarter, not poorer.
