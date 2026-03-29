---
id: TICK-010
title: Swagger — document battle + session events
status: open
priority: low
created: 2026-03-29
projects: [backend]
---

## Description
swagger.json is missing documentation for WebSocket channel events added in TICK-008 and TICK-009.
Also missing: `POST /campaigns/:id/end_battle` endpoint.

This is a docs-only change — no code logic affected.

## Backend
- [ ] swagger.json: document `roll_dice`, `apply_damage`, `set_condition` channel events (TICK-008)
- [ ] swagger.json: document `player_joined`, `player_left`, `battle_ended` channel events (TICK-009)
- [ ] swagger.json: document `POST /api/v1/campaigns/:id/end_battle` endpoint (TICK-009)

## iOS
- N/A

## Web
- N/A
