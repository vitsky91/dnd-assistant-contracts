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
- WebSocket battle events: backend ✅, web ✅, iOS ❌ (TICK-007)
- Battle combat actions: backend ✅, web ❌, iOS ❌ (TICK-008)
- Session management: backend ✅, web ❌, iOS ❌ (TICK-009)
- React + Konva.js web editor ✅

## Phase 7 — Deploy ✅
- Dockerfile (multi-stage alpine) ✅
- GitHub Actions → GHCR ✅
- Cloudflare DNS + SSL + WebSocket ✅
- Deployed at dnd.vitskylab.dev ✅

---

## Sprint 2 — 2026-03-29

Backend is ahead — almost all tickets done. Focus shifts to iOS and Web clients.

### iOS — Critical Path (sequential)

| # | Ticket | Task | Блокирует |
|---|--------|------|-----------|
| 1 | TICK-007 | WebSocket: SwiftPhoenixClient + SessionChannel + battle view | TICK-008, TICK-009 iOS |
| 2 | TICK-008 | Battle UI: dice_rolled, hp_updated, condition_changed handlers | — |
| 3 | TICK-009 | Session: player_joined/left toasts, battle_ended dismiss | — |

### iOS — Parallel (independent, can run anytime)

| # | Ticket | Task |
|---|--------|------|
| 4 | TICK-002 | Show subrace in header + classFeatures in Features tab |
| 5 | TICK-003 | CharacterDTO: hitDice + conditions + combat UI |
| 6 | TICK-004 | BackgroundDTO: description + show in wizard |
| 7 | TICK-005 | Favourite spells sync (POST/DELETE/GET) |

### Web (independent, can run in parallel with iOS)

| # | Ticket | Task |
|---|--------|------|
| 1 | TICK-008 | Combat log panel + roll dice button + HP bars + condition icons |
| 2 | TICK-009 | Players panel + End Battle button + player_joined/left + battle_ended |

### Backend (low priority)

| # | Ticket | Task |
|---|--------|------|
| 1 | TICK-010 | swagger.json: document battle + session channel events |
