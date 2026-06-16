---
title: Context — What We Know
type: reference
last_updated: 2026-06-14
---

# context/ — What We Know

Slow-changing knowledge that makes Claude useful from the first message: strategy, bets, metrics, how your market/users/operations work, background on your company and product.

**Rule of thumb:** if you'd explain it to a sharp new hire in their first week, it belongs here.

## Suggested structure
- `General company context/` — company background, mission, market, business model
- `Strategy/` — direction, goals, transformation phases
- `Bets/` — strategic bets and their success criteria
- `General product context/` — metrics, funnel, operations, key references
- `User Flows/` — how users move through the product (+ screenshots)
- `Team context/` — who you work with, stakeholders, retros and "scars"
- `Yourself/` — manager feedback, review themes, growth goals (keep private)

These map to the four "gathering buckets" the onboarding skill uses (Company / Product org / Team / Yourself). Folder names aren't sacred — adapt them to what you already have.

## Adapt for your role
- **PM:** strategy, funnel, KPIs, user flows
- **Engineer:** architecture, services, runbooks, key systems
- **UX:** personas, research findings, design principles
- **Data:** data model, metric definitions, stakeholders

Update it whenever Claude lacks something it should have known. Every file here needs YAML frontmatter (`title`, `type`, `last_updated`).
