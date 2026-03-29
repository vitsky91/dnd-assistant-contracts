---
id: TICK-005
title: Favourite Spells
status: open
priority: medium
created: 2026-03-20
projects: [backend, ios]
---

## Description
Users can save and sync favourite spells across devices.
Favourites are account-level (not tied to a specific character).

## API

```
GET    /api/v1/spells/favorites          ?lang=ru supported
POST   /api/v1/spells/favorites          body: {spell_id}  → 201
DELETE /api/v1/spells/favorites/:spell_id               → 204
```

Response shape (GET + POST):
```json
{
  "data": [
    {
      "id": "<favorite_uuid>",
      "spell_id": "<spell_uuid>",
      "spell": { ...full Spell object... }
    }
  ]
}
```

## Backend
- [x] Migration: create `user_favorite_spells` table (id, user_id FK, spell_id FK, timestamps)
- [x] Ecto schema + context (list_favorite_spells, add_favorite, remove_favorite, favorite?)
- [x] Controller + routes (GET/POST /spells/favorites, DELETE /spells/favorites/:spell_id)
- [x] FavouriteSpellJSON view (favorites/2 with lang support)
- [x] Swagger updated (FavouriteSpell schema + endpoints)

## iOS
- [ ] `FavouriteSpellDTO`: `id: String`, `spellId: String`, `spell: SpellDTO`
- [ ] `GET /spells/favorites` on app launch — store in SwiftData
- [ ] Toggle heart button → POST / DELETE sync
- [x] Toggle heart button in SpellListView and SpellDetailView (local SwiftData only)
- [x] Favourites filter in SpellListView (`showFavouritesOnly` toggle)
- [x] Offline: FavouriteSpell SwiftData model persists locally

Note: Local implementation complete. Backend endpoints are ready — iOS needs to integrate sync.

## Web
- N/A
