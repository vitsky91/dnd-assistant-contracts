---
id: TICK-001
title: Russian Spell Translations + lang param
status: done
priority: high
created: 2026-03-18
completed: 2026-03-20
projects: [backend, ios]
---

## Description
Add Russian translations for all 875 spells. Support `?lang=ru` query param on spell endpoints.
Source: dnd.su (community translations) + Claude API for remaining 18.

## Backend
- [x] Migration: add `description_ru`, `higher_levels_ru` to `spells` table
- [x] Ecto schema: add fields
- [x] `?lang=ru` param on GET /spells, GET /spells/:id, GET /characters/:id/spells
- [x] SpellJSON: fallback `description_ru || description`
- [x] Import script: `fetch_translations_ru.py` (dnd.su scraper)
- [x] Import script: `translate_missing_ru.py` (Claude API for 18 remaining)
- [x] `spells_all.json` updated with all translations
- [x] Swagger updated

## iOS
- [x] `SpellDTO` reads `descriptionRu`, `higherLevelsRu`
- [x] Sends `?lang=ru` when Russian selected in settings

## Web
- N/A (web editor is DM-only, no spell browsing)
