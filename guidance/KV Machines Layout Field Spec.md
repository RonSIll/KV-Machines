# KV Machines — Layout Field Spec

**Author:** Finn  
**Date:** 2026-06-11  
**Status:** Draft — awaiting Hans / Ron approval before implementation

---

## Problem

The current CONFIG hardcodes a single layout shape (lettered rows, numbered slots) that only works for snack machines. Drink machines, candy machines, and future types have different physical configurations. The data model needs a type discriminator so the renderer can handle each shape correctly.

---

## Proposed Change

Add a `layoutType` string field to every machine object in CONFIG. The existing `layout` array stays — its structure varies by `layoutType`. The renderer dispatches on `layoutType` rather than assuming row-grid shape.

### Machine object shape

```javascript
{
  id: 'machine1',
  name: 'Machine 1',
  layoutType: 'row-grid',   // NEW — discriminator
  layout: [ /* type-specific, see below */ ]
}
```

`layoutType` is required. There is no default — an unknown or missing type renders an error state rather than silently breaking.

---

## Defined Layout Types

### `row-grid` (current snack machines)

Lettered rows, non-sequential numbered slots. Slot code = `row + slot` (e.g., `A3`).

```javascript
layoutType: 'row-grid',
layout: [
  { row: 'A', slots: [1, 3, 5] },
  { row: 'B', slots: [1, 3, 4, 6] },
  { row: 'C', slots: [1, 2, 3, 4, 5, 6] },
]
```

Slot count for the machines list: `layout.reduce((s, r) => s + r.slots.length, 0)`

Renderer: same as current `renderGrid()` — rows of slot buttons, code displayed as `A1`, `B3`, etc.

---

### `column-stack` (drink machines — cans in vertical columns)

Numbered columns, each holding one product. Slot code = column number as string (e.g., `"1"`, `"8"`). Optional `label` overrides the display code if the machine has named columns.

```javascript
layoutType: 'column-stack',
layout: [
  { col: 1 },
  { col: 2 },
  { col: 3 },
  { col: 4 },
  { col: 5 },
  { col: 6 },
  { col: 7 },
  { col: 8 },
]
```

Slot count: `layout.length`

Renderer: single horizontal row of tall slot buttons — one per column. Display code is the column number. The modal is identical (product name, price, before/after counts). Visual design for this renderer is Tuxx's call when the first drink machine is configured.

---

## Slot Key Compatibility

`sdKey()` concatenates `locId + '__' + machId + '__' + slotCode`. Slot codes are already strings for `row-grid` (`"A1"`, `"B3"`). For `column-stack`, column numbers are stringified (`String(col)`). No change to `sdKey()` needed.

---

## Renderer Dispatch Pattern

Replace the current `renderGrid(machine)` body with a dispatch:

```javascript
function renderGrid(machine) {
  if (machine.layoutType === 'row-grid')      renderGridRowGrid(machine);
  else if (machine.layoutType === 'column-stack') renderGridColumnStack(machine);
  else document.getElementById('machine-grid').innerHTML =
    `<div class="empty-state">Unknown layout type: ${machine.layoutType}</div>`;
}
```

Each render function builds the grid DOM independently. No shared grid-building logic is forced between types — they can diverge freely as new types are added.

---

## Slot Count in `selectLocation()`

Current code:
```javascript
const slotCount = machine.layout.reduce((s, r) => s + r.slots.length, 0);
```

This breaks for `column-stack`. Replace with a helper:

```javascript
function slotCount(machine) {
  if (machine.layoutType === 'row-grid')
    return machine.layout.reduce((s, r) => s + r.slots.length, 0);
  if (machine.layoutType === 'column-stack')
    return machine.layout.length;
  return 0;
}
```

---

## Adding Future Layout Types

1. Define the `layout` array shape and slot code convention in this document.
2. Add a `renderGrid{TypeName}(machine)` function.
3. Add a branch to `renderGrid()` dispatch.
4. Add a branch to `slotCount()`.

No other code touches layout structure.

---

## Backward Compatibility

Existing `kv-machines.html` has one machine using the `row-grid` shape without a `layoutType` field. When this spec is implemented, `layoutType: 'row-grid'` must be added to that machine's CONFIG entry. No `inventory.json` / `slotData` changes are needed — slot codes don't change.
