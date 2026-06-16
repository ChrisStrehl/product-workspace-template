---
title: Ingestion — Turning Raw Material into Faithful Markdown
type: reference
last_updated: 2026-06-14
---

# Ingestion

This is the de-noising toolkit: how to turn pasted text, scraped pages, slide decks, and meeting transcripts into clean, faithful, routable markdown. Read it whenever a user hands you raw material to fold into the workspace.

**Run these in subagents — for heavy material only.** Per SKILL.md, raw material is heavy and noisy — dumping a 40-page deck or a two-hour transcript into the main thread drowns the conversation. Hand the source to a subagent with one of the prompts below, let it return clean markdown, then route that result per `references/routing-rules.md`. The main thread stays a coherent conversation; the grunt work happens out of sight. For **short pastes** — a few paragraphs, a one-pager — skip the subagent entirely and structure them inline; spinning one up adds latency without buying anything.

### The handoff mechanic (don't break the quote guarantee)

When you do hand material to a subagent, **first write the pasted text to a temp file** — e.g. `.claude/data/onboarding/ingest-<n>.md` — and give the subagent the **file path**, not the text. **Never re-summarize the source yourself to fit it into the subagent's prompt.** The moment you compress the source to hand it over, you've already done the "reading for the user" that this whole reference exists to prevent — the exact-quote guarantee is gone before the subagent even starts. Path in, faithful markdown out.

## The principle: write for me, don't read for me

The core rule of ingestion: **"Let AI write for you, but don't let it read for you."** It's fine for a model to *format* and *structure* on your behalf. It is dangerous for a model to *read on your behalf* and hand you back only its summary — because the PM's edge comes from staying close to the source. A paraphrased customer complaint loses the exact words that reveal the real problem. A summarized exec quote loses the precise hedge that signals real risk.

So for anything carrying user, customer, or stakeholder *voice*, the instruction to the subagent is: **preserve copious exact quotes, verbatim, no paraphrasing or abridging.** Distill structure around the quotes, never instead of them. The PM should be able to read the output and feel like they heard it firsthand.

## Copy-paste de-noising prompts

These are ready to drop into a subagent, with the source attached. Use the one that matches the material.

### (a) Generic document or transcript → insights + exact quotes

```
You are de-noising a source document into faithful markdown for a product workspace.

Do NOT summarize away the source's own voice. Your job is to STRUCTURE, not to
replace reading.

Produce:
1. A short header: what this document is, its date/author if known.
2. The key insights, as a structured list — each insight stated plainly.
3. Under each insight, the EXACT supporting quotes from the source, verbatim, in
   blockquotes. Do not paraphrase, abridge, or "clean up" quotes.
4. An "Open questions / unclear" section for anything ambiguous in the source.

Output clean markdown. Preserve numbers, names, and dates exactly. If something is
illegible or missing, say so — never invent it.
```

### (b) Web page → faithful verbatim markdown

```
Convert this web page into faithful markdown.

Capture ALL the substantive text verbatim — headings, body copy, captions, list
items, table contents, CTAs. Preserve the wording exactly; do not summarize,
rewrite, or editorialize. Keep the heading hierarchy.

Drop only true chrome: nav bars, cookie banners, footers, ads. If a section is an
image with text, transcribe the text. Output clean markdown only.
```

### (c) Slide-deck PDF → deep per-slide description

```
Describe this slide deck slide by slide. Do NOT summarize the deck.

For EVERY slide, in order, produce:
- Slide number and title.
- Every word of text on the slide, verbatim (bullets, labels, footnotes, callouts).
- A detailed description of each chart, diagram, or image: what it shows, the axes,
  the numbers, the trend, the annotations.
- Speaker-notes content if present, verbatim.

Go slide by slide with full detail. Never collapse multiple slides into one summary.
The reader must be able to reconstruct the deck from your output. Output markdown.
```

### (d) Meeting transcript → 9-part structured extraction

```
Extract this meeting transcript into structured markdown. Do not lose the
participants' own words.

Produce these nine sections, in order:
1. Themes — the main topics discussed.
2. Decisions — what was decided (and what was explicitly left open).
3. Action items — task, owner, and due date where stated.
4. Metrics — every number, figure, or KPI mentioned, with context.
5. Risks — concerns, blockers, and worries raised.
6. Recommendations — suggestions and proposed next steps.
7. Stakeholder insights — what each key person seems to care about / their stance.
8. Exact quotes — the most important verbatim quotes, each attributed to WHO said it.
9. Leftovers — anything notable that didn't fit the buckets above.

Preserve exact wording in sections 7 and 8. Don't invent owners or dates — mark
"unstated" where the transcript doesn't say. Output clean markdown.
```

## Source-getting tips

Getting the raw material in cleanly is half the battle.

### URL scraping is unreliable

Live scraping often returns broken, JS-mangled, or paywalled junk. A more reliable trick: tell the user to **"print the webpage to PDF and upload it"** (browser → Print → Save as PDF). The rendered PDF captures what they actually see, and prompt (c) or (b) handles it cleanly. Offer this whenever a scrape comes back thin or garbled.

### Pull from connectors directly when available

If a Confluence, Notion, or similar **MCP connector is connected**, don't ask for a paste — pull the page through the connector. It's faithful by construction, carries the source IDs (capture the `confluence_id` / page ID into frontmatter), and saves the user a copy step. Reserve "please paste" for sources no connector can reach. Check what's live with `claude mcp list` before asking the user to do manual work.

## Routing handoff

De-noising produces clean markdown — it does **not** decide where that markdown lives. Once a subagent returns the distilled result, route it per `references/routing-rules.md`: every-chat vs on-demand, which of `context/` / `initiatives/` / `discovery/`, which context bucket, with correct frontmatter and a resource-index pointer added in the same breath. Ingestion and routing are two steps; don't skip the second.

## Common Pitfalls

1. **Don't let the model read on your behalf.** A summary that discards the source's own words strips the PM of the firsthand signal that drives good judgment. Make AI structure the material; keep the exact quotes intact.

2. **Don't ingest in the main thread.** Pasting a long deck or transcript into the main conversation buries the dialogue in noise and derails the coaching. Hand heavy sources to a subagent and bring back only the clean result.

3. **Don't paraphrase user or customer voice.** "Cleaning up" a complaint or quote loses the precise wording that reveals the real problem. For anything carrying voice, preserve verbatim quotes generously.

4. **Don't trust a thin scrape.** When a URL scrape returns garbled or partial text and you proceed anyway, you bake bad source material into the workspace. Ask the user to print-to-PDF and upload, or pull via a connector instead.

5. **Don't ask for a paste when a connector can fetch it.** Manual copy-paste wastes the user's time, drops the source IDs, and risks transcription gaps. If a Confluence/Notion MCP is live, pull the page directly.

6. **Don't stop at clean markdown.** De-noised output that's never routed and indexed is just as invisible as raw material. Always finish by routing the result per `references/routing-rules.md`.
