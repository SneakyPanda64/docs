# @dnd‑kit – Drag & Drop Toolkit for React

> A lightweight, modular, performant, accessible and extensible drag‑and‑drop library for React.  
> Core (≈ 10 KB minified) → *zero* external dependencies, built entirely around React’s state and context.

---

## Table of Contents

- [Overview](#overview)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [API Documentation](#api-documentation)
  - [DndContext](#dndcontext)
  - [Droppable](#droppable)
  - [Draggable](#draggable)
  - [Sensors](#sensors)
  - [Modifiers](#modifiers)
- [Presets](#presets)
  - [Sortable](#sortable)
- [Guides](#guides)
  - [Accessibility](#accessibility)
  - [Extensibility](#extensibility)
  - [Architecture](#architecture)
  - [Performance](#performance)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

`@dnd-kit` is a **feature‑rich** drag‑and‑drop solution that offers:

| Feature | Description |
|---------|-------------|
| **Customizable collision detection** | Plug‑in your own algorithm or use the built‑in ones. |
| **Multiple activators** | Drag on pointer, mouse, touch *and* keyboard. |
| **Draggable overlay** | Render a ghost element that follows the cursor. |
| **Drag handles** | Limit drag to a specific child. |
| **Auto‑scrolling** | Automatically scroll containers while dragging. |
| **Constraints** | Lock axis, restrict to bounds, etc. |
| **Zero deps** | Core is a single, lean bundle. |
| **Extensible** | Build your own presets (e.g., `sortable`) on top of the core. |
| **Accessible** | Keyboard support, ARIA attributes, screen‑reader instructions. |
| **Performance‑first** | Minimal DOM mutations, efficient CSS transforms. |

It supports **lists, grids, nested containers, virtualized lists, 2D games**, and more—all without requiring you to restructure your app or add wrapper DOM nodes.

---

## Installation

```bash
# Using npm
npm install @dnd-kit/core

# Using yarn
yarn add @dnd-kit/core
```

If you need the sortable preset:

```bash
npm install @dnd-kit/sortable
```

---

## Quick Start

```tsx
import { DndContext, useDraggable, useDroppable } from '@dnd-kit/core';
import { DragOverlay } from '@dnd-kit/core';
import { CSS } from '@dnd-kit/utilities';

function DraggableBox() {
  const { attributes, listeners, setNodeRef, transform } = useDraggable({
    id: 'box',
  });

  const style = {
    transform: CSS.Transform.toString(transform),
    cursor: 'grab',
    background: '#90caf9',
    padding: '1rem',
    borderRadius: '4px',
  };

  return (
    <div ref={setNodeRef} {...listeners} {...attributes} style={style}>
      Drag me
    </div>
  );
}

function DroppableArea() {
  const { isOver, setNodeRef } = useDroppable({
    id: 'area',
  });

  return (
    <div
      ref={setNodeRef}
      style={{
        width: 300,
        height: 200,
        border: '2px dashed #aaa',
        background: isOver ? '#e3f2fd' : '#fafafa',
      }}
    >
      Drop here
    </div>
  );
}

export default function App() {
  const [activeId, setActiveId] = React.useState<React.ReactNode>(null);

  return (
    <DndContext
      onDragStart={(event) => setActiveId(event.active.id)}
      onDragEnd={() => setActiveId(null)}
    >
      <DraggableBox />
      <DroppableArea />

      {activeId && (
        <DragOverlay>
          <div
            style={{
              width: 100,
              height: 100,
              background: '#ffb74d',
              borderRadius: '4px',
            }}
          >
            {activeId}
          </div>
        </DragOverlay>
      )}
    </DndContext>
  );
}
```

> *This minimal example shows a draggable square and a drop zone. The `DragOverlay` renders the item in the viewport, detached from the DOM flow.*

---

## API Documentation

### `DndContext`

Wraps your application (or a subtree) and manages the drag‑and‑drop state.

| Prop | Type | Description |
|------|------|-------------|
| `sensors` | `Sensor[]` | Optional array of custom sensors. |
| `collisionDetection` | `CollisionDetectionAlgorithm` | How to detect droppable targets. |
| `onDragStart` | `(event: DragStartEvent) => void` | Fired when a drag begins. |
| `onDragMove` | `(event: DragMoveEvent) => void` | Fired continuously during a drag. |
| `onDragEnd` | `(event: DragEndEvent) => void` | Fired when a drag ends. |
| `onDragCancel` | `(event: DragCancelEvent) => void` | Fired when a drag is aborted. |

---

### `Droppable`

Hook to make an element a drop target.

```tsx
const { isOver, setNodeRef } = useDroppable({
  id: 'my-dropzone',
});
```

| Return | Type | Description |
|--------|------|-------------|
| `isOver` | `boolean` | True when an item is dragged over the area. |
| `setNodeRef` | `(node: HTMLElement | null) => void` | Ref setter for the DOM node. |

---

### `Draggable`

Hook to make an element draggable.

```tsx
const {
  attributes,
  listeners,
  setNodeRef,
  transform,
  transition,
} = useDraggable({
  id: 'my-draggable',
});
```

| Return | Type | Description |
|--------|------|-------------|
| `attributes` | `DOMAttributes` | ARIA attributes for accessibility. |
| `listeners` | `EventListeners` | Pointer/mouse/touch event listeners. |
| `setNodeRef` | `(node: HTMLElement | null) => void` | Ref setter for the DOM node. |
| `transform` | `Transform` | Current transform (`{x, y}`). |
| `transition` | `string` | Current CSS transition value. |

---

### `Sensors`

Sensors translate user input into drag events.

| Built‑in | Description |
|----------|-------------|
| `pointerSensor` | Handles mouse & touch. |
| `keyboardSensor` | Handles keyboard navigation. |
| `touchSensor` | Handles touch‑only scenarios. |

You can create custom sensors to support new input methods or custom activators.

---

### `Modifiers`

Modifiers adjust the drag behaviour.  
Examples: `restrictToParentElement`, `restrictToWindow`, `snapCenter`, etc.

```tsx
const modifiers = [
  restrictToParentElement(),
  snapCenter({ offset: 30 }),
];
```

They receive the drag context and can return a new `Transform`.

---

## Presets

Presets are thin layers that build on top of the core API, adding convenience for common patterns.

### `Sortable`

```tsx
import { SortableContext, arrayMove } from '@dnd-kit/sortable';
import { useSortable } from '@dnd-kit/sortable';
```

A sortable list is just a collection of `Draggable` items inside a `SortableContext`.  
The preset automatically calculates new indices and calls a callback on `onDragEnd`.

---

## Guides

### Accessibility

- **Keyboard support** out of the box.  
- **ARIA attributes** automatically added to draggable elements.  
- **Custom screen‑reader instructions** can be supplied via the `ScreenReaderInstructions` provider.  
- **Live region updates** keep assistive tech informed of drag status.

> *Tip:* Wrap your app with `<ScreenReaderInstructions>` and provide a `message` prop to customize the announcements.

### Extensibility

`@dnd-kit` exposes four main extension points:

| Point | What you can customize |
|-------|------------------------|
| `Sensors` | Input handling |
| `Modifiers` | Drag behaviour |
| `CollisionDetectionAlgorithm` | How targets are chosen |
| `Accessibility` | ARIA, screen‑reader logic |

The **Sortable** preset demonstrates how to compose these points into a new feature.

### Architecture

Unlike most libraries, `@dnd-kit` does **not** use the native HTML5 Drag & Drop API.  
*Pros*:

- Full touch support across all browsers.
- No extra backend needed for custom drag previews or constraints.
- Cleaner API for React.

*Cons*:

- Cannot drag between windows or from desktop to other applications.

If you need desktop‑to‑desktop drag‑and‑drop, consider `react-dnd` which builds on the HTML5 API.

### Performance

- **Lazy rect calculations**: positions are captured once per drag.
- **CSS transforms** (`translate3d`, `scale`) avoid repaints.
- **Synthetic events**: sensors use React’s synthetic event system for efficient listeners.
- **Minimal DOM mutation**: only the overlay is moved; items can be repositioned via transforms.

---

## Contributing

We welcome contributions! Check out the [contributing guidelines](CONTRIBUTING.md) for setup, tests, and code style.

---

## License

MIT © 2025 @dnd-kit

---