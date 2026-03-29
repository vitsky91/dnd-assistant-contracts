---
id: TICK-003
title: Character — hit_dice, conditions, spell_slots
status: open
priority: high
created: 2026-03-18
projects: [backend, ios]
---

## Description
Combat tab and Spells tab need these real-time fields synced via PATCH.
These are frequently updated during a session.

## Field specs

| Field | Type | Default | Notes |
|-------|------|---------|-------|
| `hit_dice` | map | `%{}` | `%{die: "d8", total: 5, remaining: 5}` |
| `conditions` | array of string | `[]` | enum: blinded, charmed, deafened, exhaustion, frightened, grappled, incapacitated, invisible, paralyzed, petrified, poisoned, prone, restrained, stunned, unconscious |
| `spell_slots` | map | `%{}` | `%{"1" => %{total: 4, used: 1}, "2" => %{total: 2, used: 0}}` |

## Backend
- [x] Migration: add `hit_dice` (map/jsonb), `conditions` (array of string), `spell_slots` (map/jsonb)
- [x] Ecto schema: add fields with types
- [x] Changeset: cast all three fields
- [x] CharacterJSON: include in all responses
- [x] Swagger: update Character schema with examples

## iOS
- [ ] `CharacterDTO`: add `hitDice`, `conditions` (spellSlots local model exists, needs backend field sync)
- [ ] Combat tab: hit dice tracker (CS-12, needs backend `hit_dice` field)
- [ ] Combat tab: conditions picker (CS-11, needs backend `conditions` field)
- [x] Spells tab: spell slot tracker per level — local SpellSlot SwiftData model + SpellSlotsView UI
- [x] Rest logic: long rest resets all slots, short rest resets Warlock Pact Magic

Note: SpellSlots already work locally. Backend fields are ready — iOS needs to integrate.

## Web
- N/A (battle map shows tokens, not full character sheet)
