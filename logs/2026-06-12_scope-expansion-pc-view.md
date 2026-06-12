# Scope Expansion — PC View

## Date
2026-06-12

## Session
Scope expansion discussion

## Conducted by
Hans

## Goal
Capture Ron's direction to unify iPhone and PC into a single app and establish the PC view scope.

## Summary
Ron directed that the PC dashboard is not a future-phase separate deliverable — it belongs in the same `kv-machines.html` as the iPhone view, accessed via auto-detected view switching with a manual override. Full scope: admin configuration (locations, machines, slot layouts), product and price assignment from both views, inventory dashboard, cost/margin analytics, and CSV export on the PC side. CONFIG migrated from hardcoded JS to GitHub-synced `config.json`.

## Decisions
- iPhone + PC views live in the same `kv-machines.html` — one file, two views
- Auto-detect mobile vs. desktop on load; manual override persists in localStorage
- CONFIG block removed from HTML source; replaced by `config.json` on GitHub (pull on load, push on save from PC admin)
- Product/price assignment available from both iPhone and PC views
- PC view adds: location/machine/slot CRUD, analytics (cost/margin), CSV export
- Knoxx reviewed config.json architecture — approved; simultaneous-write risk acceptable for single-operator use; "Last synced" timestamp recommended

## Work completed
- `C:/Users/ronsi/AI_Workshop/projects/KV Machines/CLAUDE.md` — updated with unified-app scope, architecture notes, updated next_actions
- `C:/Users/ronsi/AI_Workshop/projects/KV Machines/instructions/project_brief.md` — updated with PC view, config.json architecture, revised deliverables and out-of-scope
- `C:/Users/ronsi/AI_Workshop/projects/KV Machines/instructions/work_orders.json` — WO-03 and WO-04 closed; WO-05 (Tuxx PC design) and WO-06 (Finn config.json + PC engineering) added

## Next actions
- Tuxx: execute WO-05 — PC view design
- Hans: review and approve Tuxx's design before releasing WO-06 to Finn

## Open questions
- None
