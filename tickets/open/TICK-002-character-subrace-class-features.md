---
id: TICK-002
title: Character — subrace + class_features fields
status: open
priority: medium
created: 2026-03-18
projects: [backend, ios]
---

## Description
Two fields missing from Character schema that iOS needs for identity display and Features tab.

## Backend
- [x] Migration: add `subrace` (string, nullable), `class_features` (array of string, default [])
- [x] Ecto schema: add fields
- [x] Changeset: cast both fields
- [x] CharacterJSON: include in all responses (GET list, GET detail, POST, PATCH)
- [x] Swagger: update Character schema

## iOS
- [x] `CharacterDTO`: add `subrace: String?`, `classFeatures: [String]?` — reads from server response
- [x] `CharacterRequestBody`: sends `subrace` and `class_features` to server
- [x] `CharacterDTO.apply(to:)` and `toNewCharacter()` merge both fields correctly
- [x] Character creation: `classFeatures` already populated from class data at wizard step
- [ ] Show `subrace` in character header / identity section
- [ ] Features tab: display `classFeatures` from server (data flows in, needs UI update)

## Web
- N/A
