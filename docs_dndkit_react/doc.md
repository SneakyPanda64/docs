# @dnd‑kit – A Lightweight, Modular Drag & Drop Toolkit for React

> **@dnd‑kit** is a zero‑dependency, highly‑performant, and fully‑accessible drag‑and‑drop solution for React.  
> It gives you full control over collision detection, sensors, modifiers, and more – while still letting you build complex UIs (lists, grids, nested containers, virtualised lists, 2‑D games, etc.) with minimal boilerplate.

---

## Table of Contents

- [Installation](#installation)
- [Quick Start](#quick-start)
- [API Reference](#api-reference)
  - [DndContext](#dndcontext)
  - [Draggable](#draggable)
  - [Droppable](#droppable)
  - [DragOverlay](#dragoverlay)
  - [Sensors](#sensors)
  - [Modifiers](#modifiers)
- [Presets](#presets)
  - [Sortable](#sortable)
- [Accessibility](#accessibility)
- [Performance](#performance)
- [Architecture](#architecture)
- [Extensibility](#extensibility)
- [Examples](#examples)

---

## Installation

```bash
# Using npm
npm i @dnd-kit/core

# Using yarn
yarn add @dnd-kit/core
```

> Optional: For the Sortable preset (e.g. re‑ordering lists)  
> ```bash
> npm i @dnd-kit/sortable
> ```

---

## Quick Start

```tsx
import React from 'react';
import { DndContext, useDraggable, useDroppable } from '@dnd-kit/core';

function DraggableItem({ id, children }) {
  const { attributes, listeners, setNodeRef, transform } = useDraggable({ id });

  const style = transform
    ? { transform: `translate3d(${transform.x}px, ${transform.y}px, 0)` }
    : undefined;

  return (
    <div ref={setNodeRef} style={style} {...attributes} {...listeners}>
      {children}
    </div>
  );
}

function DroppableArea({ id, children }) {
  const { isOver, setNodeRef } = useDroppable({ id });

  const style = {
    border: isOver ? '2px solid #4A90E2' : '2px solid #ccc',
    padding: 10,
  };

  return (
    <div ref={setNodeRef} style={style}>
      {children}
    </div>
  );
}

export default function App() {
  return (
    <DndContext
      collisionDetection={closestCenter}
      onDragEnd={({ active, over }) => console.log(active.id, over?.id)}
    >
      <DroppableArea id="list">
        <DraggableItem id="item-1">Item 1</DraggableItem>
        <DraggableItem id="item-2">Item 2</DraggableItem>
      </DroppableArea>
    </DndContext>
  );
}
```

> **Note**: The above example uses the **closestCenter** collision detection algorithm.  
> You can swap it for any custom algorithm provided by the library.

---

## API Reference

### `DndContext`

*The root provider that powers the entire drag‑and‑drop experience.*

| Prop | Type | Required | Description |
|------|------|----------|-------------|
| `collisionDetection` | `CollisionDetection` | No | Function that decides where the draggable element is dropped. Default: `closestCenter`. |
| `sensors` | `Sensors[]` | No | Array of sensor objects that capture pointer, keyboard, touch, etc. Default: `[pointerSensor(), keyboardSensor()]`. |
| `modifiers` | `Modifier[]` | No | Array of functions that can tweak the dragging behavior (e.g. restrict movement to an axis). |
| `onDragStart` | `(event) => void` | No | Callback when a drag starts. |
| `onDragMove` | `(event) => void` | No | Callback while dragging. |
| `onDragEnd` | `(event) => void` | No | Callback when a drag ends. |
| `onDragCancel` | `(event) => void` | No | Callback when a drag is cancelled (e.g. escape key). |

> **Example:**
> ```tsx
> <DndContext
>   collisionDetection={closestCenter}
>   onDragEnd={handleDragEnd}
> />
> ```

---

### `useDraggable`

Hook that turns any component into a draggable item.

```tsx
const {
  attributes,   // e.g. aria attributes
  listeners,    // e.g. onPointerDown
  setNodeRef,   // ref setter for the DOM node
  transform,    // {x, y} position during drag
  isDragging,   // boolean
  over,         // droppable id currently hovered
  active,       // active draggable id
} = useDraggable({ id: 'unique-id' });
```

**Key Points**

- `transform` can be applied with CSS `translate3d` to animate the element.
- `attributes` & `listeners` should be spread onto the root DOM node for accessibility.

---

### `useDroppable`

Hook that designates a component as a drop target.

```tsx
const {
  setNodeRef,   // ref setter for the DOM node
  isOver,       // true if a draggable is hovered
  active,       // active draggable id
  droppableId,  // this droppable’s id
} = useDroppable({ id: 'droppable-id' });
```

---

### `DragOverlay`

Component that renders a “ghost” copy of the dragged item that is *outside* of the normal DOM flow (attached to the viewport). It’s useful when you want to show a preview of what’s being dragged.

```tsx
<DragOverlay>
  {active && <div>{active.id}</div>}
</DragOverlay>
```

> **Tip**: Keep the overlay lightweight – it should contain only the visual representation of the dragged item.

---

### `Sensors`

Sensors translate input events into drag actions.

| Built‑in Sensor | Trigger | Notes |
|-----------------|---------|-------|
| `pointerSensor()` | Pointer, mouse, touch | Handles all pointing devices. |
| `keyboardSensor()` | Keyboard (Space + Arrow keys) | Enables drag via keyboard for accessibility. |
| `touchSensor()` | Touch | Alternative for touch‑only setups. |

**Example** – custom sensor:

```tsx
const customSensor = {
  activatorEvent: 'pointerdown',
  listenerEvent: 'pointermove',
  onActivate: (event) => { /* custom logic */ },
};
```

---

### `Modifiers`

Functions that transform the drag state before it’s applied (e.g. constrain movement to a specific axis, snap to grid, etc.).

```tsx
import { restrictToVerticalAxis } from '@dnd-kit/core';

<DndContext modifiers={[restrictToVerticalAxis]} />
```

---

## Presets

### `@dnd-kit/sortable`

A thin wrapper that turns a list into a sortable collection. It leverages the core's hooks and extends them with sorting‑specific logic.

```tsx
import { SortableContext, useSortable, verticalListSortingStrategy } from '@dnd-kit/sortable';

function SortableItem({ id }) {
  const { attributes, listeners, setNodeRef, transform } = useSortable({ id });
  const style = transform ? { transform: `translate3d(${transform.x}px, ${transform.y}px, 0)` } : {};

  return (
    <div ref={setNodeRef} style={style} {...attributes} {...listeners}>
      {id}
    </div>
  );
}

function SortableList() {
  const items = ['a', 'b', 'c'];
  return (
    <DndContext>
      <SortableContext items={items} strategy={verticalListSortingStrategy}>
        {items.map((id) => <SortableItem key={id} id={id} />)}
      </SortableContext>
    </DndContext>
  );
}
```

---

## Accessibility

Accessibility is baked‑in:

- **Keyboard Support** – use `keyboardSensor()` to let users drag with keys.
- **ARIA Attributes** – `useDraggable` and `useDroppable` expose sensible defaults.
- **Screen Reader Instructions** – customizable instructions and live region updates can be configured in the context.
- **Live Regions** – provide real‑time announcements for drag start, movement, and drop.

---

## Performance

*Why @dnd‑kit outperforms many alternatives:*

1. **No DOM Mutations During Drag**  
   - Positions are calculated once at drag start.  
   - Elements are moved with `translate3d` and `scale`, avoiding layout thrashing.
2. **Synthetic Events** – Sensors attach listeners once per sensor type instead of per element, reducing event handling overhead.
3. **Lazy Calculations** – Only compute collision detections when necessary.

> **Best practice**: Avoid mutating the DOM inside your drag handlers unless absolutely required.

---

## Architecture

@dnd‑kit intentionally **does not** use the native HTML5 Drag & Drop API.  
Advantages:

- Full control over touch and mouse events.
- Consistent behavior across all browsers.
- Easier to customize drag preview, constrain movement, and animate items.

Trade‑off:

- Cannot drag between native desktop windows or across browsers.

If your use case requires native HTML5 drag‑and‑drop (e.g., desktop file manager), consider libraries like **react‑dnd** that wrap the native API.

---

## Extensibility

The core library exposes *extension points*:

- **Sensors** – Add custom input handling.
- **Modifiers** – Add custom physics or constraints.
- **Collision Detection** – Write your own algorithm.
- **Accessibility** – Override or extend ARIA and live‑region behaviour.

> **Example** – Custom collision detection that ignores hidden items:

```tsx
import { getIntersectionRect } from '@dnd-kit/core';

function ignoreHiddenCollision({ over, draggableRect, droppableRects }) {
  const visibleDroppables = droppableRects.filter(r => !r.hidden);
  return getIntersectionRect(draggableRect, visibleDroppables);
}
```

---

## Examples

Below are a few ready‑to‑copy code snippets you can drop into your project.

### 1. Basic Drag & Drop

```tsx
// index.tsx
import { DndContext, useDraggable, useDroppable } from '@dnd-kit/core';

function Box({ id }) {
  const { setNodeRef, listeners, attributes, transform } = useDraggable({ id });
  const style = transform ? { transform: `translate3d(${transform.x}px, ${transform.y}px, 0)` } : {};

  return (
    <div ref={setNodeRef} style={style} {...listeners} {...attributes}>
      {id}
    </div>
  );
}

function DropZone() {
  const { setNodeRef, isOver } = useDroppable({ id: 'zone' });
  const background = isOver ? '#e0f7fa' : '#fafafa';

  return (
    <div ref={setNodeRef} style={{ padding: 20, background }}>
      Drop here
    </div>
  );
}

export default function App() {
  return (
    <DndContext
      collisionDetection={closestCenter}
      onDragEnd={handleDragEnd}
    >
      <Box id="box1" />
      <DropZone />
    </DndContext>
  );
}
```

### 2. Drag Overlay

```tsx
import { DndContext, DragOverlay } from '@dnd-kit/core';

export default function OverlayDemo() {
  const [activeId, setActiveId] = React.useState(null);

  return (
    <DndContext
      onDragStart={event => setActiveId(event.active.id)}
      onDragEnd={() => setActiveId(null)}
    >
      {/* ...draggable components */}
      <DragOverlay>
        {activeId && <div className="overlay">{activeId}</div>}
      </DragOverlay>
    </DndContext>
  );
}
```

### 3. Sortable List

```tsx
import { SortableContext, useSortable, verticalListSortingStrategy } from '@dnd-kit/sortable';

function SortableItem({ id }) {
  const { attributes, listeners, setNodeRef, transform } = useSortable({ id });
  const style = transform ? { transform: `translate3d(${transform.x}px, ${transform.y}px, 0)` } : {};

  return (
    <div ref={setNodeRef} style={style} {...attributes} {...listeners}>
      {id}
    </div>
  );
}

function SortableList() {
  const items = ['one', 'two', 'three'];
  return (
    <DndContext>
      <SortableContext items={items} strategy={verticalListSortingStrategy}>
        {items.map(id => <SortableItem key={id} id={id} />)}
      </SortableContext>
    </DndContext>
  );
}
```

---

## Getting Help

- **Documentation** – https://github.com/clauderic/dnd-kit
- **Community** – https://github.com/clauderic/dnd-kit/discussions
- **Issues** – https://github.com/clauderic/dnd-kit/issues

Happy dragging!