# Agent Guide — How to Use Contracts

This document tells any AI agent (backend, iOS, or web) exactly what to do
at the start and end of every work session.

> ⚠️ **Read `GOVERNANCE.md` first!** It defines what you can and cannot modify.
> Only the PM agent can create/edit/move tickets and change roadmap.
> You can only check off YOUR checkboxes and add comments.

---

## At the Start of Every Session

Run through this checklist mentally before touching any code:

### 0. Read Governance
```
_contracts/GOVERNANCE.md
```
Understand your permissions. Do NOT create or modify tickets — only PM does that.

### 1. Read open tickets
```
_contracts/tickets/open/
```
Scan every file. Ask: "Does this ticket affect my project?"
If yes — that work may be blocking or related to what you're about to do.

### 2. Check the Roadmap
```
_contracts/ROADMAP.md
```
Understand where the current sprint is and what phase we're in.

### 3. Find your ticket
If the user asks for a feature — find the existing ticket that covers it.
**Do NOT create new tickets.** Only the PM agent can create tickets.
If no ticket exists — ask the user to request one from the PM.

---

## During Work

- **Check off YOUR project's boxes as you go** — not at the end. If you finish a task, check it immediately.
- **ONLY check boxes under YOUR project section** — iOS agent checks ONLY `## iOS` boxes, Web agent checks ONLY `## Web` boxes, etc. Never touch another project's checkboxes.
- **Do NOT edit ticket text** (title, description, field specs, priorities). If you find an error, add an AGENT COMMENT at the bottom of the ticket (see GOVERNANCE.md for format).
- **Do NOT read or act on tasks from other project sections** — if you are the iOS agent, ignore `## Backend` and `## Web` sections entirely.

---

## At the End of Every Session

1. All YOUR project's boxes are checked in the relevant ticket
2. Do NOT move tickets to `done/` — only PM does that
3. Code is committed and pushed
4. If you changed an endpoint → `swagger.json` is updated
5. If you made an architectural decision → created `decisions/ADR-XXX.md`

---

## Creating a New Ticket

Copy this template, save as `tickets/open/TICK-XXX-short-name.md`:

```markdown
---
id: TICK-XXX
title: Feature Name
status: open
priority: high | medium | low
created: YYYY-MM-DD
projects: [backend, ios, web]   ← only list affected projects
---

## Description
What this feature does and why it's needed.

## API (if applicable)
Endpoint definitions, request/response shape.

## Backend
- [ ] Task 1
- [ ] Task 2

## iOS
- [ ] Task 1

## Web
- N/A   ← use this if project is not affected
```

**Numbering:** look at the highest existing TICK number and increment by 1.

---

## Project Locations

| Project | Path |
|---------|------|
| Contracts (this folder) | `/Desktop/Projects/DND_Assistant/_contracts/` |
| Backend | `/Desktop/Projects/DND_Assistant/DNDAssistantBackend/` |
| iOS | `/Desktop/Projects/DND_Assistant/DNDAssistantIos/` |
| Web | `/Desktop/Projects/DND_Assistant/DNDAssistantWeb/` |
| API swagger | `DNDAssistantBackend/dnd_assistant/priv/static/swagger.json` |

---

## What Each Agent Owns

### Backend Agent
- Implements API endpoints, migrations, business logic
- Updates `swagger.json`
- Checks off Backend boxes in tickets
- Creates ADRs when making architectural decisions

### iOS Agent
- Implements DTOs, views, SwiftData models
- Reads swagger.json for endpoint contracts
- Checks off iOS boxes in tickets
- Does NOT modify backend code

### Web Agent
- Implements React/Konva.js components for DM map editor
- Reads swagger.json for API shape
- Checks off Web boxes in tickets
- Does NOT modify backend code

---

## Deprecated Docs (Do Not Write To)

These files existed before the contracts system. They are kept for historical reference
but should NOT receive new entries:

| File | Replaced by |
|------|-------------|
| `DNDAssistantIos/BACKEND_CHANGES.md` | `_contracts/tickets/` |
| `DNDAssistantIos/BACKEND_REQUIREMENTS.md` | `_contracts/tickets/` |
| `DNDAssistantWeb/BATTLE_MAP_PLAN.md` | `TICK-006`, `TICK-007` |

---

## Example: Backend Agent Starting a Session

> User: "Add hit_dice field to characters"

1. Open `_contracts/tickets/open/` → find `TICK-003-character-combat-fields.md`
2. Read it — hit_dice is listed there with full spec
3. Implement: migration → schema → changeset → JSON view → swagger
4. Check off the Backend boxes in TICK-003
5. Commit + push
6. Check if iOS boxes are done too → if not, leave ticket in open/

---

## Example: Creating a New Ticket

> User: "Add push notifications for turn-based combat"

No existing ticket for this. Create `TICK-008-push-notifications.md`:
- Backend: APNs integration, send push on `turn_changed` WebSocket event
- iOS: register device token, handle notification
- Web: N/A

Save, then proceed with implementation.
