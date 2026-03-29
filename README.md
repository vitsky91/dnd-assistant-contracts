# DnD Assistant — Contracts

Shared coordination layer for Backend, iOS, and Web AI agents.
No Jira. No Notion. Just structured markdown that every agent can read and update.

---

## Structure

```
_contracts/
  README.md              ← this file
  ROADMAP.md             ← feature roadmap across all projects
  tickets/
    open/                ← active work (agents read this on every session)
    done/                ← completed tickets (archive, do not delete)
  decisions/             ← Architecture Decision Records (ADRs)
```

---

## How It Works

### Ticket lifecycle

```
idea → open/TICK-XXX.md → agent works → marks checkboxes → moves to done/
```

1. When a new feature is needed — create a ticket in `open/`
2. AI agents check `open/` at the start of each session
3. As work is completed, check off boxes inside the ticket
4. When ALL boxes across ALL projects are done — move file to `done/`

### Ticket format

Each ticket uses YAML frontmatter + markdown body:

```markdown
---
id: TICK-001
title: Feature Name
status: open | in-progress | done
priority: high | medium | low
created: YYYY-MM-DD
projects: [backend, ios, web]
---

## Description
What and why.

## Backend
- [ ] Task 1
- [x] Task 2 (done)

## iOS
- [ ] Task 1

## Web
- N/A
```

### Rules for AI agents

- **Always read `open/` at session start** — check if anything affects your project
- **Mark checkboxes as you complete work** — do not just write code and leave the ticket unchanged
- **One ticket = one feature** — do not mix unrelated tasks
- **Never edit tickets in `done/`** — they are immutable history
- **Add an ADR** when making a significant architectural decision

### Naming

Tickets: `TICK-XXX-short-description.md` (zero-padded, sequential)
ADRs: `ADR-XXX-short-description.md`

---

## Projects

| Project | Repo | Agent reads |
|---------|------|-------------|
| Backend | DNDAssistantBackend | `_contracts/`, `CONTRIBUTING.md`, `swagger.json` |
| iOS | DNDAssistantIos | `_contracts/`, `BACKEND_REQUIREMENTS.md` (deprecated) |
| Web | DNDAssistantWeb | `_contracts/`, `BATTLE_MAP_PLAN.md` (deprecated) |

> Loose docs in individual repos (`BACKEND_CHANGES.md`, `BACKEND_REQUIREMENTS.md`, etc.)
> are deprecated. New work goes into tickets only.

---

## API Contract

The canonical API definition is always `DNDAssistantBackend/dnd_assistant/priv/static/swagger.json`.

- Backend agents update it when endpoints change
- iOS/Web agents read it to understand request/response shapes
- Do not duplicate endpoint docs in tickets — link to swagger instead

---

## Current Open Tickets

See `tickets/open/` directory.
