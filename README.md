# Product Workspace Template — Claude Code

A starting workspace that turns Claude Code into a teammate who already knows your product, can reach your tools, and runs your repetitive work end-to-end.

Built for **"Claude Code for Product Managers & Friends."** It's PM-flavored on purpose — but the structure (context / discovery / initiatives) is the same whatever your role. You'll adapt the specifics to yours. That adapting *is* the skill we're teaching.

## The idea in one picture

Three layers, each building on the one below:

1. **Context** — a folder of markdown so Claude knows your product as well as you do. *Table stakes.*
2. **Connectors (MCPs)** — read/write pipes to your real tools (Jira, Confluence, Amplitude…). *Plumbing.*
3. **Skills** — reusable workflows that chain context + connectors into a finished task. *The highest leverage.*

## Quick start (in order)

1. **Copy this repo** somewhere on your machine and open it in Claude Code.
2. **Run the onboarding skill** — say: *"Run the onboarding skill."* It interviews you for a few minutes and rewrites this workspace around you.
3. **Run a skill on real work** — create an initiative, draft a PRD.
4. **Connect a tool** — add one MCP from the official marketplace (onboarding tells you which).
5. **Build your own skill** — use skill-creator on a task you do every week.

## What's here

```
README.md          ← you are here
BACKLOG.md         ← brain-dump inbox for raw ideas
.claude/CLAUDE.md  ← the workspace "brain" (loaded into every conversation)
.claude/skills/    ← your reusable workflows (start with onboarding)
context/           ← what we know: strategy, metrics, user flows
discovery/         ← how we learn: research questions, interviews, OSTs
initiatives/       ← what we build: one folder per initiative
templates/         ← standard formats for PRDs, stories, epics, bugs
```

## The one habit that matters

When something is slightly off — Claude missed context, a skill stumbled — **fix the file, not just the chat.** Update CLAUDE.md, add a context file, tweak the skill. The workspace gets smarter every time you do. That's the whole game.
