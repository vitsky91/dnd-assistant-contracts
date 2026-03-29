---
id: TICK-009
title: Campaign Session Management
status: open
priority: medium
created: 2026-03-22
projects: [backend, web, ios]
---

## Description
DM needs to manage who is in the campaign and track session state.
Currently players can join via invite token but the DM has no web UI for:
- Seeing which players are online in the session channel
- Kicking a player from the campaign
- Ending a battle and archiving its result
- Starting the next battle round (next turn / end battle already in TICK-007 but needs UI)

Also missing: `GET /api/v1/campaigns/:id` single campaign endpoint (currently only list exists).

---

## API Endpoints

### GET /api/v1/campaigns/:id
Get single campaign with player list.

**Auth:** Bearer JWT. Must be member or DM.

**Response 200:**
```json
{
  "data": {
    "id": "uuid",
    "name": "...",
    "description": null,
    "ruleset": "2024",
    "status": "active",
    "dungeon_master_id": "uuid",
    "players": [
      { "id": "uuid", "username": "Aragorn", "avatar_url": null }
    ],
    "inserted_at": "...",
    "updated_at": "..."
  }
}
```

### DELETE /api/v1/campaigns/:id/players/:player_id
DM removes a player from the campaign.

**Auth:** Bearer JWT. Must be DM.

**Response 200:**
```json
{ "ok": true }
```

### POST /api/v1/campaigns/:id/end_battle
DM ends the current active battle. Sets `battle_states.status = "ended"`,
sets `maps.status = "draft"`. Returns updated map.

**Auth:** Bearer JWT. Must be DM.

**Response 200:**
```json
{ "data": { "map": { /* full map */ }, "battle_state": { /* ended state */ } } }
```

---

## Channel Events

### Server → All clients (via "session:{campaign_id}")

#### `player_joined`
Sent when a client joins the channel.
```json
{ "player_id": "uuid", "username": "Aragorn" }
```

#### `player_left`
Sent when a client leaves the channel.
```json
{ "player_id": "uuid", "username": "Aragorn" }
```

#### `battle_ended`  *(already in TICK-007 but define payload here)*
```json
{ "reason": "dm_ended" }
```

---

## Backend
- [x] `GET /api/v1/campaigns/:id` — now returns players array in response
- [x] `DELETE /api/v1/campaigns/:id/players/:player_id` — already existed
- [x] `POST /api/v1/campaigns/:id/end_battle` — ends active battle, archives map, broadcasts battle_ended
- [x] Broadcast `player_joined` / `player_left` in SessionChannel on join/leave (terminate/2)
- [ ] swagger.json updated (new endpoints + player_joined/left events)

## Web
- [ ] Players panel in BattlePage sidebar — shows online players (green dot), DM can click to kick
- [ ] "End Battle" button for DM in BattlePage header
- [ ] Handle `player_joined` / `player_left` — update online player list
- [ ] Handle `battle_ended` — show "Battle ended" overlay, Back button

## iOS
- [ ] Handle `player_joined` / `player_left` — show toast notification
- [ ] Handle `battle_ended` — dismiss battle map view, return to campaign lobby
