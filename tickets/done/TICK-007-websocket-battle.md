---
id: TICK-007
title: WebSocket Battle Events (Phase 6)
status: open
priority: high
created: 2026-03-18
projects: [backend, web, ios]
---

## Description
Real-time battle events over Phoenix Channel "session:{campaign_id}".
Requires TICK-006 (Maps API) to be done first.

## Events

### Client → Server (DM only)
```
move_token    %{token_id, x, y}
reveal_fog    %{cells: [{x, y}]}
```

### Server → All clients
```
map_loaded    %{map}                    — full map structure
token_moved   %{token_id, x, y}
fog_updated   %{cells: [{x, y, revealed: true}]}
```

## Backend
- [x] Add move_token handler in SessionChannel
- [x] Add reveal_fog handler in SessionChannel (+ hide_fog bonus)
- [x] Broadcast map_loaded when DM creates battle_state via REST
- [x] Persist token_positions + fog_state to battle_states table

## Web (DM editor)
- [x] Subscribe to "session:{campaign_id}" after map activation
- [x] Handle map_loaded — render initial state
- [x] Click token + click destination → send move_token (right-click cancels)
- [x] Fog brush (reveal/hide) → send reveal_fog / hide_fog
- [x] Receive token_moved, fog_updated — update canvas via battleStore

## iOS (Players)
- [x] Add SwiftPhoenixClient via SPM + implement SessionChannel base (connect, join, leave, reconnect)
- [x] Handle map_loaded — show battle map view (render grid + tokens + fog)
- [x] Handle token_moved — animate token position change
- [x] Handle fog_updated — reveal/hide cells on map
- [x] Players are read-only: cannot send move_token / reveal_fog

## Depends on
- TICK-006 must be complete
