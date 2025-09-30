# `@dnd-kit/modifiers` – Documentation

> **@dnd-kit/modifiers** is a lightweight package that provides a set of reusable modifiers to control the movement of draggable items in the `dnd‑kit` ecosystem.  
> Modifiers are functions that receive the current drag state and return a modified state, allowing you to constrain, snap, or otherwise transform drag coordinates.

---

## Table of Contents

1. [Installation](#installation)
2. [Quick Start](#quick-start)
3. [API Reference](#api-reference)
   - [Built‑in Modifiers](#built‑in-modifiers)
     * [Restrict movement to a single axis](#restrict-movement-to-a-single-axis)
     * [Restrict movement to a bounding rectangle](#restrict-movement-to-a-bounding-rectangle)
     * [Snap to a grid](#snap-to-a-grid)
   - [Creating Custom Modifiers](#creating-custom-modifiers)
4. [Examples](#examples)

---

## Installation

```bash
# npm
npm install @dnd-kit/modifiers

# yarn
yarn add @dnd-kit/modifiers
```

---

## Quick Start

```tsx
import React from "react";
import { DndContext, DragOverlay } from "@dnd-kit/core";
import {
  restrictToVerticalAxis,
  restrictToWindowEdges,
} from "@dnd-kit/modifiers";

export default function App() {
  return (
    <DndContext modifiers={[restrictToVerticalAxis]}>
      {/* Your draggable items go here */}
      <DragOverlay modifiers={[restrictToWindowEdges]}>
        {/* Overlay content */}
      </DragOverlay>
    </DndContext>
  );
}
```

> **Note**  
> `DndContext` and `DragOverlay` can each receive their own set of modifiers – they do not share a single modifier list.

---

## API Reference

### Built‑in Modifiers

| Modifier | Purpose | Example Usage |
|---|---|---|
| `restrictToHorizontalAxis` | Constrain movement to the X‑axis only. | `modifiers={[restrictToHorizontalAxis]}` |
| `restrictToVerticalAxis` | Constrain movement to the Y‑axis only. | `modifiers={[restrictToVerticalAxis]}` |
| `restrictToWindowEdges` | Prevent the drag from moving outside the viewport. | `modifiers={[restrictToWindowEdges]}` |
| `restrictToParentElement` | Restrict movement to the parent element of the draggable item. | `modifiers={[restrictToParentElement]}` |
| `restrictToFirstScrollableAncestor` | Restrict movement to the first scrollable ancestor of the draggable item. | `modifiers={[restrictToFirstScrollableAncestor]}` |
| `createSnapModifier(gridSize)` | Factory that returns a modifier snapping to a grid of *gridSize* pixels. | `const snapToGrid = createSnapModifier(20);` |

#### Restrict movement to a single axis

```tsx
import { restrictToHorizontalAxis } from "@dnd-kit/modifiers";

<DndContext modifiers={[restrictToHorizontalAxis]}>
  {/* draggable items */}
</DndContext>
```

#### Restrict movement to a bounding rectangle

```tsx
import { restrictToWindowEdges } from "@dnd-kit/modifiers";

<DragOverlay modifiers={[restrictToWindowEdges]}>
  {/* overlay content */}
</DragOverlay>
```

#### Snap to a grid

```tsx
import { createSnapModifier } from "@dnd-kit/modifiers";

const gridSize = 20; // pixels
const snapToGridModifier = createSnapModifier(gridSize);

<DndContext modifiers={[snapToGridModifier]}>
  {/* draggable items */}
</DndContext>
```

---

### Creating Custom Modifiers

A custom modifier is a function that receives an `args` object and returns a new transformation (or any other state modifications).  
The signature is:

```ts
type Modifier = (args: ModifierArgs) => ModifierReturn;
```

`ModifierArgs` contains the current `transform`, among other information.

#### Example – Snap to a 20px grid

```tsx
const gridSize = 20;

function snapToGrid(args: ModifierArgs) {
  const { transform } = args;

  return {
    ...transform,
    x: Math.ceil(transform.x / gridSize) * gridSize,
    y: Math.ceil(transform.y / gridSize) * gridSize,
  };
}

<DndContext modifiers={[snapToGrid]}>
  {/* draggable items */}
</DndContext>
```

> For more complex logic, inspect the source of the built‑in modifiers in the [official repository](https://github.com/clauderic/dnd-kit/tree/master/packages/modifiers/src).

---

## Examples

### 1. Drag only vertically within the window

```tsx
import { DndContext, DragOverlay } from "@dnd-kit/core";
import { restrictToVerticalAxis, restrictToWindowEdges } from "@dnd-kit/modifiers";

export function VerticalDragDemo() {
  return (
    <DndContext modifiers={[restrictToVerticalAxis]}>
      {/* draggable items */}
      <DragOverlay modifiers={[restrictToWindowEdges]}>
        {/* overlay content */}
      </DragOverlay>
    </DndContext>
  );
}
```

### 2. Drag inside a container and snap to a 50px grid

```tsx
import { DndContext } from "@dnd-kit/core";
import { restrictToParentElement, createSnapModifier } from "@dnd-kit/modifiers";

const snapToGrid = createSnapModifier(50);

export function GridDragDemo() {
  return (
    <DndContext modifiers={[restrictToParentElement, snapToGrid]}>
      {/* draggable items */}
    </DndContext>
  );
}
```

### 3. Custom modifier – add resistance when dragging

```tsx
function resistanceModifier(args: ModifierArgs) {
  const { transform } = args;
  const resistance = 0.5; // 0 = no resistance, 1 = full stop

  return {
    ...transform,
    x: transform.x * (1 - resistance),
    y: transform.y * (1 - resistance),
  };
}

<DndContext modifiers={[resistanceModifier]}>
  {/* draggable items */}
</DndContext>
```

---

### Further Reading

- [dnd‑kit documentation – Sensors](https://docs.dndkit.com/)
- [dnd‑kit documentation – Modifiers API](https://docs.dndkit.com/api-specification/modifiers)
- [Source code – Built‑in modifiers](https://github.com/clauderic/dnd-kit/tree/master/packages/modifiers/src)

---

*Last updated: 4 years ago (as of this version of the package).*