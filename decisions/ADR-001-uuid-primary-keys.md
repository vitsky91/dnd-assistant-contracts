---
id: ADR-001
title: UUID (binary_id) as primary keys for all tables
status: accepted
date: 2026-03-01
---

## Decision
All database tables use UUID (`binary_id`) as primary key, not auto-increment integer.

## Reasoning
- IDs are safe to expose in URLs and API responses
- No sequential guessing (security)
- Works well with distributed/offline-first iOS sync
- Phoenix/Ecto supports it natively via `--binary-id` flag

## Consequences
- All foreign keys are also UUIDs
- Slightly larger storage than integers (acceptable)
- iOS SwiftData models store IDs as `String`
