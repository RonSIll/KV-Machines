# CLAUDE.md — KV Machines

Mobile-first iPhone PWA + PC dashboard for KellyVend LLC vending machine restocking and inventory tracking. Single `kv-machines.html` file — iPhone and PC views in one app.

## next_actions
- Tuxx: design PC view (WO-05) — layout, admin panels, analytics, product/price assignment UI
- Finn: after WO-05 approved — migrate CONFIG to config.json and build PC view (WO-06)
- Ron: update CONFIG / config.json with real location names, machine names, and slot layouts when ready
- Ron: if app is reinstalled or localStorage is cleared, re-enter PAT via the setup prompt (PAT in `instructions/work_orders.json`)

## Summary
Ron visits vending machines, counts inventory before and after restocking, and tracks what sold. The app is a single `kv-machines.html` file hosted on GitHub Pages. iPhone view handles restocking in the field; PC view handles admin configuration and analytics at the desk.

## Goal
Unified `kv-machines.html` with auto-detecting iPhone/PC views, GitHub sync for both `config.json` and `inventory.json`, full admin configuration from PC, and cost/margin analytics with CSV export.

## Deliverables
- `kv-machines.html` (single self-contained file) → `src/`
- `config.json` on GitHub — locations, machines, slot layouts, products, prices (replaces hardcoded CONFIG block)
- `inventory.json` on GitHub — visit data (existing)
- Hosted on GitHub Pages; installable as PWA on iPhone

## Architecture
- CONFIG moves from hardcoded JS to GitHub-synced `config.json` — pulled on load alongside `inventory.json`, pushed on save from PC admin
- Auto-detect mobile vs. desktop on load; render appropriate view
- Manual override toggle available on both views
- Knoxx approved `config.json` on GitHub as the config spine (2026-06-12); simultaneous-write risk noted — last-write-wins is acceptable for single-operator use; "Last synced" timestamp recommended in UI

## Constraints
- Single-file HTML, no external dependencies
- iPhone view: `100dvh`, safe-area insets, 44px tap targets, SF Pro font stack, dark mode
- GitHub credentials hardcoded in localStorage (PAT via first-run setup modal)
- Tab icons: inline SVG
- Slot numbers are NOT sequential — row A may have slots 1, 3, 5

## Views
**iPhone view** — restocking workflow: location/machine select, slot grid (snack) or product-count (drink), save/sync
**PC view** — admin + analytics: inventory dashboard, add/edit locations/machines/slot layouts, assign products and prices, cost/margin analytics, CSV export

## Team on this project
- **Tuxx** — UI design: all screens, slot grid, modal, tab bar, PC layout, admin panels, analytics views
- **Finn** — engineering: HTML/CSS/JS, localStorage, GitHub sync API, auto-detect, config.json migration
- **Anna** — filing (runs `/aaa` when needed)
- **Knoxx** — invoked by Hans before any significant architectural decision

## Out of scope
- PC dashboard as a separate file (it is part of `kv-machines.html`)
- Settings screen / PAT entry (PAT stored in localStorage via first-run modal)
- Automated access to RS&H governed data

## Key reference
Full project brief: `instructions/project_brief.md`
Source brief (user-supplied): `C:\Users\ronsi\OneDrive\Downloads\KellyVend_Project_Brief.md`

Run `/aaa` at any time to file loose files.
