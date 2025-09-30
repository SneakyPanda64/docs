# @dnd‑kit

> A lightweight, modular, performant, accessible and extensible drag & drop toolkit for React.

> **Core library** – `@dnd-kit/core`  
> **Presets** – `@dnd-kit/sortable`, etc.

> Published for free with GitBook’s Community Plan – Sponsored via GitBook.

---

## Table of Contents

1. [Overview](#overview)  
2. [Features](#features)  
3. [Installation](#installation)  
4. [Quick Start](#quick-start)  
5. [API Documentation](#api-documentation)  
6. [Sensors](#sensors)  
7. [Modifiers](#modifiers)  
8. [Presets](#presets)  
9. [Extensibility](#extensibility)  
10. [Accessibility](#accessibility)  
11. [Architecture](#architecture)  
12. [Performance](#performance)  

---

## Overview

`@dnd-kit` is a **modular** drag‑and‑drop framework built specifically for React.  
It exposes the following core concepts:

| Concept | What it does | How to use |
|---------|--------------|------------|
| **Draggable** | Elements that can be moved | `useDraggable` hook |
| **Droppable** | Areas that accept draggables | `useDroppable` hook |
| **Context** | Global state and configuration | `<DndContext>` provider |

### Why `@dnd-kit`?

* **Zero external dependencies** – Core library ≈ 10 KB (minified).  
* **Customizable** – Collision detection, collision algorithms, modifiers, sensors, key bindings.  
* **Extensible** – Build your own presets or extend existing ones.  
* **Accessible** – Keyboard support, screen‑reader instructions, live regions.  
* **Performance** – Avoid DOM mutations where possible, use CSS transforms, synthetic events.  
* **Multi‑input** – Pointer, mouse, touch, keyboard sensors out of the box.  

---

## Features

| Feature | Description | Example |
|---------|-------------|---------|
| Collision Detection | Customizable algorithms (e.g., `closestCenter`, `pointerWithin`) | `collisionDetection={closestCenter}` |
| Multiple Activators | Click, touch, keyboard | `activators={[{ type: 'pointer', event: 'pointerdown' }]}` |
| Drag Overlay | Renders a draggable clone that follows the pointer | `<DragOverlay>{item}</DragOverlay>` |
| Auto‑Scrolling | Keeps dragged items visible in scrollable containers | `autoScroll={{ scrollContainer: node => node.closest('.scrollable') }}` |
| Constraints | Lock dragging to an axis or container bounds | `modifiers={[restrictToVerticalAxis]}` |
| Virtualized Lists | Works with `react-window` or `react-virtualized` | `<DndContext items={items} strategy={verticalListSortingStrategy}>…</DndContext>` |
| 2D Games | Dragging sprites in a canvas | Custom `useDragSensor` for WebGL contexts |

---

## Installation

```bash
# npm
npm install @dnd-kit/core @dnd-kit/sortable

# yarn
yarn add @dnd-kit/core @dnd-kit/sortable
```

> **Tip:** Only install the core library if you plan to build custom behaviour.  
> The preset `@dnd-kit/sortable` is useful for list or grid reordering.

---

## Quick Start

Below is a minimal example of a sortable list using `@dnd-kit/sortable`.

```tsx
import { DndContext, useSensor, useSensors, PointerSensor, KeyboardSensor } from '@dnd-kit/core';
import { arrayMove, SortableContext, sortableKeyboardCoordinates } from '@dnd-kit/sortable';
import { useState } from 'react';

function SortableList() {
  const [items, setItems] = useState(['Apple', 'Banana', 'Cherry']);

  const sensors = useSensors(
    useSensor(PointerSensor, { activationConstraint: { distance: 5 } }),
    useSensor(KeyboardSensor, { coordinateGetter: sortableKeyboardCoordinates })
  );

  return (
    <DndContext
      sensors={sensors}
      collisionDetection={closestCenter}
      onDragEnd={(event) => {
        const { active, over } = event;
        if (over && active.id !== over.id) {
          setItems((items) => {
            const oldIndex = items.indexOf(active.id);
            const newIndex = items.indexOf(over.id);
            return arrayMove(items, oldIndex, newIndex);
          });
        }
      }}
    >
      <SortableContext items={items}>
        {items.map((id) => (
          <SortableItem key={id} id={id} />
        ))}
      </SortableContext>
    </DndContext>
  );
}

function SortableItem({ id }: { id: string }) {
  const { attributes, listeners, setNodeRef, transform, transition } = useSortable({ id });
  const style = {
    transform: CSS.Transform.toString(transform),
    transition,
  };

  return (
    <div ref={setNodeRef} style={style} {...attributes} {...listeners}>
      {id}
    </div>
  );
}
```

> **Note:** The `SortableContext` and `useSortable` hooks are part of `@dnd-kit/sortable`.  
> The core library (`@dnd-kit/core`) handles the low‑level drag logic.

---

## API Documentation

### `<DndContext>`

The root provider that manages drag state and configuration.

| Prop | Type | Description |
|------|------|-------------|
| `sensors` | `Sensor[]` | Array of sensor instances. |
| `collisionDetection` | `CollisionDetection` | Algorithm for detecting droppable collisions. |
| `onDragStart` | `(event: DragStartEvent) => void` | Callback when a drag starts. |
| `onDragMove` | `(event: DragMoveEvent) => void` | Callback while dragging. |
| `onDragEnd` | `(event: DragEndEvent) => void` | Callback when a drag ends. |
| `onDragCancel` | `(event: DragCancelEvent) => void` | Callback when a drag is cancelled. |
| `autoScroll` | `AutoScrollOptions` | Configuration for auto‑scrolling during a drag. |

### `useDraggable`

```tsx
const { attributes, listeners, setNodeRef, transform, transition } = useDraggable({ id: string | number });
```

| Return | Description |
|--------|-------------|
| `attributes` | ARIA attributes (`role`, `aria-grabbed`, etc.). |
| `listeners` | Event listeners for activation (pointerdown, touchstart, keydown). |
| `setNodeRef` | Ref callback for the draggable DOM node. |
| `transform` | `Transform` object for CSS transforms. |
| `transition` | Transition style for smooth animation. |

### `useDroppable`

```tsx
const { setNodeRef, isOver } = useDroppable({ id: string | number });
```

| Return | Description |
|--------|-------------|
| `setNodeRef` | Ref callback for the droppable DOM node. |
| `isOver` | Boolean indicating if a draggable is currently over this droppable. |

### `<DragOverlay>`

Renders an overlay element that follows the pointer.

```tsx
<DragOverlay>
  <YourItemComponent item={activeItem} />
</DragOverlay>
```

The overlay is **removed from the normal document flow** and positioned relative to the viewport, preventing layout thrashing.

---

## Sensors

Sensors detect user input and initiate drag actions.

| Sensor | Built‑in | Description |
|--------|----------|-------------|
| `PointerSensor` | ✅ | Handles mouse, touch, and pen. |
| `KeyboardSensor` | ✅ | Handles keyboard navigation. |
| `TouchSensor` | ❌ | (Optional) Separate sensor for touch‑specific logic. |
| `CustomSensor` | ❌ | Create your own sensor by extending `useSensor`. |

**Example of custom sensor**

```tsx
import { useSensor, useSensors, PointerSensor } from '@dnd-kit/core';

const MySensor = () => {
  const sensor = useSensor(PointerSensor, {
    activationConstraint: { distance: 10 },
    eventOptions: { passive: false },
  });

  return sensor;
};

const sensors = useSensors(MySensor());
```

---

## Modifiers

Modifiers transform drag data before it reaches the drop target.

| Modifier | Built‑in | Example |
|----------|----------|---------|
| `restrictToVerticalAxis` | ✅ | Locks movement to the Y‑axis. |
| `restrictToHorizontalAxis` | ✅ | Locks movement to the X‑axis. |
| `restrictToBounds` | ✅ | Keeps the draggable within a container. |
| `restrictToWindowEdges` | ✅ | Prevents dragging outside the viewport. |
| `customModifier` | ❌ | Build your own logic. |

```tsx
<DragOverlay
  modifiers={[restrictToBounds, restrictToWindowEdges]}
>
  {/* overlay content */}
</DragOverlay>
```

---

## Presets

> A *preset* is a thin layer built on top of `@dnd-kit/core` that adds common behaviours.

| Preset | Purpose | When to use |
|--------|---------|-------------|
| `@dnd-kit/sortable` | Reorder lists or grids | When you need sortable items. |
| `@dnd-kit/drag-drop-context` | Simplified drag & drop between containers | For basic drag‑and‑drop workflows. |
| `@dnd-kit/virtual` | Drag in virtualized lists | When working with `react-window` or `react-virtualized`. |

---

## Extensibility

`@dnd-kit` is designed around *extension points*:

| Extension Point | What it lets you do | Example |
|------------------|---------------------|---------|
| **Sensors** | Add support for new input methods | Custom `GamepadSensor` |
| **Modifiers** | Tailor drag constraints | `customModifier` |
| **Collision Detection** | Use a different algorithm | `customCollisionDetection` |
| **Accessibility** | Customize ARIA and live regions | Override screen‑reader instructions |

### Example: Adding a Custom Sensor

```tsx
import { useSensor } from '@dnd-kit/core';

function GamepadSensor() {
  const sensor = useSensor(/* ... */);
  // Add gamepad event listeners, activation logic, etc.
  return sensor;
}
```

---

## Accessibility

`@dnd-kit` includes built‑in support for keyboard navigation, screen‑reader instructions, and live‑region updates.

### Keyboard Support

* Arrow keys to move a dragged item.
* `Space` / `Enter` to pick up and drop an item.
* `Esc` to cancel a drag.

### Screen‑Reader Instructions

You can customize the instructions displayed to assistive technologies:

```tsx
<DndContext
  screenReaderInstructions={{
    dragStart: 'Item picked up. Press arrow keys to move. Press space to drop.',
    dragEnd: 'Item dropped.',
  }}
>
  {/* ... */}
</DndContext>
```

### Live Region

Automatic live‑region announcements keep users informed of drag events. Override the default messages if needed.

---

## Architecture

Unlike many drag‑and‑drop libraries, `@dnd-kit` **does not** rely on the native HTML5 Drag & Drop API.  

**Benefits**

* Works on **touch devices** natively.
* No need for separate code paths for desktop vs. mobile.
* Full control over drag preview, constraints, and animations.
* Smaller bundle size (≈ 10 KB for core).

**Trade‑off**

* Cannot drag between browser windows or across native OS interfaces.

> **Alternative**: If you need native HTML5 Drag & Drop (e.g., file drag from the OS), consider `react-dnd`, which implements that backend.

---

## Performance

### Minimizing DOM Mutations

`@dnd-kit` calculates initial positions once at drag start and uses **CSS transforms** (`translate3d`, `scale`) for movement. This avoids costly reflows and repaints.

### Synthetic Events

Sensors use React’s synthetic event system for activation events, leading to fewer listeners and better performance.

### Example: Optimized Sorting Strategy

```tsx
import { sortableKeyboardCoordinates, arrayMove } from '@dnd-kit/sortable';

const onDragEnd = (event) => {
  const { active, over } = event;
  if (over && active.id !== over.id) {
    setItems((items) => arrayMove(items, items.indexOf(active.id), items.indexOf(over.id)));
  }
};
```

The `arrayMove` helper updates the underlying array without mutating the DOM until the next render cycle.

---

## FAQ

| Question | Answer |
|----------|--------|
| *Can I use `@dnd-kit` in a non‑React project?* | No. It’s built for React and relies on hooks and context. |
| *Does it support server‑side rendering?* | Yes, but you’ll need to ensure any browser‑specific APIs are only called in the client. |
| *How do I add custom collision detection?* | Provide a custom function to `collisionDetection` prop of `<DndContext>`. |
| *Is there a TypeScript definition?* | Yes, the package ships with built‑in types. |

---

## Getting Help

* **Documentation** – This file and the official GitBook docs.  
* **Community** – Join the **@dnd-kit** Discord or GitHub Discussions.  
* **Issues** – Report bugs or request features on the GitHub repo.

---

Happy dragging! 🚀