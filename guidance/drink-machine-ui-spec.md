# Drink Machine Product-Count Screen — Design Spec

**Author:** Tuxx  
**Date:** 2026-06-11  
**Work Order:** WO-2026-06-11-03

---

## Context

The Drink Machine at Ready Containment has no slot grid. Ron records only qty added per drink type per visit. This spec covers:
1. A new `sub-drink` subscreen for product-count entry
2. Performance tab treatment for drink machine data

---

## Screen: `sub-drink`

Structurally mirrors `sub-grid` — same header and sync-status pattern, different body.

**Header**
- Back chevron SVG (existing style) + machine name as `.screen-title`
- `.sync-status` below header (same as sub-grid)

**Product rows** — `flex-direction: column; gap: 8px`

Each row:
```
[ Product Name           ] [  −   0   +  ]
```

- Container: `background: var(--bg2); border-radius: 14px; padding: 14px 16px; display: flex; justify-content: space-between; align-items: center;`
- Product name: `font-size: 17px; font-weight: 600; color: var(--text);`
- Stepper:
  - `−` and `+` buttons: `44 × 44px` tap target, `background: var(--bg3); border-radius: 50%; border: none; font-size: 22px; color: var(--text);`
  - Count display between them: `font-size: 28px; font-weight: 700; color: var(--text); min-width: 44px; text-align: center;`
  - Stepper container: `display: flex; align-items: center; gap: 8px;`

All 8 products listed in column order: A&W · Dr Pepper · Sunkist · Sprite · Mtn Dew · Coke · Brisk Tea · Coke Zero

**Zero state**: count starts at 0. Stepper is self-explanatory — no additional empty-state copy needed.

**Save Visit button** (after product list, 16px margin-top)
- Reuses `.btn-save` exactly: full-width, green background, "Save Visit" label
- Bottom padding: `calc(16px + var(--safe-bottom))` to clear iPhone home indicator

---

## Data model (for Finn)

On "Save Visit", push one visit record per product where qty_added > 0:

```js
{
  slotKey: locId + '__' + machId + '__' + productId,
  locationId, machineId, locationName, machineName,
  slot: productId,
  name: productName,
  price: null,
  sold: qty_added,   // "added" mapped to "sold" for performance tab compatibility
  revenue: 0,
  ts: Date.now()
}
```

This keeps the existing Performance tab aggregation working without modification. Drink products appear alongside snack products — `sold` means "cans added," which is the useful metric for drinks anyway.

---

## Performance tab

No layout changes. Drink products appear in "All Products" with:
- Sold count = qty added
- Revenue = $0.00 (expected — no price tracking for drinks)
- Eligible for "Top Seller" / "Lowest" highlights

---

## CSS additions (minimal)

Two new classes only — all values from existing tokens:
- `.drink-product-row` — the row container described above
- `.stepper` — the `display:flex; align-items:center; gap:8px;` wrapper around the three stepper elements

No new color tokens, no new spacing values. Everything else reuses existing classes.

---

## What does NOT change

- Snack machine slot grid — unchanged
- Performance tab structure — unchanged
- All existing CSS classes — unchanged
