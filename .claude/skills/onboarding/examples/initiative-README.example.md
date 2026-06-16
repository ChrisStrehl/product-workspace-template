<!-- EXAMPLE — a golden sample for the onboarding skill to imitate. Fictional product (SproutLog). -->
---
title: Streamline First-Session Plant Onboarding
id: first-session-onboarding
status: defining
goals: [G1, G3]
bet: "If we cut first-session friction so a user adds a plant AND completes one care task before leaving, week-2 retention rises because the habit loop starts on day zero."
phase: Discovery → Definition
start_date: 2026-05-19
target_date: 2026-08-15
owner: Maya Chen
last_updated: 2026-06-14
---

# Streamline First-Session Plant Onboarding

## Why This Matters

41% of users who add their first plant never complete a single care task, and that leak widened 5.1 pp in May (see `context/General product context/activation-funnel.md`). It is now the largest single drop in the activation funnel — bigger than onboarding and plant-add combined.

**Cost of inaction:** at current LTV (€12.40/retained user) the regression alone is ~€55,800/year of recurring value, and it directly stalls our 24% → 35% week-2 retention target — the metric the seed-extension round is underwritten on. Every month we don't fix the first session, we lose ~375 retained users we already paid to acquire.

**Who feels it:** first-time plant owners ("serial plant killers") who installed *because* they need help and then hit a dead first session — they add a plant, see no immediate value, and churn. Maya (PM) owns the number; Renata (Head of Product) is accountable to investors for it.

## Stakeholder Context

- **Renata Vogel (Head of Product), 2026-05-16:** week-2 retention is the board's #1 ask for the extension round — this initiative is the top activation bet for the half.
- **Sarah Lindqvist (Designer), 2026-05-22:** usability testing showed users expect a care task to appear *immediately* after adding a plant; today the first reminder can be up to 24h out, so the session ends with nothing to do.
- **Tomás Iglesias (Backend), 2026-05-28:** generating an instant "starter task" on plant-add is feasible but the scheduling service assumes future-dated reminders only — needs a same-session task path.
- **Jordan Bell (Data), 2026-06-02:** the May drop is partly paid-TikTok cohort mix; recommends an organic-only holdout to read true impact.
- **Priya Raman (Lead Mobile), 2026-06-09:** flagged that ~22% deny push permission at onboarding and convert far worse — the permission ask timing should be part of scope.

## Work Streams

1. **Instant first care task on plant-add** — generate a "Water me now" (or species-appropriate) task the moment a plant is added, so the session always ends with one actionable item. → SPL-412
2. **Permission ask re-timing** — move the push-permission prompt from app-open to *after* the first plant is added, when intent is highest. → SPL-418
3. **First-session UI nudge** — visually surface the starter task with a single primary CTA on the home screen. → SPL-421
4. **Tracking + holdout** — instrument the new events and run an organic-only holdout so we can attribute impact cleanly. → SPL-425

## Success Criteria

- [ ] Instant starter-task generation shipped on plant-add (iOS + Android)
- [ ] Push-permission prompt re-timed to post-plant-add
- [ ] First plant added → first care task completed recovers to **≥ 64%** (May baseline 59%)
- [ ] Week-2 retention improves by **≥ 1.5 pp** on the treated cohort vs. holdout
- [ ] No regression in onboarding-complete rate (guardrail: stays ≥ 72%)
- [ ] Tracking plan live and validated in Amplitude before rollout

## Linked Artifacts

| Type | Link |
|---|---|
| Funnel analysis | `context/General product context/activation-funnel.md` |
| Activation strategy | `context/Strategy/2026-activation-strategy.md` |
| Discovery campaign | `discovery/campaigns/2026-05-first-session-friction/` |
| PRD | `initiatives/first-session-onboarding/prd.md` |
| Tracking plan | `initiatives/first-session-onboarding/tracking-plan.md` |
| Jira epic | SPL-410 |
| Figma flow | (Figma) First-Session v3 — `context/General product context/Reference Links.md` |

## Key Decisions

| Date | Decision | Rationale |
|---|---|---|
| 2026-05-23 | Generate an *instant* starter task instead of shortening the first scheduled reminder | A shortened reminder still leaves an empty session if the user closes the app first; an instant task guarantees a day-zero action. Shortening was rejected as it doesn't fix the "nothing to do right now" problem. |
| 2026-06-03 | Run an organic-only holdout rather than a global before/after read | Jordan showed the May paid-TikTok cohort skews the funnel; a global read would conflate cohort mix with product impact. Before/after was rejected as un-attributable. |
| 2026-06-10 | Include permission re-timing in this initiative rather than a separate one | The push prompt and the first care task are the same moment in the flow; splitting them would mean two flow reworks. A separate initiative was rejected to avoid double-handling the onboarding screens. |

## Open Questions

- Does the instant starter task itself cause retention, or only correlate? Needs the holdout to confirm. — owner: Jordan Bell / SPL-425
- What's the right *first* task per species — water, light-check, or "do nothing yet"? — owner: Sarah Lindqvist / SPL-421
- Will re-timing the permission ask lower overall opt-in rate even as it lifts care-task completion? Guardrail TBD. — owner: Priya Raman / SPL-418
- Should the starter task fire a notification immediately, or only show in-app to avoid feeling spammy? — owner: Maya Chen / SPL-412
