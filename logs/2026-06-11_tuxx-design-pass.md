# Tuxx Design Pass

## Date
2026-06-11

## Session
Tuxx Design Pass

## Conducted by
Hans

## Goal
Invoke Tuxx to execute WO 2026-06-11-01 — UI polish pass on the prototype delivering drop-in SVG markup, color decisions, and annotated guidance for Finn.

## Summary
Tuxx reviewed the prototype at src/kv-machines.html and the KV Accounts visual reference, then delivered a complete design guidance document. All decisions were made and documented with exact markup and CSS changes ready for Finn. No HTML mockup was produced — guidance document is the deliverable per the work order spec.

## Decisions
- Tab icons: 2×2 grid squares (Inventory) and 3-bar ascending chart (Performance) — inline SVG with currentColor
- Back button and list chevrons: stroke SVG chevrons replacing all unicode characters
- Color: light mode shifts to #33808f teal (KV Accounts brand alignment); dark mode keeps #30d158 (iOS system green, native feel)
- Active tab indicator: color changes from black/white (var(--text)) to teal (var(--green)) — more intentional
- Save button: teal fill (var(--green)) replacing black/white — consistent primary action signal
- Slot name font size: 11px (up from 10px) for arm's-length legibility
- Performance highlight badges: plain text ("Top Seller" / "Lowest") — no emoji
- Empty state: dimmed bar chart SVG icon above text — reuses Performance tab icon
- App wordmark: none — Home Screen icon/label handles identity; in-app wordmark would add clutter

## Work completed
- `C:/Users/ronsi/AI_Workshop/projects/KV Machines/guidance/KV Machines UI Design Notes.md` — full design spec with drop-in SVG markup, CSS diffs, JS changes, and summary table for Finn

## Next actions
- Ron: review guidance/KV Machines UI Design Notes.md and approve (or redirect)
- Hans: on Ron's approval, mark WO 2026-06-11-01 complete and release WO 2026-06-11-02 to Finn
- Finn: execute WO 2026-06-11-02 — branding, SVG icons, PWA manifest, GitHub sync, config cleanup

## Open questions
- Ron has not yet reviewed or approved Tuxx's design notes — Finn is gated until approval
