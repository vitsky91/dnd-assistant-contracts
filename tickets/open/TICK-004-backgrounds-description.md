---
id: TICK-004
title: Backgrounds — description field
status: open
priority: low
created: 2026-03-18
projects: [backend, ios]
---

## Description
Backgrounds lack a flavour/lore description. iOS character creation wizard shows only mechanical
benefits (skill proficiencies, tool proficiencies, feature). Adding `description` improves UX
during character creation.

## Backend
- [x] Migration: N/A — backgrounds are static data, not DB
- [x] Populate descriptions for all backgrounds (29 backgrounds: 13 × 2014, 16 × 2024)
- [x] BackgroundJSON: included automatically — game_data_json passes data through
- [x] Swagger: added Background schema + updated response schemas

## iOS
- [ ] `BackgroundDTO`: add `description: String?`
- [ ] `Step3_BackgroundView`: show description below background name (always visible, not just on select)

## Web
- N/A
