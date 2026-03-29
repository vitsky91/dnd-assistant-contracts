---
id: ADR-002
title: CORS via Corsica plug
status: accepted
date: 2026-03-22
---

## Decision
Use `corsica ~> 2.0` Plug for CORS handling in the Phoenix endpoint.

## Reasoning
- Browser preflight OPTIONS requests were returning 404 (no CORS handling)
- Corsica is the standard Elixir CORS library, actively maintained
- Placed before `Plug.Parsers` in endpoint pipeline to intercept OPTIONS early

## Allowed origins
- `http://localhost:5173` — local Vite dev server (Web editor)
- `http://localhost:4000` — local Phoenix
- `~r{^https?://.*\.vitskylab\.dev$}` — all subdomains of vitskylab.dev

## Consequences
- iOS app uses JWT over HTTPS — no CORS concern
- Web editor (React) can now make cross-origin requests in dev and prod
