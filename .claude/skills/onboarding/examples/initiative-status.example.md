<!-- EXAMPLE — a golden sample for the onboarding skill to imitate. Fictional product (SproutLog). -->
---
title: "Status: Streamline First-Session Plant Onboarding"
initiative: first-session-onboarding
type: status
last_updated: 2026-06-14
---

# Status: Streamline First-Session Plant Onboarding

## Current Status

Primary track is the **instant starter-task on plant-add (SPL-412)**, which is in development with Tomás building the same-session task path in the scheduling service — the design and tracking plan are done and approved, so this is the critical path to a first treated cohort. **Blocked:** the permission re-timing work (SPL-418) is on hold pending a legal/privacy review of moving the push prompt after sign-in, and the organic-only holdout (SPL-425) can't be configured until Jordan returns. **Who's out:** Jordan Bell (Data) is on leave until 2026-06-23, so holdout setup and the impact-read plan are paused; Sarah's first-task-per-species spec (SPL-421) is in review and not yet blocking. Target rollout to a 50% organic cohort is still 2026-08-15 if the holdout config lands the week Jordan is back.

## Tasks

- [x] Funnel analysis confirming first-care-task as the top leak — Maya
- [x] Discovery campaign synthesis (why users stall after plant-add) — Maya
- [x] Design: instant starter-task home-screen treatment — Sarah
- [x] Tracking plan drafted + reviewed — Maya / Jordan
- [ ] Backend: same-session starter-task generation (SPL-412) — Tomás *(in progress)*
- [ ] Mobile: render starter task + primary CTA on home (SPL-421) — Priya
- [ ] Privacy review for permission re-timing (SPL-418) — Devin *(blocked)*
- [ ] Configure organic-only holdout in Amplitude (SPL-425) — Jordan *(blocked — out until 06-23)*
- [ ] First-task-per-species spec finalized (SPL-421) — Sarah *(in review)*
- [ ] QA pass on iOS + Android — Priya

## Blockers & Dependencies

- **Privacy review for push-permission re-timing (SPL-418)** — state: *open, awaiting Devin to schedule with legal.* Path to resolution: Devin books the review for w/c 2026-06-16; if legal flags the post-sign-in prompt, we ship SPL-412 alone and split SPL-418 into its own initiative rather than block the starter task.
- **Holdout configuration depends on Jordan (SPL-425)** — state: *paused, owner on leave until 2026-06-23.* Path to resolution: Maya drafts the holdout spec now so Jordan can execute on day one back; rollout date holds only if config lands that week.
- **Same-session task path in scheduling service (SPL-412)** — state: *in progress, no blocker yet.* Dependency: this must merge before the mobile CTA work (SPL-421) can be tested end-to-end.

## Progress Log

- **2026-06-14 [planning]** — Drafted holdout spec ahead of Jordan's return so SPL-425 isn't a cold start; rollout date still 08-15 if config lands w/c 06-23.
- **2026-06-12 [eng]** — Tomás started SPL-412; confirmed the scheduling service needs a new "immediate task" path since it currently only accepts future-dated reminders.
- **2026-06-10 [decision]** — Folded permission re-timing into this initiative rather than spinning a separate one (same flow moment). Logged in README Key Decisions.
- **2026-06-09 [risk]** — Priya flagged ~22% push-permission denial at onboarding hurting care-task completion; added SPL-418 to scope.
- **2026-06-03 [decision]** — Chose organic-only holdout over global before/after read after Jordan showed paid-TikTok cohort mix skews the May funnel.
- **2026-05-22 [research]** — Usability testing (Sarah): users expect a care task *immediately* after adding a plant; today's first reminder can be up to 24h out, leaving an empty first session.
- **2026-05-19 [kickoff]** — Initiative opened; funnel analysis confirmed first plant added → first care task as the largest activation leak (41% drop, −5.1 pp MoM).
