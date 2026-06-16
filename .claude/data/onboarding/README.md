# Onboarding state

The `onboarding` skill tracks workspace maturity here so re-runs know what's already done and what's
still thin.

**On a first run, `state.json` does not exist yet** — that absence is how Phase 0 detects a first
run. The skill creates `state.json` at the end of the first run and updates it at the end of every
run afterward. Don't hand-create `state.json` from this README; let the skill make it.

Schema:

```json
{
  "maturity_level": 2,
  "rungs": {
    "hire": "done",
    "onboard_context": "partial",
    "connect": "done",
    "coordinates": "todo",
    "initiatives": "todo"
  },
  "context_coverage": {
    "company": "seeded",
    "product_org": "thin",
    "team": "seeded",
    "yourself": "empty"
  },
  "connectors": {
    "atlassian": "connected",
    "amplitude": "needs_auth"
  },
  "last_run": "YYYY-MM-DD"
}
```

Values: rung status is `todo` | `partial` | `done`; coverage is `empty` | `thin` | `seeded`;
connector status is `not_set_up` | `needs_auth` | `connected`.
