---
title: Gathering Context — What to Gather and How to Ask
type: reference
last_updated: 2026-06-14
---

# Gathering Context

This is the playbook for what to pull into `context/`, and how to elicit it without turning onboarding into an interrogation. Read it when you're seeding context or filling gaps.

The mindset that makes this work: **ingestion-first.** A pasted strategy doc, landing page, or competitor table answers in thirty seconds what an interview would take twenty minutes to extract — and it's in the user's real language, not their best guess under pressure. So for every bucket below, your first move is to ask *which documents to paste*, and questions only fill the gaps the documents leave. The buckets are a guide for coverage, not a form to march through.

**When the full question bank fires.** The complete bank in each bucket below is for the **final, bounded gap-filling step (Step 4 of the skill)** — not for the start. In the initial gather step, ask only the *few short questions* needed to make sense of the material you were handed; save the deep interview for the end. The cadence rules below apply to both.

When something gets pasted, hand it to a subagent to de-noise (see `references/ingestion.md`), then route the result into the navigable `context/` subfolders (see `references/routing-rules.md`). Don't dump raw material into the main thread.

## When there are no docs (or the user answers tersely)

Never block on missing material. If the user has nothing to paste, or answers in clipped fragments:

1. **Use the "dinner-party explanation" move** — ask them to just *talk* through their product as if explaining it to someone at a dinner party. Spoken, low-pressure narration surfaces the real mental model when a blank-page question stalls.
2. **Pair it with the batched short questions** (see cadence below) so they can answer fast.
3. **If they're still terse after one nudge, don't push again** — write a *valid starter file* with `{{TODO}}` markers for what's missing, route and index it, and move on. The gaps get filled on a later run. A seeded file with honest holes beats a stalled onboarding.

## The four buckets

### Company

The foundation: what this company is and how it wins. This grounds every later judgment Claude makes.

**Documents to ask for (first):** the landing page text (or URL to scrape — see ingestion tips); mission/vision statement; the company strategy deck; any customer segmentation or persona docs; a competitor comparison table; the growth model if one exists.

**Then dictate, then ask.** One uniquely useful move: ask the user to *dictate a 1–2 minute "dinner-party explanation"* — "explain what your product does the way you would to a smart friend over dinner." Spoken explanations capture the real mental model better than polished copy.

Question bank (fill the gaps the docs left):
- What does the company actually do, in one sentence?
- Who's the customer, and how do you segment them?
- How does the company make money? What's the growth model?
- Who are the real competitors, and why do you win against each?
- What are the "irrational fears" or "scars of leadership" — the things the org is over-cautious about because of past pain?

### Product org

How the product team thinks, measures, and learns.

**Documents to ask for (first):** the product vision; written product principles; the top-level metrics dashboard or definitions; a funnel diagram; any doc describing feedback channels or research ops; any **templates** the user already works from (their PRD, user-story, or epic formats) — drop those in `templates/`, not `context/`.

Question bank:
- What's the product vision — where is this going in 2–3 years?
- What product principles guide decisions when there's no obvious answer?
- What are the top-level metrics? Which one matters most right now?
- Walk me through the funnel — the stages and where users drop.
- Where do you get user insight today? (Feedback channels, sales, CS, communities, support tickets.)
- What are the hairiest product risks — the things most likely to blow up?

### Team

The people around the work, because Claude's advice is only useful if it accounts for who has to act on it.

**Documents to ask for (first):** an org chart or team list; past retro notes and their action items; any stakeholder map.

Question bank:
- Who do you work with day to day? For each: their superpower and their weakness.
- Who are your stakeholders? For each: what they care about, and how best to approach them.
- What came out of recent retros — and which action items are still open?
- What are the team's "scars" — mistakes you've made that you must not repeat?
- Are there rollout groups (beta cohorts, internal-first, region phases) Claude should know about?

### Yourself

Sensitive by design. **Offer this privately and explain the payoff — never push it in a shared screen, a workshop, or a group setting.** Say plainly: "This one's personal, so only if you're somewhere private — it makes me a sharper coach for you specifically." If the moment isn't private, skip it and flag it for a later solo run.

**Documents to ask for (first):** written manager feedback; performance-review notes; 360 feedback summaries.

Question bank:
- What feedback has your manager given you that's worth me holding onto?
- What themes from performance reviews should I help you watch for?
- What did your last 360 surface?
- What PM wisdom do you *know* but keep forgetting to apply in the moment?

## Interview cadence

The how-you-ask matters as much as what-you-ask. Two modes:

### Batch the short-answer questions

For anything answerable in a phrase — role, top metric, competitor names, who-owns-what — send them as **one batched message**. People answer a visible list far faster than a slow drip of one-liners, and they can see the shape of what you need. This is the default for filling gaps.

### One-at-a-time for paragraph answers

For anything that needs a paragraph of real thought — the product vision, the irrational fears, a stakeholder's true motivations — go **one question at a time.** And briefly say *why each question matters and why it's the most important thing to know right now.* "I'm asking about your biggest product risk next because everything we prioritize should be hedging against it — and I don't yet have a read on it." Explaining the why earns better answers and models the coaching relationship.

### Probe the seams

You're not just collecting — you're pressure-testing. As answers come in, look for *what doesn't cohere, what's subtly missing or unspoken, what makes your expert intuition tingle.* If the stated strategy and the top metric point in different directions, say so. If everyone's a "superpower" and no one has a weakness, push. Productive friction here is the whole value.

## The write-back rule

After a conversation, capture **only the *new* learnings** — not what's already in the workspace. Re-recording known facts is noise (see routing-rules). Optimize what you write for reuse: structured, interpreted, findable. And for the most salient things — the killer quote, the precise definition, the fear named in their own language — use the **user's exact words**, not your paraphrase. Their phrasing is the signal; your summary loses it.

## Common Pitfalls

1. **Don't interrogate before you ingest.** If you ask a question a pasted doc would answer, you waste the user's time and get a worse answer than the document holds. Always lead with "what do you already have?"

2. **Don't drip paragraph questions one tiny one-liner at a time, or batch the deep ones.** Batching short answers is fast; batching deep ones gets shallow answers because each deserves focus. Match the mode to the depth, and explain why each deep question matters.

3. **Don't push the "Yourself" bucket in a shared setting.** Asking about manager feedback or review themes with others watching is a breach of trust that can poison the whole engagement. Offer it privately, explain the payoff, and skip without friction if the moment's wrong.

4. **Don't transcribe — interpret and probe.** Recording answers verbatim without testing for what doesn't cohere wastes the coaching opportunity and lets contradictions calcify into the workspace. Challenge the seams as you go.

5. **Don't re-record what's already known.** Writing back facts the workspace already holds buries the genuinely new learning under noise. Capture only the delta, in the user's exact words for the things that matter most.

6. **Don't treat the buckets as a checklist to complete in one run.** Forcing all four to full depth in a single sitting exhausts the user and produces thin, dutiful answers. Breadth first — a starter file in each — then depth on deliberate re-runs.

7. **Don't pressure for numbers the user doesn't have.** If a metric or figure is unknown, don't push for an estimate — write it as a clear `{{TODO: …}}` or leave it out of the file entirely. Never a guess, and never a half-filled metrics section implying data that isn't there.
