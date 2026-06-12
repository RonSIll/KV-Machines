# KV Machines — PC View Design Spec

**Author:** Tuxx
**Date:** 2026-06-12
**Work Order:** WO-2026-06-12-05
**Status:** Ready for Hans review → Finn build

---

## Overview

The PC view is a distinct admin environment that shares the same HTML file and CSS token system as the iPhone view. It is not a scaled-up version of the iPhone — it is a different layout optimised for keyboard-and-mouse use at a desk. Three sections: **Dashboard**, **Admin**, **Analytics**.

Auto-detect breakpoint: **1024px viewport width**. Below 1024px → iPhone view. At or above 1024px → PC view. Persist manual override in localStorage.

---

## 1. PC App Shell

```
┌─────────────────────────────────────────────────────────────┐
│ HEADER (60px, border-bottom: 0.5px var(--border))           │
│ [KV Machines wordmark, 18px 700]     [Last synced 14:32] [→ iPhone View] │
├──────────────┬──────────────────────────────────────────────┤
│              │                                              │
│  SIDEBAR     │  CONTENT AREA                               │
│  200px fixed │  scrollable, flex: 1                        │
│              │                                              │
│  ● Dashboard │                                              │
│    Admin     │                                              │
│    Analytics │                                              │
│              │                                              │
└──────────────┴──────────────────────────────────────────────┘
```

**Header:**
- Left: "KV Machines" wordmark — `font-size: 18px; font-weight: 700; color: var(--text); letter-spacing: -0.3px`
- Right group (flex, gap 16px, align-items center): sync status text + "iPhone View" toggle
- Height: 60px; `padding: 0 24px`; `border-bottom: 0.5px solid var(--border)`; `background: var(--bg)`

**Sidebar:**
- Width: 200px; fixed height; `border-right: 0.5px solid var(--border)`; `background: var(--bg)`; `padding: 16px 12px`
- Nav items: `border-radius: 10px; padding: 10px 12px; font-size: 15px; font-weight: 500; cursor: pointer`
- Inactive: `color: var(--text2); background: none`
- Active: `color: var(--green); background: var(--green-bg); font-weight: 600`
- Each nav item has a small inline SVG icon (16×16) + label, side by side

**Content area:**
- `flex: 1; overflow-y: auto; padding: 28px 32px`
- `max-width: none` — fills available width

**PC root container:**
- `display: flex; flex-direction: column; height: 100vh` (not `100dvh` — PC doesn't need dynamic viewport)
- Inner row: `display: flex; flex: 1; overflow: hidden`

---

## 2. Sidebar Nav Items and Icons

**Dashboard** (grid-2×2, reuses existing tab icon):
```html
<svg width="16" height="16" viewBox="0 0 22 22" fill="none">
  <rect x="2" y="2" width="7" height="7" rx="1.5" fill="currentColor"/>
  <rect x="13" y="2" width="7" height="7" rx="1.5" fill="currentColor"/>
  <rect x="2" y="13" width="7" height="7" rx="1.5" fill="currentColor"/>
  <rect x="13" y="13" width="7" height="7" rx="1.5" fill="currentColor"/>
</svg>
```

**Admin** (wrench/settings — sliders icon):
```html
<svg width="16" height="16" viewBox="0 0 22 22" fill="none">
  <line x1="4" y1="6" x2="18" y2="6" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
  <line x1="4" y1="11" x2="18" y2="11" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
  <line x1="4" y1="16" x2="18" y2="16" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
  <circle cx="8" cy="6" r="2.5" fill="var(--bg)" stroke="currentColor" stroke-width="2"/>
  <circle cx="14" cy="11" r="2.5" fill="var(--bg)" stroke="currentColor" stroke-width="2"/>
  <circle cx="8" cy="16" r="2.5" fill="var(--bg)" stroke="currentColor" stroke-width="2"/>
</svg>
```

**Analytics** (bar chart, reuses existing Performance icon):
```html
<svg width="16" height="16" viewBox="0 0 22 22" fill="none">
  <rect x="2" y="13" width="4" height="7" rx="1.5" fill="currentColor"/>
  <rect x="9" y="8" width="4" height="12" rx="1.5" fill="currentColor"/>
  <rect x="16" y="3" width="4" height="17" rx="1.5" fill="currentColor"/>
</svg>
```

---

## 3. View Mode Toggle

**PC → iPhone:** Small text button in the header, far right. Label: "iPhone View". Style: `font-size: 13px; font-weight: 500; color: var(--text2); background: var(--bg2); border: none; border-radius: 8px; padding: 6px 12px; cursor: pointer`. On hover: `color: var(--text)`. No icon needed — text is clear.

**iPhone → PC:** Small text link in the `sub-locations` header, top-right. Position: absolutely placed at the top-right of the `.screen-header` row alongside the existing "Locations" title. Label: "Desktop". Style: `font-size: 13px; color: var(--text3); background: none; border: none; cursor: pointer`. On tap: `color: var(--text2)`. Low-friction, non-intrusive — users who never need it won't notice it.

Implementation note for Finn: add `position: relative` to the `.screen-header` on `sub-locations`; absolutely position the Desktop button at `top: 50%; right: 14px; transform: translateY(-50%)`. The existing title is already `flex: 1` so it won't crowd.

---

## 4. Sync Status — PC Placement

Position: top header, right side, left of the "iPhone View" toggle.

```
[Last synced 14:32]   [→ iPhone View]
```

Style:
- Normal (after successful sync): `font-size: 12px; color: var(--text3)`
- On sync in progress: "Syncing…" in `var(--text3)`
- On success (brief flash): "Synced ✓" in `var(--green)` — revert to "Last synced HH:MM" after 2s
- On error: "Sync failed" in `var(--red)`

Display format after success: "Last synced HH:MM" using the local time from `new Date().toLocaleTimeString([], {hour:'2-digit', minute:'2-digit'})`.

The existing iPhone sync status elements (`#sync-status`, `#sync-status-drink`) remain unchanged — they only render on the iPhone screens.

---

## 5. Admin Section — Location/Machine Hierarchy

Two-panel master-detail layout within the content area:

```
┌──────────────────────┬──────────────────────────────────────┐
│ LOCATIONS PANEL      │ DETAIL PANEL                         │
│ 280px                │ remaining width                      │
│                      │                                      │
│ + Add Location       │ [Machine name edit]                  │
│ ─────────────────    │ Type: Snack / Drink                  │
│ Ready Containment ›  │ ─── Slot Layout ───                  │
│                      │ [row editor]                         │
│                      │                                      │
│                      │ ─── Products & Prices ───            │
│                      │ [inline table]                       │
└──────────────────────┴──────────────────────────────────────┘
```

**Locations panel:**
- `width: 280px; border-right: 0.5px solid var(--border); padding: 0 16px`
- "+ Add Location" button at top: `font-size: 14px; font-weight: 600; color: var(--green); background: none; border: none; cursor: pointer; padding: 0 0 12px`
- Each location row: `padding: 12px 0; border-bottom: 0.5px solid var(--border); cursor: pointer; font-size: 15px; font-weight: 600; color: var(--text)`. Selected state: `color: var(--green)`.
- On location row hover: reveal inline "Edit name" and "Delete" text links (12px, `var(--text3)`, `float: right`). Edit opens a small inline text input in place of the name.
- Location expanded → shows machines as indented rows below the location name: `padding-left: 16px; font-size: 14px; font-weight: 500; color: var(--text2)`. Selected machine: `color: var(--green)`.
- "+ Add Machine" appears at the bottom of the expanded machine list (same teal text link style)

**Detail panel:** renders the selected machine's configuration. When nothing is selected: `<div class="empty-state">Select a machine to configure it.</div>` centered in the panel.

---

## 6. Admin — Machine Detail Form

Top of detail panel: machine name as an editable `<input>` (not a static title). Style matches existing `.field input` but wider: `font-size: 22px; font-weight: 700; border: none; border-bottom: 2px solid var(--border2); border-radius: 0; padding: 4px 0; background: transparent; width: 100%; margin-bottom: 20px`. On focus: `border-bottom-color: var(--green)`.

Below the name: **Type selector** — two segmented buttons: "Snack" and "Drink". Style: `background: var(--bg2); border-radius: 10px; display: inline-flex; padding: 3px`. Each option: `padding: 6px 20px; border-radius: 8px; font-size: 14px; font-weight: 600; border: none; cursor: pointer`. Active: `background: var(--bg); color: var(--text); box-shadow: 0 1px 3px rgba(0,0,0,0.1)`. Inactive: `color: var(--text2); background: transparent`.

**Auto-save:** All admin changes auto-save and push `config.json` on blur/change — no explicit "Save" button for individual fields. A small inline "Saved" flash (green, 1.5s) confirms each push. Debounce the push by 500ms to prevent stacking API calls during rapid edits.

---

## 7. Admin — Slot Layout Editor (Snack Machine)

Appears below the type selector when type = Snack.

Section title: "Slot Layout" (same style as `.detail-section-title` in existing CSS)

Row table — each row in the layout:
```
[A]  [1, 3, 5          ]  [×]
[B]  [1, 3, 4, 6       ]  [×]
[C]  [1, 2, 3, 4, 5, 6 ]  [×]
     + Add Row
```

- Row letter input: `width: 36px; text-align: center; font-size: 15px; font-weight: 700; text-transform: uppercase`
- Slot numbers input: free-text, comma-separated (e.g. `1, 3, 5`). `flex: 1; font-size: 14px`. Finn parses on save: split by comma, trim, convert to integers, filter NaN. Width `100%` of remaining space.
- Delete row button [×]: `color: var(--text3); background: none; border: none; cursor: pointer; font-size: 16px`. On hover: `color: var(--red)`.
- "+ Add Row" link: teal text button below the rows.
- Container: `display: flex; flex-direction: column; gap: 6px; margin-bottom: 20px`
- Each row: `display: flex; align-items: center; gap: 8px`

**Mini layout preview:** Below the row editor, render a read-only mini slot grid (non-interactive): `transform: scale(0.6); transform-origin: top left` — same visual as the iPhone slot grid. Updates live as rows/slots change. Helps Ron confirm the shape before saving. Wrap in a container with `overflow: hidden` and explicit height (`height: calc(naturalHeight * 0.6)`) to prevent bleed into adjacent content.

---

## 8. Admin — Product & Price Assignment (Snack Machine)

Appears below the slot layout editor for Snack machines. Section title: "Products & Prices"

Inline table with all slots:

```
SLOT    PRODUCT              PRICE
A1      Doritos Nacho        $ 1.50
A3      Cheez-Its            $ 1.25
A5      —                    —
B1      …
```

- Table: `width: 100%; border-collapse: collapse`
- Header row: `.detail-section-title` style on TH cells
- Each data row: `border-bottom: 0.5px solid var(--border)`; row height ~48px
- Slot column (60px): `font-size: 15px; font-weight: 700; color: var(--text)`
- Product column: click to edit inline — shows `<input type="text">` on click, blurs to save. Empty slots show "—" in `var(--text3)`.
- Price column (100px): same click-to-edit. Shows formatted `$ 1.50` at rest; becomes `<input type="number" step="0.25">` on click.
- No separate Save button — auto-saves (same pattern as machine name).

**iPhone context for this same data:** No change to the iPhone slot modal. The modal already shows Product Name and Sale Price fields. Ron edits these in the field on his phone. Both paths write to the same `slotData` key → `config.json`.

---

## 9. Admin — Drink Machine Config

When machine type = Drink, the slot layout editor is hidden. Replace with: "Products" section.

Simple vertical list — each row:
```
[A&W               ]  [$ 1.50]  [×]
[Dr Pepper         ]  [$ 1.50]  [×]
…
+ Add Product
```

- Product name input: `flex: 1; font-size: 15px`
- Price input: `width: 80px; font-size: 15px; text-align: right`
- Delete: same [×] treatment as slot row editor
- "+ Add Product": teal text button below list
- Inputs auto-save on blur

---

## 10. Dashboard Section

Three summary stat cards across the top:

```
┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐
│ Total Locations  │  │ Total Machines   │  │ Total Visits     │
│       1          │  │       2          │  │      14          │
└──────────────────┘  └──────────────────┘  └──────────────────┘
```

Card style: `background: var(--bg2); border-radius: 14px; padding: 20px 24px`. Label: `font-size: 12px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.06em; color: var(--text2); margin-bottom: 8px`. Value: `font-size: 36px; font-weight: 700; color: var(--text); letter-spacing: -1px`. Cards in a `display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; margin-bottom: 28px`.

**Stock levels grid:** Below the stat cards. Section title "Current Stock". One card per machine; each card shows the machine name and a miniature slot grid (same read-only preview as the admin mini preview above). Filled slots shown in their filled state; empty slots shown empty. Cards in a `display: flex; flex-wrap: wrap; gap: 16px`.

**Visit history table:** Section title "Visit History". Sortable table — default sort: newest first.

Columns: Date | Location | Machine | Product | Sold | Revenue

Table style: `width: 100%; border-collapse: collapse`. TH: `.detail-section-title` style. Each TD: `padding: 10px 8px; border-bottom: 0.5px solid var(--border); font-size: 14px; color: var(--text)`. Date column: `color: var(--text2)`. Revenue column: right-aligned. Empty state (no visits): small centered message with the Performance empty-state icon.

---

## 11. Analytics Section

**Section header row:** "Analytics" H1 (same `.screen-title` style) left + "Export CSV" button right.

Export CSV button: `font-size: 14px; font-weight: 600; color: var(--green); background: var(--green-bg); border: none; border-radius: 10px; padding: 8px 16px; cursor: pointer`.

**Summary row:** same 3-card pattern as Dashboard, but values: Total Revenue | Total Sold (units) | Avg Revenue/Visit. Same card style.

**By-Product table:** Section title "By Product". Columns: Product | Machines | Total Sold | Revenue | Avg Price. Sortable by header click. The top product row gets `color: var(--green)` on its name.

**By-Machine table:** Section title "By Machine". Columns: Machine | Location | Visits | Units Sold | Revenue. Same style.

Both tables: `width: 100%; border-collapse: collapse; margin-bottom: 32px`. Alternating row background: `var(--bg)` / `var(--bg2)` on every other row (`nth-child(even)`).

**Bar chart:** Below the By-Product table, render a horizontal bar chart of total sold per product — same `.compare-bar-row` / `.bar-fill` / `.bar-track` pattern from the existing iPhone product detail view. Top product gets `.highlight` class (teal fill). Labels left, values right, bars filling the remaining width.

---

## 12. CSS additions for PC view

All new classes are prefixed `pc-` to avoid collision with existing iPhone classes. No existing classes are modified. New classes added to the `<style>` block after the existing mobile CSS.

Key new classes Finn will need:

| Class | Purpose |
|-------|---------|
| `.pc-app` | Root PC shell: `display:flex; flex-direction:column; height:100vh` |
| `.pc-header` | Top header bar |
| `.pc-wordmark` | App name in header |
| `.pc-view-toggle` | "iPhone View" button |
| `.pc-body` | Row container below header: sidebar + content |
| `.pc-sidebar` | Left nav panel |
| `.pc-nav-item` | Sidebar nav button (inactive) |
| `.pc-nav-item.active` | Active state |
| `.pc-content` | Scrollable right panel |
| `.pc-section` | Each of the 3 main content areas |
| `.pc-stat-grid` | 3-column summary card grid |
| `.pc-stat-card` | Individual stat card |
| `.pc-admin-panels` | Two-panel master-detail container |
| `.pc-locations-panel` | Left admin tree panel |
| `.pc-detail-panel` | Right admin detail panel |
| `.pc-table` | Shared table style for dashboard/analytics |
| `.pc-slot-editor` | Row editor for slot layout |
| `.pc-machine-name-input` | Editable machine name field |
| `.pc-type-switcher` | Snack/Drink segmented control |
| `.pc-layout-preview` | Mini read-only slot grid |

Dark mode: all PC classes use the same CSS variable tokens already in `:root` — no new dark-mode overrides needed.

---

## Summary for Finn

| Decision | Choice |
|----------|--------|
| Breakpoint | 1024px |
| PC nav | Sidebar, 200px, 3 items |
| View toggle — PC | Header top-right, text button "iPhone View" |
| View toggle — iPhone | Locations header top-right, text link "Desktop" |
| Sync status — PC | Header right, "Last synced HH:MM" |
| Admin layout | Two-panel master-detail (280px tree + detail) |
| Slot layout editor | Row-by-row table: letter input + comma-separated slots + delete; + Add Row; mini preview |
| Product/price — PC | Inline editable table, auto-save on blur |
| Product/price — iPhone | Unchanged (existing slot modal) |
| Drink machine config | Product list with name + price inline inputs, + Add Product |
| Dashboard | 3 stat cards + stock level machine cards + visit history table |
| Analytics | 3 stat cards + by-product table + by-machine table + bar chart + CSV export |
| Auto-save | Admin fields auto-save on blur → push config.json; small "Saved" flash confirmation |
| CSS strategy | All new `.pc-*` classes; no existing classes modified |
