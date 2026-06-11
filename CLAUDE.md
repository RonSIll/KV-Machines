# CLAUDE.md — KV Machines

Mobile-first iPhone PWA for KellyVend LLC vending machine restocking and inventory tracking.

## next_actions
- Finn: execute WO 2026-06-11-02 (build pass) — branding, SVG icons per Tuxx's spec, PWA manifest, GitHub sync, config cleanup
- Hans: review Finn's output, verify against WO-02 definition of done, then /sss and push

## Summary
Ron visits vending machines, counts inventory before and after restocking, and tracks what sold. The app is a single `kv-machines.html` file hosted on GitHub Pages and installed on iPhone Home Screen as a PWA.

## Goal
Polished, shippable `kv-machines.html` with GitHub sync — push `inventory.json` on every save, pull on open.

## Deliverables
- `kv-machines.html` (single self-contained file) → `src/`
- Hosted on GitHub Pages; installable as PWA on iPhone

## Constraints
- Single-file HTML, no external dependencies
- iPhone-native feel: `100dvh`, safe-area insets, 44px tap targets, SF Pro font stack, dark mode
- GitHub credentials hardcoded (no Settings screen)
- Tab icons: inline SVG
- Slot numbers are NOT sequential — row A may have slots 1, 3, 5

## Team on this project
- **Tuxx** — UI design: all screens, slot grid, modal, tab bar, performance bar charts
- **Finn** — engineering: HTML/CSS/JS, localStorage, GitHub sync API
- **Anna** — filing (runs `/aaa` when needed)
- **Knoxx** — invoked by Hans before any significant architectural decision

## Out of scope
- PC dashboard (future phase)
- Settings screen / PAT entry
- Cost-per-unit tracking
- Adding locations or machines from the phone

## Key reference
Full project brief: `instructions/project_brief.md`
Source brief (user-supplied): `C:\Users\ronsi\OneDrive\Downloads\KellyVend_Project_Brief.md`

Run `/aaa` at any time to file loose files.
