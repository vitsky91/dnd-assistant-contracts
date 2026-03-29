---
id: TICK-008
title: Battle Combat Actions (Phase 6 continuation)
status: open
priority: high
created: 2026-03-22
projects: [backend, web, ios]
depends_on: [TICK-007]
---

## Description
During an active battle session players and DM need to:
- Roll dice (attack, damage, skill checks) — result broadcast to all clients
- Apply HP damage / healing to characters — persisted in `battle_states.token_positions` or characters table
- Track conditions (poisoned, stunned, etc.) per token — persisted in `battle_states`

These events flow over the existing "session:{campaign_id}" Phoenix Channel.

---

## Channel Events

### Client → Server

#### `roll_dice`
Any connected client (player on their turn, or DM at any time).
```json
{
  "dice": "1d20",
  "purpose": "attack",
  "character_id": "uuid"
}
```
`dice` format: `"NdX"` e.g. `"2d6"`, `"1d20"`.

#### `apply_damage`
DM only.
```json
{
  "character_id": "uuid",
  "amount": -12,
  "type": "damage"
}
```
`amount` is negative for damage, positive for healing.

#### `set_condition`
DM only.
```json
{
  "character_id": "uuid",
  "condition": "poisoned",
  "active": true
}
```
Conditions: `poisoned | stunned | blinded | paralyzed | prone | invisible | frightened | grappled | restrained | exhausted | dead`

---

### Server → All clients

#### `dice_rolled`
```json
{
  "character_id": "uuid",
  "player_username": "Aragorn",
  "dice": "1d20",
  "result": 17,
  "rolls": [17],
  "purpose": "attack",
  "timestamp": "2026-03-22T10:00:00Z"
}
```

#### `hp_updated`
```json
{
  "character_id": "uuid",
  "hp": 18,
  "max_hp": 30,
  "delta": -12
}
```
Backend reads current HP from `characters` table, applies delta, persists, broadcasts.

#### `condition_changed`
```json
{
  "character_id": "uuid",
  "condition": "poisoned",
  "active": true
}
```

---

## Backend
- [x] `roll_dice` handler — supports both `notation` and `dice`+`purpose`+`character_id` formats
- [x] `apply_damage` handler — updates `characters.hit_points` (clamped [0, max_hp]), broadcasts `hp_updated`; DM-only
- [x] `set_condition` handler — updates `battle_states.conditions`, broadcasts `condition_changed`; DM-only
- [x] `conditions` stored as JSONB in `battle_states`: `%{"char-uuid" => ["poisoned", "prone"]}`
- [x] Migration: `20260326125408_add_conditions_to_battle_states`
- [x] swagger.json → moved to TICK-010

## Web
- [ ] Combat log panel in BattlePage sidebar — shows dice_rolled, hp_updated, condition_changed events (chronological, newest on top)
- [ ] Roll dice button in InitiativePanel (on current player's turn) → sends roll_dice
- [ ] HP bar per token on BattleCanvas (small bar under token circle, green→red)
- [ ] Condition icons on tokens (tiny skull / eye-slash / etc.)

## iOS
- [ ] Receive `dice_rolled` → show roll result banner (dice emoji + value, fades after 3s)
- [ ] Receive `hp_updated` → update HP bar on character token
- [ ] Receive `condition_changed` → show condition icon on token
- [ ] Player can send `roll_dice` on their turn (roll button in HUD)
