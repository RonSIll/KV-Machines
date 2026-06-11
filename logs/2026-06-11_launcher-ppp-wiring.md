# Launcher /ppp Wiring

## Date
2026-06-11

## Session
Launcher /ppp Wiring

## Conducted by
Hans

## Goal
Diagnose why a fresh session had no context from prior sessions, and fix the root cause workshop-wide.

## Summary
Ron noticed that opening a session via the launcher left Claude with no memory of prior work — only the CLAUDE.md auto-loaded, not the work orders or session logs. Diagnosed the gap: session launchers only set the working directory; /ppp is required to load instructions and the most recent log, but was never wired into the launchers. Updated Anna's launcher spec and all six project session launchers to auto-run `/ppp {Project Name}` on start. _Global Session.bat left unchanged (no single project context). No KV Machines build work occurred this session.

## Decisions
- All project session launchers will auto-run `/ppp {Project Name}` as the initial Claude prompt on launch
- _Global Session.bat is exempt — it opens at the workshop root with no single project context
- Anna's `sessions_folder.launcher_format` spec updated to document /ppp as a required element of all new launchers

## Work completed
- `C:/Users/ronsi/AI_Workshop/Team Members/anna.md` — launcher_format updated to require /ppp auto-run
- `C:/Users/ronsi/AI_Workshop/Sessions/Sunday School Session.bat` — added /ppp auto-run
- `C:/Users/ronsi/AI_Workshop/Sessions/KV Machines Session.bat` — added /ppp auto-run
- `C:/Users/ronsi/AI_Workshop/Sessions/Communion Sermon Session.bat` — added /ppp auto-run
- `C:/Users/ronsi/AI_Workshop/Sessions/MD Reader App Session.bat` — added /ppp auto-run
- `C:/Users/ronsi/AI_Workshop/Sessions/KV Accounts Session.bat` — added /ppp auto-run
- `C:/Users/ronsi/AI_Workshop/Sessions/Auto Insurance Session.bat` — added /ppp auto-run

## Next actions
- Ron: review Tuxx's design notes at `guidance/KV Machines UI Design Notes.md` and approve (or redirect)
- Hans: on Ron's approval, mark WO 2026-06-11-01 complete and release WO 2026-06-11-02 to Finn
- Finn: execute WO 2026-06-11-02 (build pass) — branding, SVG icons per Tuxx's spec, PWA manifest, GitHub sync, config cleanup

## Open questions
- None
