---
title: Coordinate Setup — The Orientation File
type: reference
last_updated: 2026-06-16
---

# Coordinate Setup

Onboarding builds **one** file: the orientation file `context/Reference Links.md`. It captures the coordinates CLAUDE.md needs to point at — where the team's docs, tracker, and analytics live, and how each initiative maps to a strategy bet and an external spec.

Why this and not more: orientation answers *"where does this live, and how does it relate?"* — the everyday navigation question. That's enough for Claude to do good general product work. The deeper machine-readable references (field IDs, body formats, call signatures) only matter once you build skills that *write back* to your tools, and that's the rest of the workshop — not basic onboarding. Keep the line crisp.

## What the orientation file captures

| Section | What it holds | How you'd use it |
|---|---|---|
| Docs / KB index | The key Confluence/Notion/Docs pages, with links | Jump to the source of truth for a topic |
| Tracker index | Active epics/projects, with links | Glance at what's in flight |
| Analytics / BI links | Dashboards (activation funnel, etc.) | Find the metric without hunting |
| **Initiative crosswalk** | Initiative ↔ local folder ↔ strategy bet/goal ↔ external spec | Connect "what we're building" to "why" |
| Local file index | Key files in the workspace + what each holds | Know where Claude's own working docs live |

## The orientation file template (`context/Reference Links.md`)

```markdown
---
title: Reference Links
type: reference
last_updated: 2026-06-16
---

# Reference Links — where everything lives

## Docs / KB index
| What | ID | Link |
|---|---|---|
| Product strategy 2026 | <pageId> | <url> |
| Onboarding spec | <pageId> | <url> |

## Tracker index (active work)
| Epic / project | ID | Link |
|---|---|---|
| Checkout revamp | PROJ-100 | <url> |

## Analytics / BI links
| Dashboard | Link |
|---|---|
| Activation funnel | <url> |

## Initiative crosswalk  ← the centerpiece
| Initiative | Local folder | Strategy bet / goal | External spec link |
|---|---|---|---|
| Checkout revamp | initiatives/checkout-revamp/ | Bet: lift activation | <Confluence url> |

## Local file index
| File | What it holds |
|---|---|
| context/General product context/funnel.md | the activation funnel + drop-offs |
```

**The initiative crosswalk is the centerpiece** because it's the one table that ties the three worlds together: the *strategy bet* (why we're doing it), the *local folder* (where Claude's working files live), and the *external spec* (where the team's source of truth lives). It's how a future conversation answers "what folder holds the checkout work, and which strategy goal does it serve?" without you re-explaining.

## How to find the basic coordinates

These are navigation coordinates — mostly copy-paste from URLs, not API queries:

- **Tracker project key** — read it straight from any issue URL: `…/browse/KEY-123` → the project key is `KEY`.
- **Docs space** — the space key/name from the Confluence URL, or the database id (the 32-char hex) from a Notion URL.
- **Analytics workspace** — copy the dashboard links directly from the analytics tool.
- **Page / epic IDs** — read the numeric id from the page or issue URL.

When a tracker MCP is **live**, the skill can read these directly (list active epics/projects, fetch page titles) and pre-fill the tables for the user instead of making them paste every link by hand. When no MCP is live, build the file from copy-pasted links — it still delivers all the orientation value.

Pull each initiative's folder from `initiatives/`, its strategy bet from `context/Strategy/` (or `context/Bets/`), and its external spec link from whatever the team uses as the source of truth. Then wire the file into CLAUDE.md's resource index so every conversation can find it.

## Later: execution references (out of scope for onboarding)

When you go on to build skills that **write** to your tools — creating a Jira story, posting a Confluence page, updating a Linear issue — those skills can't guess values. They need a machine-readable execution reference: the workspace constants, the field map (internal identifiers, types, value shapes), enumerated-option IDs, the body format (e.g. ADF vs. markdown), and verified call signatures. That's a separate file (kept where your write-skills can load it), discovered by querying each tool's metadata APIs.

You don't build that during onboarding. When you're ready to ship a write-skill, build the execution reference alongside it using **skill-creator** — it'll walk you through discovering the IDs and structuring the file for the agent to call. Until then, the orientation file is all you need.

## Common Pitfalls

1. **Don't try to build the execution file during onboarding.** Field IDs, ADF/body formats, and call signatures are out of scope here — they belong to the write-skills you build later. Adding them now is effort spent on coordinates nothing yet uses.
2. **Don't skip the initiative crosswalk.** It's the single table that links strategy bet ↔ local folder ↔ external spec; without it the workspace can't connect "what we're building" to "why." It's the centerpiece, not an optional row.
3. **Don't leave the file un-indexed.** An orientation file CLAUDE.md doesn't point at is invisible — add it to the resource index the moment you create it.
4. **Don't hand-collect IDs when a tracker MCP is live.** If the connector responds, read the active epics, project key, and page titles directly and pre-fill the tables — don't make the user paste links you can fetch.
5. **Don't fabricate links or IDs to fill the table.** Leave a `{{TODO}}` marker with where to find it (which URL to copy from) rather than inventing a coordinate that sends a future conversation to the wrong place.
