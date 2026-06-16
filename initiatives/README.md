---
title: Initiatives — What We Build
type: reference
last_updated: 2026-06-14
---

# initiatives/ — What We Build

One folder per initiative, holding everything that initiative needs — so nothing is scattered across tools.

```
initiatives/[initiative-slug]/
  README.md   ← what this initiative IS: why, success criteria, decisions (changes rarely)
  status.md   ← where it IS now: tasks, blockers, progress log (changes often)
  prd.md      ← requirements
  ...         ← tracking-plan, qa-findings, impact-analysis, etc.
```

**Why README vs status:** read README once to understand the initiative; check status to see where it stands today. Separating them stops the "what's the latest?" doc from burying the "what is this?" doc.

Create new ones with the create-initiative skill (or by hand). Adapt for your role — an engineer's "initiative" might be a migration or a new service; a UX lead's might be a research project.
