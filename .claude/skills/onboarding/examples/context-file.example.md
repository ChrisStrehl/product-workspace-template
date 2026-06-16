<!-- EXAMPLE — a golden sample for the onboarding skill to imitate. Fictional product (SproutLog). -->
---
title: SproutLog Activation Funnel
type: reference
last_updated: 2026-06-14
---

# SproutLog Activation Funnel

*Last updated: 2026-06-14 | Primary data source: Amplitude (cohort: all new installs, 2026-05-01 → 2026-05-31; n = 18,240) cross-checked against the App Store / Play Console install counts.*

The activation funnel is the path from install to a retained, habit-forming user. We define **activation** as *install → first care task completed* and the **north star** as *week-2 retention*. Everything before week-2 retention exists to serve it: a user who completes a care task in their first session retains at roughly 2.6x the rate of one who doesn't.

## Topline Landscape

| Stage | Volume | Conversion (from prior stage) | Prior period (Apr) | Delta |
|---|---|---|---|---|
| Install | 18,240 | — | 16,910 | — |
| Onboarding complete | 13,315 | 73.0% | 71.4% | +1.6 pp |
| First plant added | 9,587 | 72.0% | 70.8% | +1.2 pp |
| First care task completed | 5,656 | **59.0%** | 64.1% | **−5.1 pp** |
| Week-2 retention | 4,377 | 77.4% (of activated) | 78.0% | −0.6 pp |

Overall install → activated: **31.0%** (Apr: 33.2%). Install → week-2 retained: **24.0%**.

**The biggest, and worsening, leak is first plant added → first care task completed: 41% of users who add a plant never complete a single care action, and that drop got 5.1 pp worse month-over-month — it now costs us more activated users than the onboarding and plant-add steps combined.**

## Cost / Opportunity

- Closing the first-care-task gap back to April's 64.1% would have activated **~485 additional users in May** (9,587 × 5.1 pp).
- At our current week-2 retention of 77.4% on activated users, that's **~375 additional retained users/month**.
- At a blended LTV of **€12.40** per retained user (12-month, current paywall conversion), that single regression represents **~€4,650/month**, or **~€55,800/year** of recurring value left on the table — before any compounding from word-of-mouth.
- Framed for the seed-extension narrative: this one funnel step moves week-2 retention by ~1.0 pp on its own. Our stated target is 24% → 35%; this step is the cheapest available point on that path.

## Data Nuances
<!-- Honesty section. What the numbers do NOT prove. Forcing function — never delete, fill as you learn. -->

- **Correlation, not causation, on the 2.6x retention lift.** Users who complete a first care task may simply be more motivated to begin with; we have not run a holdout to prove the care task itself *causes* retention. Treat the 2.6x as directional until an experiment confirms it.
- **Definition sensitivity.** "First care task completed" only counts in-app taps on the "Done" button. A user who waters a plant in real life but never marks it done is invisible to this funnel — so the true care rate is almost certainly higher than 59%. The number we can *move* is the in-app one.
- **The May regression is partly attribution noise.** ~30% of the May install spike came from a paid TikTok campaign skewing younger/lower-intent than organic. Some of the −5.1 pp is likely cohort mix, not a product regression. Recommend segmenting organic vs. paid before over-investing.
- **iOS notification-permission confound.** Users who deny push permission at onboarding (~22%) get no reminder nudges and convert to first care task far worse. This funnel blends them with permission-granters. A permission-segmented funnel is the highest-value next cut.
- **What this data does NOT tell us:** *why* users abandon after adding a plant (confusion? no reminder yet? wrong first task?). That's a discovery question, not an analytics one — see the linked research campaign.

## Related Documents

- `context/Strategy/2026-activation-strategy.md` — the "habit, not features" bet this funnel serves
- `initiatives/first-session-onboarding/README.md` — the initiative attacking the first-care-task leak
- `discovery/campaigns/2026-05-first-session-friction/` — interviews probing *why* users stall after adding a plant
- `context/General product context/north-star-retention.md` — week-2 retention definition + history
