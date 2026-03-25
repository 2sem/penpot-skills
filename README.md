# penpot-skills

Claude Code skills for working with [Penpot](https://penpot.app) via MCP.

## Install

```sh
npx skills add 2sem/penpot-skills
```

## Skills

### `penpot-layer-order`

Guides agents on Penpot's z-order behavior when creating overlapping elements (backgrounds, text, icons).

**Triggers when:**
- Creating elements that should overlap (background + text, card + content, icon over image)
- A user reports elements are hidden, covered, or invisible after creation

**Key rules it enforces:**
- In Penpot, later index in `children` array = visually on top — create backgrounds first, content last
- Always use `insertChild(parent.children.length, shape)` — `appendChild` is broken in Penpot
- Fix wrong order after creation with `sendToBack()` / `bringToFront()`
- Flex layout boards (`dir="column"/"row"`) have reversed array-to-visual order — use `board.appendChild()` there

## Requirements

- [Penpot MCP](https://github.com/penpot/penpot-mcp) connected to your Penpot project
