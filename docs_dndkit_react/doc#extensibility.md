# dnd‑kit – A Lightweight, Modular Drag & Drop Toolkit for React

> **dnd‑kit** is a small, zero‑dependency library that gives you a solid foundation for building drag‑and‑drop interfaces in React.  
> It is built around React state & context, offers a rich API surface (hooks, providers, components), and is fully accessible out of the box.

---

## Table of Contents

| Section | Description |
|---------|-------------|
| [Introduction](#introduction) | What dnd‑kit is and why you might choose it |
| [Installation](#installation) | Add the core and optional presets to your project |
| [Quick Start](#quick-start) | A minimal working example |
| [API Documentation](#api-documentation) | Core concepts, hooks, components, and props |
| [Sensors](#sensors) | Built‑in and custom input handling |
| [Modifiers](#modifiers) | Transform the drag interaction |
| [Collision Detection](#collision-detection) | Detect overlaps between draggable & droppable |
| [Presets](#presets) | Sortable, reorder, etc. |
| [Accessibility](#accessibility) | Keyboard support & screen‑reader helpers |
| [Architecture](#architecture) | Why we avoid the HTML5 Drag & Drop API |
| [Performance](#performance) | Avoiding repaints & efficient DOM usage |
| [Extensibility](#extensibility) | Build your own extensions on top of the core |
| [Examples](#examples) | Real‑world code snippets |
| [FAQ](#faq) | Common questions & troubleshooting |

---

## Introduction

dnd‑kit is a **lightweight, modular, performant, accessible, and extensible** toolkit for creating drag & drop interfaces in React. It exposes:

* **`useDraggable` & `useDroppable`** hooks to turn any component into a draggable or droppable area.
* A **`<DndContext>`** provider that orchestrates the interaction and lets you configure sensors, collision detection, and more.
* A **`<DragOverlay>`** component that renders a floating element that follows the cursor/finger without affecting layout.

The core package is ~10 KB minified and has *no external dependencies*. It’s built on top of React’s own state and context mechanisms, which keeps the library lean.

> **Why not use the native HTML5 Drag & Drop API?**  
> It’s limited (no touch support, hard‑to‑customize previews, etc.) and requires a separate implementation for mobile. dnd‑kit implements everything natively, so you get a consistent experience across desktop, mobile, and keyboard.

---

## Installation

```bash
# npm
npm install @dnd-kit/core @dnd-kit/sortable

# yarn
yarn add @dnd-kit/core @dnd-kit/sortable
```

> The core library is sufficient for most use‑cases. The **@dnd-kit/sortable** preset is a thin layer that adds sorting logic for lists/grids.

---

## Quick Start

Below is a minimal example that lets you drag and drop a list of items.

```tsx
import { DndContext, useDraggable, useDroppable } from '@dnd-kit/core';

function DraggableItem({ id, children }) {
  const { attributes, listeners, setNodeRef, transform } = useDraggable({
    id,
  });

  const style = {
    transform: transform ? `translate3d(${transform.x}px, ${transform.y}px, 0)` : '',
    touchAction: 'none', // prevents unwanted browser gestures
  };

  return (
    <div
      ref={setNodeRef}
      style={style}
      {...listeners}
      {...attributes}
    >
      {children}
    </div>
  );
}

function DroppableArea({ children }) {
  const { setNodeRef } = useDroppable({
    id: 'dropzone',
  });

  return <div ref={setNodeRef}>{children}</div>;
}

export default function App() {
  const handleDragEnd = ({ active, over }) => {
    if (over) {
      console.log(`Dropped ${active.id} over ${over.id}`);
    }
  };

  return (
    <DndContext onDragEnd={handleDragEnd}>
      <DroppableArea>
        <DraggableItem id="1">Item 1</DraggableItem>
        <DraggableItem id="2">Item 2</DraggableItem>
        <DraggableItem id="3">Item 3</DraggableItem>
      </DroppableArea>
    </DndContext>
  );
}
```

**Key points**

| Feature | Explanation |
|---------|-------------|
| `useDraggable` | Turns a component into a draggable element. |
| `useDroppable` | Marks a container that accepts drops. |
| `transform` | CSS transform that moves the element. |
| `onDragEnd` | Callback when a drag ends – you can update state, persist data, etc. |

---

## API Documentation

### `DndContext`

```tsx
<DndContext
  sensors={[/* optional Sensor objects */]}
  collisionDetection={/* algorithm */}
  modifiers={[/* optional modifiers */]}
  onDragStart={fn}
  onDragMove={fn}
  onDragEnd={fn}
  onDragCancel={fn}
>
  {children}
</DndContext>
```

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `sensors` | `Sensor[]` | `defaultSensors` (pointer, touch, mouse, keyboard) | Defines how user input is handled. |
| `collisionDetection` | `CollisionDetection` | `closestCenter` | Strategy used to determine which droppable is under the cursor. |
| `modifiers` | `Modifier[]` | `[]` | Functions that adjust the drag state (e.g., constrain to axis, bounding box). |
| `onDragStart` | `(args) => void` | – | Fires when a draggable is activated. |
| `onDragMove` | `(args) => void` | – | Fires while the item is being dragged. |
| `onDragEnd` | `(args) => void` | – | Fires when the drag ends (either dropped or cancelled). |
| `onDragCancel` | `(args) => void` | – | Fires when the drag is cancelled (e.g., ESC key). |

### `useDraggable`

```ts
const {
  attributes,
  listeners,
  setNodeRef,
  transform,
  isDragging,
} = useDraggable({
  id: string,
  data?: any,          // optional data passed through the drag state
  disabled?: boolean,  // disable dragging
});
```

| Return | Type | Description |
|--------|------|-------------|
| `attributes` | `HTMLAttributes` | Spread onto the element to provide `role="grabbable"` and ARIA attributes. |
| `listeners` | `EventHandlers` | Spread onto the element to bind drag events. |
| `setNodeRef` | `(node: HTMLElement | null) => void` | Attach to the component’s root element. |
| `transform` | `{ x: number; y: number } | null` | Current translation while dragging. |
| `isDragging` | `boolean` | Whether the element is currently being dragged. |

### `useDroppable`

```ts
const {
  setNodeRef,
  isOver,
  isDraggingOver,
} = useDroppable({
  id: string,
  data?: any,
  disabled?: boolean,
});
```

| Return | Type | Description |
|--------|------|-------------|
| `setNodeRef` | `(node: HTMLElement | null) => void` | Attach to the container element. |
| `isOver` | `boolean` | `true` while an item is hovered over the droppable. |
| `isDraggingOver` | `boolean` | `true` while a draggable is *currently* being dragged over it. |

### `DragOverlay`

```tsx
<DragOverlay>
  {({ active }) => {
    if (!active) return null;
    return <div style={{ /* styles for overlay */ }}>{active.data?.content}</div>;
  }}
</DragOverlay>
```

`DragOverlay` renders a floating element that follows the cursor. It receives the same drag state as `useDraggable` but is *outside* the normal DOM flow.

---

## Sensors

Sensors convert raw pointer/mouse/touch/keyboard events into drag state. Built‑in sensors:

| Sensor | Description |
|--------|-------------|
| `PointerSensor` | Handles mouse & touch. |
| `KeyboardSensor` | Enables keyboard drag (arrow keys, space, etc.). |
| `TouchSensor` | Touch‑only fallback (legacy). |
| `MouseSensor` | Mouse‑only (rarely needed). |

> **Custom Sensor**: implement the `Sensor` interface to create your own activators (e.g., long‑press, external triggers).

---

## Modifiers

Modifiers adjust the drag state before it is applied:

| Modifier | Purpose |
|----------|---------|
| `restrictToAxis` | Confine movement to `x` or `y`. |
| `restrictToBox` | Keep the item inside a bounding box. |
| `restrictToVerticalAxis` | Same as `restrictToAxis('y')`. |
| `restrictToHorizontalAxis` | Same as `restrictToAxis('x')`. |
| `autoScroll` | Automatically scroll parent containers when near the edge. |

You compose them in the `modifiers` array passed to `<DndContext>`.

---

## Collision Detection

Collision detection decides which droppable a draggable is currently over. The core library ships with several algorithms:

| Algorithm | When to use |
|-----------|-------------|
| `closestCenter` | Default – chooses the droppable whose center is closest to the draggable’s center. |
| `pointerWithin` | Checks if the pointer is inside a droppable’s bounding rectangle. |
| `closestCorners` | Chooses the droppable with the closest corner to the pointer. |
| `offsetCenter` | Uses the offset of the drag (e.g., for virtual lists). |

You can provide a custom algorithm that returns an array of droppable IDs sorted by priority.

---

## Presets

### `@dnd-kit/sortable`

A thin layer that adds **sorting logic** to a list or grid. It uses the core's extension points (sensors, modifiers, collision detection) internally.

```tsx
import { arrayMove } from '@dnd-kit/sortable';

function SortableList({ items, setItems }) {
  const handleDragEnd = ({ active, over }) => {
    if (over && active.id !== over.id) {
      const oldIndex = items.findIndex(item => item.id === active.id);
      const newIndex = items.findIndex(item => item.id === over.id);
      setItems(arrayMove(items, oldIndex, newIndex));
    }
  };

  return (
    <DndContext onDragEnd={handleDragEnd}>
      {items.map(item => (
        <SortableItem key={item.id} id={item.id}>{item.content}</SortableItem>
      ))}
    </DndContext>
  );
}
```

Other presets are coming soon; the architecture lets you build your own on top of the core.

---

## Accessibility

| Feature | Description |
|---------|-------------|
| Keyboard support | Built‑in `KeyboardSensor` moves items with arrow keys, activates with space/enter, cancels with ESC. |
| ARIA attributes | `role="grabbable"`, `aria-grabbed`, `aria-describedby` are automatically applied. |
| Screen‑reader instructions | `useScreenReaderInstructions` hook for custom messages. |
| Live region updates | Real‑time announcements of drop events and drag progress. |

**Tip**: If you have custom UI, always forward the `attributes` and `listeners` to the DOM node to preserve the accessible behavior.

---

## Architecture

- **No HTML5 Drag & Drop API**: We avoid the native API entirely to support touch, custom previews, axis constraints, and smooth animations.
- **React‑centric**: All state lives inside React context; no external event listeners on the window.
- **Modular**: The core is tiny; optional features (sortable, collision detection) live in separate packages.

---

## Performance

- **Lazy calculation of item positions**: On drag start, dnd‑kit caches client rects and uses them to compute positions.
- **CSS transforms**: We use `translate3d` / `scale` to move items, avoiding reflows.
- **Synthetic event listeners**: Sensors listen via React’s synthetic events, reducing overhead.
- **No DOM mutations during drag**: Prefer using transforms over re‑ordering DOM nodes; if you must, be aware of the repaint cost.

---

## Extensibility

The core exposes extension points:

- **Sensors** – Create custom input handling (e.g., long‑press, external triggers).
- **Modifiers** – Write reusable transformation functions.
- **Collision Detection** – Implement custom hit‑testing logic.
- **Accessibility** – Plug in your own ARIA/live‑region helpers.

The preset architecture (Sortable, Reorder, etc.) is built entirely on these points, so you can mix and match as needed.

---

## Examples

### 1. Draggable Card with Overlay

```tsx
function Card({ id, content }) {
  const { attributes, listeners, setNodeRef, transform } = useDraggable({ id });

  return (
    <div
      ref={setNodeRef}
      {...attributes}
      {...listeners}
      style={{
        transform: transform
          ? `translate3d(${transform.x}px, ${transform.y}px, 0)`
          : '',
      }}
    >
      {content}
    </div>
  );
}

export function DraggableList({ items }) {
  return (
    <DndContext
      onDragEnd={({ active, over }) => {
        if (over) console.log(`Moved ${active.id} to ${over.id}`);
      }}
    >
      <DragOverlay>
        {({ active }) =>
          active ? <div>{active.data?.content}</div> : null
        }
      </DragOverlay>
      {items.map(item => (
        <Card key={item.id} id={item.id} content={item.content} />
      ))}
    </DndContext>
  );
}
```

### 2. Sortable List

```tsx
import { arrayMove, SortableContext, sortableKeyboardCoordinates } from '@dnd-kit/sortable';

function SortableItem({ id, content }) {
  const { attributes, listeners, setNodeRef, transform } = useDraggable({ id });

  return (
    <li
      ref={setNodeRef}
      {...attributes}
      {...listeners}
      style={{
        transform: transform
          ? `translate3d(${transform.x}px, ${transform.y}px, 0)`
          : '',
      }}
    >
      {content}
    </li>
  );
}

export function SortableList({ items, setItems }) {
  return (
    <DndContext
      sensors={[keyboardSensor, mouseSensor]}
      onDragEnd={({ active, over }) => {
        if (over && active.id !== over.id) {
          const oldIndex = items.findIndex(item => item.id === active.id);
          const newIndex = items.findIndex(item => item.id === over.id);
          setItems(arrayMove(items, oldIndex, newIndex));
        }
      }}
    >
      <SortableContext items={items.map(i => i.id)}>
        <ul>
          {items.map(item => (
            <SortableItem key={item.id} id={item.id} content={item.content} />
          ))}
        </ul>
      </SortableContext>
    </DndContext>
  );
}
```

### 3. Keyboard‑Only Dragging

```tsx
<DndContext
  sensors={[keyboardSensor]}
  collisionDetection={pointerWithin}
>
  {/* draggable items */}
</DndContext>
```

Press **Space** or **Enter** to pick an item, use arrow keys to move, and press **Space** again to drop.

---

## FAQ

| Question | Answer |
|----------|--------|
| **Can I drag from one browser window to another?** | No. dnd‑kit does not use the native HTML5 Drag & Drop API, so cross‑window dragging is not supported. |
| **Will my list re‑render on every drag step?** | No. dnd‑kit keeps all state internal. You only re‑render the draggable that moves; the rest stay untouched. |
| **How do I support touch?** | Include the built‑in `PointerSensor` (default). No extra work is required. |
| **Is it safe for SSR?** | Yes. All sensors are activated only in the browser. |

---

### License

MIT © 2024 dnd‑kit contributors.

---

> This documentation is a distilled, sanitized version of the official dnd‑kit docs. All code snippets are kept verbatim and translated where necessary. Happy dragging!