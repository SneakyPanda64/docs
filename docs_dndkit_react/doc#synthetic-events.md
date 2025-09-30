# dnd‑kit – Drag & Drop Toolkit for React

`@dnd-kit/core` is a lightweight, modular, performance‑focused, accessible, and extensible drag‑and‑drop toolkit for React.  
The library ships with zero external dependencies (≈10 kB minified) and is built around React’s own state and context APIs.

> **Why not use the HTML5 Drag‑and‑Drop API?**  
> The HTML5 API has severe limitations (no touch, no custom preview, no axis constraints, etc.). `@dnd-kit/core` deliberately avoids it to provide a consistent, touch‑friendly experience across all browsers. If you need desktop‑only, inter‑window dragging, consider `react‑dnd` instead.

---

## Table of Contents

- [Installation](#installation)
- [Quick Start](#quick-start)
- [Core Concepts](#core-concepts)
  - [DndContext](#dndcontext)
  - [Draggable](#draggable)
  - [Droppable](#droppable)
  - [DragOverlay](#dragoverlay)
- [Sensors & Modifiers](#sensors--modifiers)
- [Presets](#presets)
  - [Sortable](#sortable)
- [Accessibility](#accessibility)
- [Extensibility](#extensibility)
- [Performance](#performance)
- [Architecture](#architecture)
- [API Reference](#api-reference)
- [Examples](#examples)

---

## Installation

```bash
# npm
npm install @dnd-kit/core

# Yarn
yarn add @dnd-kit/core

# pnpm
pnpm add @dnd-kit/core
```

> Optional: If you want a ready‑made sortable list, install the preset:
> ```bash
> npm install @dnd-kit/sortable
> ```

---

## Quick Start

Below is a minimal working example that shows a list of items that can be reordered by dragging.

```tsx
import React from 'react';
import {
  DndContext,
  useDraggable,
  useDroppable,
  DragOverlay,
} from '@dnd-kit/core';

const DraggableItem = ({ id, children }) => {
  const { attributes, listeners, setNodeRef, transform } = useDraggable({
    id,
  });

  const style = transform
    ? { transform: `translate3d(${transform.x}px, ${transform.y}px, 0)` }
    : undefined;

  return (
    <div
      ref={setNodeRef}
      style={style}
      {...attributes}
      {...listeners}
    >
      {children}
    </div>
  );
};

const DroppableContainer = ({ children }) => {
  const { setNodeRef } = useDroppable({
    id: 'droppable',
  });

  return (
    <div ref={setNodeRef} style={{ minHeight: '200px', border: '1px solid #ccc' }}>
      {children}
    </div>
  );
};

const App = () => {
  const [items, setItems] = React.useState(['one', 'two', 'three']);
  const [activeId, setActiveId] = React.useState(null);

  const handleDragStart = ({ active }) => setActiveId(active.id);
  const handleDragEnd = ({ active, over }) => {
    setActiveId(null);
    if (over && active.id !== over.id) {
      setItems((prev) => {
        const newIndex = prev.indexOf(over.id);
        const oldIndex = prev.indexOf(active.id);
        const updated = [...prev];
        updated.splice(oldIndex, 1);
        updated.splice(newIndex, 0, active.id);
        return updated;
      });
    }
  };

  return (
    <DndContext onDragStart={handleDragStart} onDragEnd={handleDragEnd}>
      <DroppableContainer>
        {items.map((item) => (
          <DraggableItem key={item} id={item}>
            {item}
          </DraggableItem>
        ))}
      </DroppableContainer>

      <DragOverlay>
        {activeId ? <div style={{ padding: 8, background: '#fff' }}>{activeId}</div> : null}
      </DragOverlay>
    </DndContext>
  );
};

export default App;
```

> **Tip:** The `useDraggable` hook returns `attributes`, `listeners`, and `setNodeRef`. Attach them to the DOM node that should become draggable.

---

## Core Concepts

### DndContext

Wrap your app (or any subtree) with `<DndContext>`.  
It provides context for all drag‑and‑drop interactions, handles sensors, and emits events such as `onDragStart`, `onDragMove`, `onDragEnd`, `onDragCancel`.

```tsx
<DndContext
  sensors={sensors}
  collisionDetection={closestCenter}
  onDragStart={handleDragStart}
  onDragEnd={handleDragEnd}
>
  {/* Your draggable/droppable components */}
</DndContext>
```

### Draggable

Make an element draggable:

```tsx
const { attributes, listeners, setNodeRef, transform, transition } = useDraggable({
  id: 'unique-id',
  data: { /* optional data payload */ },
});
```

- **id** – unique identifier for the draggable.
- **data** – arbitrary data that can be read later via `active.data.current`.
- **transform** – current translate offsets (`x`, `y`).
- **transition** – optional CSS transition string.

### Droppable

Make a container receptive to draggable items:

```tsx
const { setNodeRef } = useDroppable({
  id: 'container-id',
});
```

- **id** – unique identifier for the droppable area.

### DragOverlay

Render a preview that is detached from the normal DOM flow:

```tsx
<DragOverlay>
  {active ? <div>{active.data.current.name}</div> : null}
</DragOverlay>
```

The overlay follows the pointer and is positioned relative to the viewport.

---

## Sensors & Modifiers

| Sensor | Description |
|--------|-------------|
| **PointerSensor** | Default pointer (mouse, touch) support. |
| **KeyboardSensor** | Enables keyboard dragging (e.g., arrow keys). |
| **TouchSensor** | Handles touch specifics (long‑press, tap). |
| **CustomSensor** | Create your own by extending `Sensor`. |

```tsx
import { PointerSensor, KeyboardSensor, useSensor, useSensors } from '@dnd-kit/core';

const sensors = useSensors(
  useSensor(PointerSensor),
  useSensor(KeyboardSensor, { coordinateGetter: getKeyboardCoordinates })
);
```

Modifiers alter the drag behavior, e.g.:

- `restrictToWindowEdges` – keeps the item inside the viewport.
- `restrictToRect` – restricts to a bounding rectangle.

```tsx
import { restrictToWindowEdges } from '@dnd-kit/modifiers';

const modifiers = [restrictToWindowEdges];
```

---

## Presets

### Sortable

`@dnd-kit/sortable` adds higher‑level logic for sortable lists. It wraps `useDraggable` and `useDroppable` with sorting strategies and animation helpers.

```tsx
import { arrayMove } from '@dnd-kit/sortable';

const handleDragEnd = (event) => {
  const { active, over } = event;
  if (over && active.id !== over.id) {
    setItems((items) => arrayMove(items, active.index, over.index));
  }
};
```

> **Note:** The sortable preset is a thin layer on top of `@dnd-kit/core`, so you still have full access to all core hooks and sensors.

---

## Accessibility

`@dnd-kit/core` ships with sensible ARIA defaults:

- Keyboard support: arrow keys to move items, `Space` to pick up/drop.
- Screen‑reader announcements via a live region.
- Customizable screen‑reader instructions.

```tsx
<DndContext
  collisionDetection={closestCenter}
  modifiers={[restrictToWindowEdges]}
  sensors={sensors}
>
  {/* ... */}
</DndContext>
```

> Customize the live‑region or ARIA labels by providing a `screenReaderInstructions` object or overriding `ariaLabel`.

---

## Extensibility

`@dnd-kit/core` exposes several extension points:

| Extension | What you can do |
|-----------|-----------------|
| **Sensors** | Add new input methods. |
| **Modifiers** | Adjust drag boundaries, snapping, etc. |
| **Collision detection** | Implement custom algorithms. |
| **Presets** | Build reusable feature packs (e.g., sortable). |

Because the library is zero‑dependency, you can swap out or extend any of these points without touching the core code.

---

## Performance

- **No DOM mutation during drag** – all movement is done with `translate3d`/`scale`.  
- **Lazy calculation** – initial positions are cached when a drag starts.  
- **Synthetic event listeners** – sensors use React’s synthetic events for better performance.

**Tip:** If you have large lists, consider virtualizing them (e.g., `react-window`) and use the `dragOverlay` to render a floating copy.

---

## Architecture

Unlike many drag‑and‑drop libraries, `@dnd-kit/core` does **not** rely on the HTML5 Drag‑and‑Drop API.  
Instead, it:

1. Uses React context to store state.  
2. Calculates collision detection and snapping in JavaScript.  
3. Uses CSS transforms for animations.

This design yields:

- Touch‑friendly interactions.  
- Fine‑grained control over behavior.  
- Consistent experience across browsers.

---

## API Reference

### `DndContext` Props

| Prop | Type | Description |
|------|------|-------------|
| `sensors` | `Sensor[]` | Array of active sensors. |
| `collisionDetection` | `CollisionDetectionFn` | Function that returns colliding draggable IDs. |
| `modifiers` | `Modifier[]` | Functions to adjust drag behavior. |
| `onDragStart` | `(event: DragStartEvent) => void` | Triggered when a drag begins. |
| `onDragMove` | `(event: DragMoveEvent) => void` | Triggered on drag move. |
| `onDragOver` | `(event: DragOverEvent) => void` | Triggered when a draggable enters a droppable. |
| `onDragEnd` | `(event: DragEndEvent) => void` | Triggered when a drag ends. |
| `onDragCancel` | `(event: DragCancelEvent) => void` | Triggered when a drag is cancelled. |
| `children` | `ReactNode` | Components that use drag‑and‑drop hooks. |

### `useDraggable` Options

| Option | Type | Description |
|--------|------|-------------|
| `id` | `string | number` | Unique identifier. |
| `data` | `any` | Arbitrary payload accessible via `active.data.current`. |
| `disabled` | `boolean` | Disable dragging. |

### `useDroppable` Options

| Option | Type | Description |
|--------|------|-------------|
| `id` | `string | number` | Unique identifier. |
| `disabled` | `boolean` | Disable drop. |

### `DragOverlay` Props

| Prop | Type | Description |
|------|------|-------------|
| `children` | `ReactNode` | Rendered node following the cursor. |

---

## Examples

### 1. Basic Draggable Item

```tsx
const { attributes, listeners, setNodeRef, transform } = useDraggable({ id: 'item-1' });

return (
  <div
    ref={setNodeRef}
    style={{ transform: transform ? `translate3d(${transform.x}px, ${transform.y}px, 0)` : undefined }}
    {...attributes}
    {...listeners}
  >
    Drag me!
  </div>
);
```

### 2. Droppable List with Sortable Items

```tsx
import { DndContext, useDraggable, useDroppable, DragOverlay } from '@dnd-kit/core';
import { arrayMove } from '@dnd-kit/sortable';

function SortableList({ items }) {
  const [order, setOrder] = React.useState(items);
  const [activeId, setActiveId] = React.useState(null);

  const handleDragStart = ({ active }) => setActiveId(active.id);
  const handleDragEnd = ({ active, over }) => {
    setActiveId(null);
    if (over && active.id !== over.id) {
      setOrder((prev) => arrayMove(prev, prev.indexOf(active.id), prev.indexOf(over.id)));
    }
  };

  return (
    <DndContext onDragStart={handleDragStart} onDragEnd={handleDragEnd}>
      <div>
        {order.map((id) => (
          <SortableItem key={id} id={id} />
        ))}
      </div>

      <DragOverlay>
        {activeId ? <div>{activeId}</div> : null}
      </DragOverlay>
    </DndContext>
  );
}
```

---

### 3. Keyboard‑Only Dragging

```tsx
import { useSensor, useSensors, PointerSensor, KeyboardSensor, DndContext } from '@dnd-kit/core';

const sensors = useSensors(
  useSensor(PointerSensor),
  useSensor(KeyboardSensor, {
    coordinateGetter: (args) => {
      // Custom logic for keyboard dragging
      return args;
    },
  })
);

return (
  <DndContext sensors={sensors}>
    {/* Draggable / Droppable components */}
  </DndContext>
);
```

---

## Conclusion

`@dnd-kit/core` gives you a lightweight, high‑performance drag‑and‑drop foundation that you can extend with your own sensors, modifiers, and presets.  
Whether you need a simple sortable list or a complex multi‑container grid, the toolkit scales to your use case while keeping accessibility and performance at its core.