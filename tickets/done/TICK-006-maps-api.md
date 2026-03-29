---
id: TICK-006
title: Maps REST API (Phase 6)
status: open
priority: high
created: 2026-03-18
projects: [backend, web]
---

## Description
DM creates and edits battle maps in the React/Konva.js web editor.
Maps are stored in the `maps` table (already exists). Each map has a grid, named
layers (tile / object / token / fog), and an optional background image URL.

---

## Database

### Table: `maps` (already exists)
```
id                  uuid PK
campaign_id         uuid FK → campaigns.id
name                varchar
width               integer        -- columns (default 20)
height              integer        -- rows (default 15)
cell_size           integer        -- pixels per cell on client (default 40)
background_image_url varchar null
layers              jsonb          -- see Layer schema below
status              varchar        -- "draft" | "active"  default "draft"
inserted_at         timestamp
updated_at          timestamp
```

### `layers` JSONB structure
```json
[
  {
    "id": "uuid-string",
    "name": "Tiles",
    "type": "tile",
    "visible": true,
    "cells": {
      "2,3": { "tile_type": "floor_stone" },
      "2,4": { "tile_type": "wall" }
    }
  },
  {
    "id": "uuid-string",
    "name": "Fog",
    "type": "fog",
    "visible": true,
    "cells": {
      "0,0": { "tile_type": "floor_stone" }
    }
  }
]
```

`cells` is a sparse map — key is `"x,y"` (integer coords), value has `tile_type`.
Only painted cells are stored. Empty cells are absent from the map.

### `tile_type` values
`floor_stone` | `floor_wood` | `floor_dirt` | `wall` | `wall_stone` |
`water` | `lava` | `grass` | `forest` | `door_open` | `door_closed` |
`stairs_up` | `stairs_down`

---

## API Endpoints

### GET /api/v1/campaigns/:campaign_id/maps
Returns short list of maps for a campaign.

**Auth:** Bearer JWT required. User must be DM of the campaign.

**Response 200:**
```json
{
  "data": [
    {
      "id": "uuid",
      "name": "Dungeon Level 1",
      "campaign_id": "uuid",
      "status": "draft",
      "inserted_at": "2026-03-22T10:00:00Z",
      "updated_at": "2026-03-22T10:00:00Z"
    }
  ]
}
```

---

### POST /api/v1/campaigns/:campaign_id/maps
Create a new map.

**Auth:** Bearer JWT. Must be DM of the campaign.

**Request body** (flat, no wrapper):
```json
{
  "name": "Dungeon Level 1",
    "width": 20,
    "height": 15,
    "cell_size": 40,
    "background_image_url": null,
    "layers": [
      {
        "id": "550e8400-e29b-41d4-a716-446655440000",
        "name": "Tiles",
        "type": "tile",
        "visible": true,
        "cells": {}
      },
      {
        "id": "550e8400-e29b-41d4-a716-446655440001",
        "name": "Objects",
        "type": "object",
        "visible": true,
        "cells": {}
      },
      {
        "id": "550e8400-e29b-41d4-a716-446655440002",
        "name": "Fog",
        "type": "fog",
        "visible": true,
        "cells": {}
      }
    ]
  }
}
```

**Response 201:** Full map object (see below).

---

### GET /api/v1/maps/:id
Get full map with all layers and cells.

**Auth:** Bearer JWT.

**Response 200 — Full Map object:**
```json
{
  "data": {
    "id": "uuid",
    "campaign_id": "uuid",
    "name": "Dungeon Level 1",
    "width": 20,
    "height": 15,
    "cell_size": 40,
    "background_image_url": null,
    "status": "draft",
    "layers": [
      {
        "id": "uuid",
        "name": "Tiles",
        "type": "tile",
        "visible": true,
        "cells": {
          "2,3": { "tile_type": "floor_stone" },
          "3,3": { "tile_type": "wall" }
        }
      }
    ],
    "inserted_at": "2026-03-22T10:00:00Z",
    "updated_at": "2026-03-22T10:00:00Z"
  }
}
```

---

### PATCH /api/v1/maps/:id
Update map (name, size, layers). Called by auto-save every 2s after edits.

**Auth:** Bearer JWT. Must be DM of the campaign.

**Request body** (all fields optional):
```json
{
  "map": {
    "name": "Dungeon Level 1 (edited)",
    "width": 20,
    "height": 15,
    "cell_size": 40,
    "background_image_url": null,
    "layers": [ /* full layers array, replaces existing */ ]
  }
}
```

**Response 200:** Full map object (same as GET).

---

### DELETE /api/v1/maps/:id
Delete map and its battle_state if any.

**Auth:** Bearer JWT. Must be DM.

**Response 200:**
```json
{ "ok": true }
```

---

### POST /api/v1/maps/:id/activate
Activates a map for battle — sets `status = "active"`, creates or resets `battle_state`.

**Auth:** Bearer JWT. Must be DM.

**Request body:** empty `{}`

**Response 200:**
```json
{
  "data": {
    "map": { /* full map object */ },
    "battle_state": {
      "id": "uuid",
      "campaign_id": "uuid",
      "map_id": "uuid",
      "status": "active",
      "round": 1,
      "current_turn_index": 0,
      "initiative_order": [],
      "token_positions": {},
      "fog_state": {}
    }
  }
}
```

---

### GET /api/v1/maps/:id/battle_state
Get current battle state for a map.

**Response 200:**
```json
{
  "data": {
    "id": "uuid",
    "campaign_id": "uuid",
    "map_id": "uuid",
    "status": "active",
    "round": 1,
    "current_turn_index": 0,
    "initiative_order": [],
    "token_positions": {
      "char-uuid-1": { "x": 5, "y": 3 }
    },
    "fog_state": {
      "0,0": "revealed",
      "1,0": "revealed"
    }
  }
}
```

---

## Error Responses

```json
{ "error": "not_found" }           // 404
{ "error": "unauthorized" }        // 401
{ "error": "forbidden" }           // 403
{ "errors": { "name": ["can't be blank"] } }  // 422
```

---

## Backend
- [x] MapController (index, show, create, update, delete)
- [x] BattleStateController (show, create, update) — replaces /activate endpoint
- [x] Routes: campaigns nested (index/create), standalone (show/update/delete/battle_state)
- [x] Maps context: CRUD functions + delete_map checks for active battle
- [x] BattleState context: create_battle_state, update_battle_state, get_battle_state_for_map
- [x] MapJSON view (summary for list, full for show/create/update with grid unpacking)
- [x] BattleStateJSON view
- [x] Auth plug: DM check on create/update/delete, member check on show
- [x] swagger.json updated (MapSummary, Map, MapInput, BattleState schemas + all endpoints)

Note: Activate endpoint is POST /api/v1/maps/:map_id/battle_state (not /activate).
Web client adapted accordingly.

## Web
- [x] Map list view (per campaign) — MapsPage.tsx
- [x] Konva.js editor: grid, layers, tile painting, fog brush — MapCanvas.tsx
- [x] Save map → PATCH /api/v1/maps/:id (auto-save 2s debounce) — MapEditorPage.tsx
- [x] New Map modal → POST /api/v1/campaigns/:id/maps
- [x] Activate map → POST /api/v1/maps/:map_id/battle_state + fix { map: } wrapper on create/update
- [x] Redirect to battle page after activation

## iOS
- N/A (players see map via WebSocket only, not REST editor)
