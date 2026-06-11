# KV Machines — UI Design Notes

**Prepared by:** Tuxx  
**Date:** 2026-06-11  
**For:** Finn (WO 2026-06-11-02)  
**Status:** Ready for build

The prototype structure is solid — this is a polish pass only. No navigation or layout changes. All markup below is drop-in ready.

---

## 1. Tab Bar Icons

Replace the `.tab-icon` content in both tab buttons. The SVGs use `currentColor` throughout, so the existing CSS (`.tab.active .tab-icon { color: var(--green); }` — see Section 5) drives active/inactive states automatically.

**Inventory tab** (2×2 grid — represents the slot grid):
```html
<svg class="tab-icon" width="22" height="22" viewBox="0 0 22 22" fill="none" xmlns="http://www.w3.org/2000/svg">
  <rect x="2" y="2" width="7" height="7" rx="1.5" fill="currentColor"/>
  <rect x="13" y="2" width="7" height="7" rx="1.5" fill="currentColor"/>
  <rect x="2" y="13" width="7" height="7" rx="1.5" fill="currentColor"/>
  <rect x="13" y="13" width="7" height="7" rx="1.5" fill="currentColor"/>
</svg>
```

**Performance tab** (3-bar ascending chart):
```html
<svg class="tab-icon" width="22" height="22" viewBox="0 0 22 22" fill="none" xmlns="http://www.w3.org/2000/svg">
  <rect x="2" y="13" width="4" height="7" rx="1.5" fill="currentColor"/>
  <rect x="9" y="8" width="4" height="12" rx="1.5" fill="currentColor"/>
  <rect x="16" y="3" width="4" height="17" rx="1.5" fill="currentColor"/>
</svg>
```

Remove the `<div class="tab-icon">` wrapper — move the class directly onto the `<svg>` element as shown above. Update `.tab-icon` CSS: remove `font-size: 22px`, add `display: block`.

---

## 2. Back Button

Replace `‹` with this SVG. The button color is already `var(--text)` via CSS — the SVG inherits it via `currentColor`.

```html
<svg width="14" height="24" viewBox="0 0 14 24" fill="none" xmlns="http://www.w3.org/2000/svg">
  <path d="M12 22L2 12L12 2" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/>
</svg>
```

Apply to all three back buttons (sub-machines, sub-grid, sub-perf-detail). Remove `font-size: 32px` from `.back-btn` — no longer needed.

**List chevrons** (`.list-chevron`): same treatment, rotated. Replace `›` with:
```html
<svg width="8" height="14" viewBox="0 0 8 14" fill="none" xmlns="http://www.w3.org/2000/svg">
  <path d="M1 1l6 6-6 6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
</svg>
```
Remove `font-size: 22px` from `.list-chevron`.

---

## 3. Color — Teal Alignment

**Decision:** Light mode shifts to KV Accounts teal for brand family consistency. Dark mode keeps iOS system green — it reads as native and is highly legible against dark backgrounds.

Update `:root` (light mode only):
```css
--green: #33808f;       /* was #1a9e6e — now matches KV Accounts --accent */
--green-bg: #e3f0f2;    /* was #e8f8f2 — now matches KV Accounts --accent-soft */
```

Dark mode block is unchanged: `--green: #30d158; --green-bg: #1a2e22;`

**Active tab color:** Update this rule so active tabs show the teal accent instead of black/white:
```css
/* change from: */
.tab.active .tab-icon, .tab.active .tab-label { color: var(--text); }
/* to: */
.tab.active .tab-icon, .tab.active .tab-label { color: var(--green); }
```

---

## 4. Primary Action Color — Save Button and Bar Chart Highlight

The Save button and filled bar chart currently use `var(--text)` (black/white). They should use the teal accent as the intentional "this matters" signal.

**Save button** (`.btn-save`):
```css
.btn-save { background: var(--green); color: #ffffff; }
```
Remove `color: var(--bg)` — always white is correct since teal is legible in both modes.

**Bar fill highlight** (`.bar-fill.highlight`): already uses `var(--green)` — no change needed.

---

## 5. Slot Grid — Minor Legibility Bump

One change only: bump `.slot-name` from 10px to 11px. Ron reads this standing in front of a machine; the extra pixel matters.

```css
.slot-name { font-size: 11px; /* was 10px */ ... }
```

Everything else in the grid (slot code at 20px, filled state, border treatment) is correct — leave it.

---

## 6. Performance Highlights — Replace Emoji

The JS-generated badge strings use emoji. Replace them in `renderPerformance()`:

```javascript
/* change from: */
`<div class="highlight-badge">🏆 Top</div>`
`<div class="highlight-badge">⚠️ Lowest</div>`

/* to: */
`<div class="highlight-badge">Top Seller</div>`
`<div class="highlight-badge">Lowest</div>`
```

The `.highlight-badge` CSS already handles color (`color: var(--green)` / `color: var(--red)`) and uppercase treatment — the text reads clearly without emoji.

---

## 7. Empty State — Performance Tab

Replace the current `.empty-state` string in `renderPerformance()`:

```javascript
/* change from: */
pEl.innerHTML = '<div class="empty-state">Log some visits to see performance.</div>';

/* to: */
pEl.innerHTML = `<div class="empty-state">
  <svg width="40" height="40" viewBox="0 0 22 22" fill="none" style="display:block;margin:0 auto 12px;color:var(--text3)">
    <rect x="2" y="13" width="4" height="7" rx="1.5" fill="currentColor"/>
    <rect x="9" y="8" width="4" height="12" rx="1.5" fill="currentColor"/>
    <rect x="16" y="3" width="4" height="17" rx="1.5" fill="currentColor"/>
  </svg>
  Log some visits to see performance.
</div>`;
```

The icon is the same Performance tab icon, dimmed to `--text3`. No new CSS needed.

---

## 8. App Wordmark

**Decision: none.** The Home Screen icon and label handle app identity. Adding a wordmark inside the app chrome adds clutter with no information gain. The large screen titles ("Locations", "Performance") already orient the user.

---

## Summary for Finn

| Item | File location | Change type |
|---|---|---|
| Tab icons | HTML — both `.tab` buttons | Replace unicode div with SVG; update `.tab-icon` CSS |
| Back buttons | HTML — 3 locations | Replace `‹` with SVG; remove `font-size` from `.back-btn` |
| List chevrons | JS — `renderLocations()`, `selectLocation()` | Replace `›` with SVG; remove `font-size` from `.list-chevron` |
| Teal color | CSS `:root` light mode only | `--green` and `--green-bg` values |
| Active tab color | CSS `.tab.active` rule | `color: var(--green)` |
| Save button | CSS `.btn-save` | `background: var(--green); color: #ffffff` |
| Slot name size | CSS `.slot-name` | `font-size: 11px` |
| Performance badges | JS `renderPerformance()` | Remove emoji, plain text |
| Empty state | JS `renderPerformance()` | Add icon + updated text |
