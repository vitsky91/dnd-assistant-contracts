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
- REST /api/v1/maps/* ✅ (TICK-006 done)
- WebSocket: move_token, reveal_fog, map_loaded (TICK-007 — backend pending)
- Battle combat actions: roll_dice, apply_damage, conditions (TICK-008)
- Campaign session management: players online, end battle (TICK-009)
- React + Konva.js web editor ✅

## Phase 7 — Deploy ⏳
- Dockerfile (multi-stage alpine)
- docker-compose.yml
- GitHub Actions → GHCR
- Cloudflare DNS + SSL + WebSocket

---

## Current Sprint — Priority Order for Backend

| Priority | Ticket | Feature | Status |
|----------|--------|---------|--------|
| 1 | TICK-007 | WebSocket battle: move_token, reveal_fog, map_loaded | 🔄 Open (backend not started) |
| 2 | TICK-002 | Character — subrace + class_features | 🔄 Open |
| 3 | TICK-003 | Character — hit_dice + conditions + spell_slots | 🔄 Open |
| 4 | TICK-004 | Backgrounds — description field | 🔄 Open |
| 5 | TICK-005 | Favourite Spells | 🔄 Open |
| 6 | TICK-008 | Battle combat actions: dice, damage, conditions | 🔄 Open (depends on TICK-007) |
| 7 | TICK-009 | Campaign session: player list, kick, end battle | 🔄 Open |
