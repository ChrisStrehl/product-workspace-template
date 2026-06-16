---
title: MCP Setup — Install Tool Plugins, Authenticate, Verify
type: reference
last_updated: 2026-06-16
---

# MCP Setup

Connect the user's tools by installing each one as a marketplace plugin, doing every piece of declarative wiring yourself, handing the human the few steps only they can do, and verifying before claiming anything is live.

Treat **every** named tool as a marketplace plugin — there's no "official plugin vs. plain MCP server" split to worry about. The user's tracker, docs, analytics, and meeting tools all install the same way: `/plugin install <name>@claude-plugins-official`. That uniformity is the point: one flow for everything.

## Connect everything in ONE upfront pass — before any exploration

This is Step 1 of the flow, and it runs *before* Step 2 gathers anything. The goal: wire up **all** the user's tools and collect **all** their access pointers in one pass, so exploration (Step 2) can then fan out against everything at once. The slow, wrong way is the connect-one → explore → connect-next → explore loop; it serializes the wait on every source. Connect first, point everything, verify — *then* let the parallel crawl do the heavy lifting.

Identify the tools by **function**, not by reciting names: ask what they use for *docs, tickets/roadmap, analytics, design, meetings, and code* — that catches whatever they actually use (not just the names you'd guess), and you map each to its plugin below.

## Collect the pointers, not just the connection

Connecting a plugin is rarely enough on its own. Each tool needs **pointers** to be useful — the specific things to look at — and you collect those in the *same breath* as the connection, not later. A connected Figma with no file URLs can do nothing.

When you connect a tool, ask in the same breath: **"What should I point it at?"**

- **Figma** → the specific **file URLs** the user wants looked at.
- **Confluence / Notion** → the **space key(s)** / specific pages to crawl.
- **Jira / Linear** → the **project / board key**.
- **Codebase** → the **repo(s)** to wire in (see `references/codebase-setup.md`).
- **Amplitude / analytics** → the **workspace / project** (and which dashboards matter).

Hold all of these by the end of the step — they're what Step 2 fans out against. A connection without its pointer is a dead end you'll only discover mid-crawl.

## Verify before you rely on it — before exploring

Each connector must be confirmed working with **`claude mcp list` before Step 2 leans on it.** Don't discover a dead connection mid-exploration — that wastes the whole parallel pass. "I installed it" ≠ "it works" (see the next section); the verify gate is what lets you trust the upfront pass and move on.

## Lead with the truth: what you CAN and CAN'T do

The single most important thing to internalize is the boundary. You can do all the *declarative* wiring — merging permissions, generating exact install commands, reloading plugins, verifying. You **cannot** click through a browser OAuth login. Be honest about this up front so the hand-off feels like a clean division of labor, not a failure.

| The skill CAN do (no human help) | The human MUST do |
|---|---|
| **Merge** `permissions.allow` into `.claude/settings.local.json` (read-merge-write) — **effective immediately**, kills the per-call approval prompts | Complete **OAuth**: open `/mcp` → select the server → Authenticate → finish the browser login |
| **Generate** the exact install + auth steps for each tool | **Install interactively**: `/plugin` → **Discover** tab → search → install (the `/plugin install <name>@claude-plugins-official` one-liner is a CLI-only shortcut — it may not work in the VS Code extension) |
| **Activate** changes with `/reload-plugins` — **no full restart needed** | — |
| **Verify** with `claude mcp list` (or the `/plugin` Installed tab) | — |

### Reload, not restart (why "I installed it" ≠ "it works")

After an install or enable, run **`/reload-plugins`** to activate — there's no full session restart required. Caveat: for plugins that provide MCP servers, `/reload-plugins` may warn and need `--force` because it invalidates the prompt cache. Mention this briefly so it isn't a surprise.

Two rules follow:

1. **Never claim a connector is live until `claude mcp list` shows it connected** (or it appears under the `/plugin` Installed tab as connected). "I wrote the config / ran the install" is true; "the tool is working" is a separate, verified claim. Keep them distinct.
2. **Permissions are the exception** — merged `permissions.allow` entries apply immediately, because they govern *approval*, not *tool loading*. (The *write* to `settings.local.json` may itself prompt for a one-time approval, since it widens permissions — that's expected; tell the user to approve it. Once written, the entries are live the moment the plugin reloads.)

## The clean 4-step hand-off

Do your part first (merge permissions, prepare the exact commands). Then present the human's part as a short numbered hand-off, not a wall of caveats:

> I've merged the permissions into `settings.local.json`. Now your four steps:
> 1. **Install** — run `/plugin`, open the **Discover** tab, search each tool (e.g. "atlassian"), and install it there. (In a terminal CLI you can shortcut with `/plugin install atlassian@claude-plugins-official`, but the Discover manager always works — use it in the VS Code extension.)
> 2. **Authenticate** — open `/mcp`, pick each server → Authenticate, finish the browser login.
> 3. **Activate** — run `/reload-plugins` (add `--force` if it warns about the prompt cache).
> 4. **I verify** — I'll run `claude mcp list` and confirm each one shows connected before calling it live.

If a plugin name isn't found at install time:

```
/plugin marketplace update claude-plugins-official
```

…and if it's still missing, re-add the marketplace explicitly:

```
/plugin marketplace add anthropics/claude-plugins-official
```

`claude-plugins-official` is **auto-available on startup**, so you normally install straight away — these two commands are only the recovery path. There is no `<marketplace>` placeholder to fill in; always write `@claude-plugins-official`.

## Per-tool map

These are the install commands and permission patterns for the user's tools. Add only the connectors the user actually uses — don't set up tools they don't have.

| Tool (what the user says) | Plugin name | Install command | Auth | Permission pattern | Verify |
|---|---|---|---|---|---|
| **Jira + Confluence** ("Atlassian") | `atlassian` | `/plugin install atlassian@claude-plugins-official` | OAuth | `mcp__plugin_atlassian_atlassian__*` | `claude mcp list` |
| **Figma** | `figma` | `/plugin install figma@claude-plugins-official` | OAuth | `mcp__plugin_figma_figma__*` | `claude mcp list` |
| **Linear** | `linear` | `/plugin install linear@claude-plugins-official` | OAuth | `mcp__plugin_linear_linear__*` | `claude mcp list` |
| **Notion** | `notion` | `/plugin install notion@claude-plugins-official` | OAuth | `mcp__plugin_notion_notion__*` | `claude mcp list` |
| **GitHub** | `github` | `/plugin install github@claude-plugins-official` | OAuth | `mcp__plugin_github_github__*` | `claude mcp list` |
| **Slack** | `slack` | `/plugin install slack@claude-plugins-official` | OAuth | `mcp__plugin_slack_slack__*` | `claude mcp list` |
| **Sentry** | `sentry` | `/plugin install sentry@claude-plugins-official` | OAuth | `mcp__plugin_sentry_sentry__*` | `claude mcp list` |
| **Amplitude** (analytics) | `amplitude` | `/plugin install amplitude@claude-plugins-official` | OAuth / API key | `mcp__plugin_amplitude_amplitude__*` | `claude mcp list` |
| **Chrome DevTools** (browser / QA) | `chrome-devtools` | `/plugin install chrome-devtools@claude-plugins-official` | none / local | `mcp__plugin_chrome-devtools_chrome-devtools__*` | `claude mcp list` |
| **Circleback** (meetings) | `circleback` | `/plugin install circleback@claude-plugins-official` | OAuth / API key | granular, e.g. `mcp__plugin_circleback_circleback__SearchMeetings` | `claude mcp list` |

**Naming logic (so you can derive any pattern):** a plugin tool's permission key is `mcp__plugin_<plugin>_<server>__<Tool>`, where `<plugin>` and `<server>` come from the plugin (often the same word, dashes preserved). The trailing `*` wildcards every tool the connector exposes; naming a single tool (the Circleback `SearchMeetings` row) locks it to that one capability.

## Wildcard vs. granular lock

You **merge** these into `.claude/settings.local.json` — read the existing file, add your entries to `permissions.allow`, write it back. Never overwrite the whole file. The choice between a wildcard and a granular lock is a real product decision about trust and blast radius — not a formality.

```json
{
  "permissions": {
    "allow": [
      "mcp__plugin_atlassian_atlassian__*",
      "mcp__plugin_circleback_circleback__SearchMeetings"
    ]
  }
}
```

- **Wildcard (`…__*`) = full, unprompted access** to everything that connector can do — read *and* write. Use it for tools the user works in constantly and trusts to act on their behalf (the tracker, the docs KB). The payoff is zero approval-prompt friction.
- **Granular lock (one tool named) = least privilege.** Here Circleback can only `SearchMeetings` — it can read meeting notes but cannot do anything else the plugin exposes. Use this when a connector has powerful or destructive tools you don't want auto-approved, or when you only need one narrow capability. List several specific tools instead of `*` when you need a few.

Rule of thumb: **wildcard the daily-driver read/write tools; granular-lock anything you only half-trust or only partly need.** When unsure, start granular — you can always widen later.

## CLI form (usable from Bash)

Everything above has a non-interactive CLI equivalent you can run via the Bash tool:

```bash
claude plugin install atlassian@claude-plugins-official
claude plugin list
```

The same rules still apply: the human still does OAuth in `/mcp`, and you still `/reload-plugins` (or restart) and verify with `claude mcp list` before calling anything live.

## If a tool isn't in the official marketplace

`claude-plugins-official` covers the tools above, but other marketplaces exist for anything that isn't there:

| Marketplace | Add it with | Then install with |
|---|---|---|
| Community | `/plugin marketplace add anthropics/claude-plugins-community` | `/plugin install <name>@claude-community` |
| Demo | `/plugin marketplace add anthropics/claude-code` | `/plugin install <name>@claude-code-plugins` |

Same flow throughout — install, authenticate, `/reload-plugins`, verify.

## How to run the step: start narrow, hand off clean

**Start with the single highest-value connector** — almost always the tracker (Atlassian/Jira, or Linear). One working connector that pulls in real work is worth more than five half-wired ones, and it's what makes Steps 3–4 richer. Add the rest on a later run.

Record each connector in `state.json` as `connected` / `needs_auth` / `not_set_up` based on what `claude mcp list` actually shows — never on what you wrote.

## Common Pitfalls

1. **Don't overwrite `settings.local.json` — merge into it.** Read the existing file, add your entries to `permissions.allow`, write it back. A blind overwrite wipes the user's other settings and permissions, which is far worse than a missed prompt.
2. **Don't claim a connector is live before `claude mcp list` confirms it.** Installing a plugin and a tool actually loading-and-authenticating are different events; reporting "Jira is connected" off the back of an install is the most common way to mislead the user.
3. **Don't tell the user to restart — it's `/reload-plugins`.** A full restart is unnecessary; after install/enable, `/reload-plugins` activates the change (use `--force` if it warns about the prompt cache).
4. **Don't try to complete OAuth for the user.** Only the human can do the browser login (`/mcp` → select server → Authenticate). Generate the exact commands and hand off; never pretend you authenticated.
5. **Don't leave a `<marketplace>` placeholder.** It's always `@claude-plugins-official` (auto-available); if a name isn't found, `/plugin marketplace update claude-plugins-official` or re-add it — don't guess.
6. **Don't wildcard a connector you don't fully trust.** `mcp__…__*` auto-approves write/destructive tools too. For anything powerful-but-rarely-needed, name the specific tools (the Circleback `SearchMeetings` pattern).
7. **Don't set up tools the user doesn't use.** Five connectors the team never touches is clutter and attack surface. Wire the highest-value one first, add others on demand.
8. **Don't connect a tool without grabbing its pointers.** A connected Figma with no file URLs is useless; same for Confluence with no space, a tracker with no project key, or a wired codebase with no repo. Collect files / spaces / projects / repos in the *same pass* you connect — that's what Step 2 explores against.
9. **Don't start exploring before connections are verified.** Run `claude mcp list` and confirm every connector is live *before* Step 2 leans on any of them. Finding a dead connector mid-crawl wastes the parallel pass you set up — verify the whole batch up front, then fan out.
