---
name: penpot-layer-order
description: Use when creating overlapping elements in Penpot via MCP — backgrounds behind text, cards with content, icons over images. Also use when a user reports elements are hidden, covered, or invisible after creation.
---

# Penpot Layer Order

## Overview

In Penpot, **z-order is determined by position in the parent's `children` array**:
- **Later index = visually on top** (foreground)
- **Earlier index = visually behind** (background)

Create backgrounds first, foreground content last. The Penpot MCP does not enforce this — agents must manage it explicitly.

## Critical: Use `insertChild`, Never `appendChild`

```js
// ✅ Correct — append to end (puts shape on top)
parent.insertChild(parent.children.length, shape)

// ❌ BROKEN — do not use
parent.appendChild(shape)
```

## Creation Order Rule

```js
// ✅ Correct: bg first → text last → text is on top
parent.insertChild(parent.children.length, bgRect)
parent.insertChild(parent.children.length, textShape)

// ❌ Wrong: text first → bg last → bg covers text
parent.insertChild(parent.children.length, textShape)
parent.insertChild(parent.children.length, bgRect)
```

## Fix Z-Order After Creation

If elements are already created in the wrong order, use:

```js
bgRect.sendToBack()      // move bg behind everything
textShape.bringToFront() // bring content to front
shape.setParentIndex(n)  // precise 0-based index control
```

Other available methods: `bringForward()`, `sendBackward()`

## Flex Layout Exception

For boards with `dir="column"` or `dir="row"`, the children array is **reversed** relative to visual order:

| Array position | Visual position |
|---|---|
| First (index 0) | Visually at the **end** (bottom/right) |
| Last | Visually at the **start** (top/left) |

For flex layout boards, use `board.appendChild(shape)` (the board method, not the broken general one) — it handles the reversal automatically. Call it in the order of visual appearance.

For absolute-positioned children inside a flex board (`child.layoutChild.absolute = true`), z-order follows the standard rule (later = on top).

## Checklist for Overlapping Designs

- [ ] Create background shapes first, foreground content last
- [ ] Use `insertChild(parent.children.length, shape)` — never `appendChild`
- [ ] If order is wrong after creation, call `sendToBack()` on backgrounds
- [ ] If inside a flex board, use `board.appendChild()` in visual appearance order

## Red Flags

- Text hidden after creation → bg was inserted after text; call `bgRect.sendToBack()`
- Element disappeared → likely covered; check children array order
- Used `appendChild` → this is broken, replace with `insertChild`
