# DnD Assistant — Roadmap

Status legend: ✅ Done · 🔄 In Progress · ⏳ Planned · ❌ Blocked

---

## Phase 1 — Foundation ✅
- Database schema (all tables)
- Phoenix project setup

## Phase 2 — Auth ✅
- JWT via Guardian
- POST /auth/register, /auth/login, DELETE /auth/logout

## Phase 3 — Characters API ✅
- Full CRUD /api/v1/characters
- Extended fields (saving throws, spell slots, conditions, etc.)

## Phase 4 — Campaigns + Invitations ✅
- Campaign CRUD with DM role check
- Players management
- Invitation tokens

## Phase 5 — WebSocket Channel ✅
- Phoenix Channel "session:{campaign_id}"
- roll_dice, update_character events

## Phase 6 — Maps & Battle 🔄
- REST /api/v1/maps/* ✅ (TICK-006)
- WebSocket battle events ✅ (TICK-007)
- Character fields (subrace, hit_dice, conditions, favourites) ✅ (TICK-002–005)
- Battle combat actions: backend ✅, iOS ✅, web ❌ (TICK-008)
- Session management: backend ✅, iOS ✅, web ❌ (TICK-009)
- React + Konva.js web editor ✅

## Phase 7 — Deploy ✅
- Dockerfile (multi-stage alpine) ✅
- GitHub Actions → GHCR ✅
- Cloudflare DNS + SSL + WebSocket ✅
- Deployed at dnd.vitskylab.dev ✅

---

## Sprint 2 — 2026-03-29

Backend is ahead — almost all tickets done. Focus shifts to iOS and Web clients.

### iOS ✅ DONE

### Web (active)

| # | Ticket | Task |
|---|--------|------|
| 1 | TICK-008 | Combat log panel + roll dice button + HP bars + condition icons |
| 2 | TICK-009 | Players panel + End Battle button + player_joined/left + battle_ended |

### Backend (low priority)

| # | Ticket | Task |
|---|--------|------|
| 1 | TICK-010 | swagger.json: document battle + session channel events |
