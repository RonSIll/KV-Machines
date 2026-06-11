# Work Orders and GitHub Setup

## Date
2026-06-11

## Session
Work Orders and GitHub Setup

## Conducted by
Hans

## Goal
Review the prototype, create work orders for Tuxx and Finn, and complete GitHub infrastructure setup so the app has a deploy target.

## Summary
Prototype (`kellyvend.html`, user-supplied) was read, assessed, and filed to `src/kv-machines.html`. All project references were updated from "KellyVend" to "KV Machines" / "kv-machines". Work orders were created for Tuxx (UI design pass) and Finn (build pass). GitHub infrastructure was completed live with Ron: repo KV-Machines created under RonSill, GitHub Pages enabled on main, and a fine-grained PAT (Contents R/W, KV-Machines only, expires 2027-06-11) generated and stored in Finn's work order.

## Decisions
- App name is "KV Machines"; HTML filename is `kv-machines.html`; GitHub repo is `KV-Machines`
- Tab bar icons: inline SVG — best iPhone experience (Retina-sharp, currentColor theming, no external dependency); unicode glyphs rejected
- KV Accounts visual palette (`#33808f` teal, `#eef1f4` bg, `#1f2933` ink) to carry over to the future PC dashboard component for visual family consistency
- Finn's build is gated on Tuxx's approved design (WO 2026-06-11-01 must close before WO 2026-06-11-02 begins icon/polish work)
- PAT hardcoded in HTML is an accepted tradeoff — risk is low since it is scoped to one repo with Contents only

## Work completed
- `C:/Users/ronsi/AI_Workshop/projects/KV Machines/src/kv-machines.html` — prototype filed from Downloads
- `C:/Users/ronsi/AI_Workshop/projects/KV Machines/instructions/project_brief.md` — renamed to KV Machines, filename/repo references updated
- `C:/Users/ronsi/AI_Workshop/projects/KV Machines/instructions/work_orders.json` — WO 2026-06-11-01 (Tuxx design) and WO 2026-06-11-02 (Finn build) created; GitHub credentials added to Finn's order
- `C:/Users/ronsi/AI_Workshop/projects/KV Machines/CLAUDE.md` — next_actions updated; all kellyvend references replaced
- GitHub repo RonSill/KV-Machines created (public), Pages enabled on main branch
- PAT `github_pat_11CA33B5A0…` generated, scoped to KV-Machines Contents R/W, expires 2027-06-11

## Next actions
- Tuxx: execute WO 2026-06-11-01 (UI design pass — SVG icons, KV brand palette, modal/grid polish)
- Hans: review Tuxx's design output, approve, then release WO 2026-06-11-02 to Finn
- Finn: execute WO 2026-06-11-02 after Tuxx design approval — branding, SVG icons, PWA manifest, GitHub sync, config cleanup

## Open questions
- None — all GitHub credentials are stored; work orders are ready; prototype is in src/
