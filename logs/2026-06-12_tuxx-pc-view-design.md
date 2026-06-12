# Tuxx PC View Design (WO-05)

## Date
2026-06-12

## Session
Tuxx PC View Design (WO-05)

## Conducted by
Hans

## Goal
Invoke Tuxx to execute WO-05 — produce the full PC view UI design spec for kv-machines.html.

## Summary
Tuxx executed WO-05 in full. The design spec covers all nine required decisions: sidebar nav layout, 1024px auto-detect breakpoint, view mode toggle placement on both iPhone and PC, two-panel master-detail admin layout, slot layout row editor, inline product/price table (PC) and unchanged slot modal (iPhone), drink machine product list config, dashboard with stat cards and visit history, analytics with by-product/by-machine tables and CSV export, and sync status in the PC header. Knoxx reviewed the spec before delivery and cleared it — two implementation notes added (debounce auto-save by 500ms; overflow:hidden wrapper on mini layout preview). Spec is delivered to Ron for approval; WO-06 to Finn gates on that approval.

## Decisions
- PC navigation: left sidebar, 200px fixed, three items — Dashboard / Admin / Analytics
- Auto-detect breakpoint: 1024px viewport width
- View toggle: "iPhone View" text button in PC header top-right; "Desktop" text link in iPhone Locations header top-right
- Admin layout: two-panel master-detail — 280px location/machine tree left, config form right
- Slot layout editor: row-by-row table (letter input + comma-separated slots + delete); + Add Row button; mini read-only preview with overflow:hidden container
- Product/price on PC: inline editable table, auto-save on blur with 500ms debounce; no separate Save button
- Product/price on iPhone: unchanged — existing slot modal handles it
- Drink machine config on PC: product name + price inline list with + Add Product
- Dashboard: 3 stat cards + read-only stock grid + visit history table
- Analytics: 3 stat cards + by-product and by-machine tables + horizontal bar chart + CSV export top-right
- Sync status: "Last synced HH:MM" in PC header, same token colors as iPhone sync indicator
- CSS strategy: all new `.pc-*` classes; no existing iPhone classes modified

## Work completed
- `C:/Users/ronsi/AI_Workshop/projects/KV Machines/guidance/pc-view-design-spec.md` — created (WO-05 deliverable)

## Next actions
- Ron: review and approve `guidance/pc-view-design-spec.md` — confirm design direction is correct
- Hans: on Ron's approval, release WO-06 to Finn
- Finn: after WO-06 released — config.json migration + full PC view build
- Ron: update CONFIG / config.json with real location names, machine names, and slot layouts when ready
- Ron: if app is reinstalled or localStorage is cleared, re-enter PAT via the setup prompt (PAT in `instructions/work_orders.json`)

## Open questions
- Is the PC view design direction correct? (WO-05 spec at `guidance/pc-view-design-spec.md` — Ron's approval gates WO-06)
