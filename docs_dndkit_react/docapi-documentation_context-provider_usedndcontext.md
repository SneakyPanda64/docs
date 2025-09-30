# DndKit API Documentation

> **Version** – 4 years old  
> **Last updated** – 4 years ago  

This documentation describes the public API of the **@dnd‑kit/core** package. It is intended for developers who want to build custom drag‑and‑drop presets or need direct access to the internal DndContext for advanced use‑cases.

---

## Table of Contents

1. [Overview](#overview)
2. [Installation](#installation)
3. [Quick Start](#quick-start)
4. **API Documentation**
   - [DndContext](#dndcontext)
   - [useDndContext](#usedndcontext)
   - [useDndMonitor](#usedndmonitor)
   - [Droppable](#droppable)
   - [Draggable](#draggable)
   - [Sensors](#sensors)
   - [Modifiers](#modifiers)
   - [PRESETS](#presets)
   - [Sortable](#sortable)
5. [Guides](#guides)
   - [Accessibility](#accessibility)

---

## Overview

Drag‑and‑drop functionality can be complex. **@dnd‑kit/core** provides a low‑level, highly extensible foundation that can be wrapped in higher‑level presets or integrated into custom components. The core package offers:

- A **DndContext** component that manages the drag‑and‑drop state.
- React hooks (`useDraggable`, `useDroppable`, etc.) that hook into the context.
- A set of **sensors** to handle input (mouse, touch, keyboard, etc.).
- **Collision detection** algorithms and **modifiers** for fine‑grained control.
- A robust event model for monitoring drag state.

---

## Installation

```bash
npm install @dnd-kit/core
# or
yarn add @dnd-kit/core
```

---

## Quick Start

```tsx
import { DndContext, useDraggable, useDroppable } from '@dnd-kit/core';

function DraggableItem() {
  const { attributes, listeners, setNodeRef, transform } = useDraggable({
    id: 'item-1',
  });

  return (
    <div
      ref={setNodeRef}
      style={{
        transform: transform ? `translate3d(${transform.x}px, ${transform.y}px, 0)` : undefined,
      }}
      {...attributes}
      {...listeners}
    >
      Drag me!
    </div>
  );
}

function DroppableZone() {
  const { isOver, setNodeRef } = useDroppable({ id: 'zone-1' });

  return (
    <div ref={setNodeRef} style={{ border: isOver ? '2px solid green' : '2px solid gray' }}>
      Drop here
    </div>
  );
}

function App() {
  return (
    <DndContext>
      <DraggableItem />
      <DroppableZone />
    </DndContext>
  );
}
```

---

## API Documentation

### DndContext

The top‑level component that provides the drag‑and‑drop context to its children. It accepts a number of props to configure sensors, collision detection, and event callbacks.

```tsx
import { DndContext } from '@dnd-kit/core';

function App() {
  return (
    <DndContext
      sensors={/* sensor array */}
      collisionDetection={/* collision algorithm */}
      onDragStart={/* handler */}
      onDragEnd={/* handler */}
    >
      {/* Draggable and Droppable components */}
    </DndContext>
  );
}
```

> **Why use `DndContext`?**  
> The context holds shared state such as the current drag id, collision map, and the set of active sensors. All hooks that rely on drag‑and‑drop (e.g., `useDraggable`, `useDroppable`) internally subscribe to this context.

### useDndContext

`useDndContext` exposes the raw context object for advanced usage. It is useful when creating custom presets or when you need to interact with the internal API directly.

#### Example

```tsx
import { useDndContext } from '@dnd-kit/core';

function CustomPreset() {
  const dndContext = useDndContext();

  // You now have access to:
  // - dndContext.active // the active drag id
  // - dndContext.sensor // current sensor
  // - dndContext.dropTargetId // current drop target
  // - dndContext.addEventListener(...)
  // - dndContext.removeEventListener(...)
  // - dndContext.notify(...)
}

export default CustomPreset;
```

> **When to use `useDndContext`?**  
> If you are building a new preset that must read or mutate the internal drag‑and‑drop state, or you need to listen to low‑level events that are not exposed by the high‑level hooks.

> **Contribution** – If you develop a useful preset, feel free to open a PR in the [dnd‑kit repository](https://github.com/clauderic/dnd-kit). Community contributions help keep the ecosystem vibrant.

### useDndMonitor

`useDndMonitor` lets you listen to global drag events that are not tied to a particular draggable or droppable. This is useful for analytics, logging, or custom UI feedback.

```tsx
import { useDndMonitor } from '@dnd-kit/core';

function DragTracker() {
  useDndMonitor({
    onDragStart: (event) => console.log('Drag started:', event),
    onDragEnd: (event) => console.log('Drag ended:', event),
  });

  return null; // No UI needed
}
```

### Droppable

A hook that makes a component a drop target. It returns information such as whether the current active drag is over this target.

```tsx
import { useDroppable } from '@dnd-kit/core';

function DroppableZone() {
  const { setNodeRef, isOver, setActivatorNodeRef } = useDroppable({
    id: 'zone-1',
    data: { /* custom data */ },
  });

  return (
    <div
      ref={setNodeRef}
      style={{ border: isOver ? '2px solid green' : '2px solid gray' }}
    >
      Drop here
    </div>
  );
}
```

### Draggable

A hook that makes a component draggable. It returns listeners to attach to the element, attributes for accessibility, and a ref for the drag source node.

```tsx
import { useDraggable } from '@dnd-kit/core';

function DraggableItem() {
  const { attributes, listeners, setNodeRef, transform } = useDraggable({
    id: 'item-1',
    data: { /* custom data */ },
  });

  return (
    <div
      ref={setNodeRef}
      style={{
        transform: transform
          ? `translate3d(${transform.x}px, ${transform.y}px, 0)`
          : undefined,
      }}
      {...attributes}
      {...listeners}
    >
      Drag me!
    </div>
  );
}
```

### Sensors

Sensors detect user input and translate it into drag events. Common sensors include:

- **PointerSensor** – handles mouse and touch input.
- **KeyboardSensor** – supports keyboard‑only drag‑and‑drop.
- **TouchSensor** – specialized for touch gestures.

You can create custom sensors by implementing the `Sensor` interface and passing them to the `DndContext`.

```tsx
import { PointerSensor } from '@dnd-kit/core';

<DndContext sensors={[new PointerSensor()]}>
  {/* … */}
</DndContext>
```

### Modifiers

Modifiers transform the drag payload before it is applied. Examples:

- **restrictRect** – keeps the draggable inside a bounding rectangle.
- **snapCenter** – snaps the draggable to the center of a droppable.

```tsx
import { restrictRect } from '@dnd-kit/core';

<DndContext modifiers={[restrictRect({ boundaries: { top: 0, left: 0, right: 500, bottom: 500 } })]}>
  {/* … */}
</DndContext>
```

### PRESETS

Presets are higher‑level abstractions that bundle DndContext, sensors, collision detection, and modifiers into a single component. Built‑in presets include:

- **Sortable** – drag‑and‑drop with sorting capabilities.

See the **Sortable** section below for usage examples.

### Sortable

The `Sortable` preset provides drag‑and‑drop with reorderable lists.

```tsx
import { SortableContext, useSortable, arrayMove } from '@dnd-kit/sortable';

function SortableList({ items }) {
  return (
    <SortableContext items={items}>
      {items.map((id) => (
        <SortableItem key={id} id={id} />
      ))}
    </SortableContext>
  );
}

function SortableItem({ id }) {
  const { attributes, listeners, setNodeRef, transform, transition } =
    useSortable({ id });

  return (
    <li
      ref={setNodeRef}
      style={{
        transform: transform ? `translate3d(${transform.x}px, ${transform.y}px, 0)` : undefined,
        transition,
      }}
      {...attributes}
      {...listeners}
    >
      {id}
    </li>
  );
}
```

---

## Guides

### Accessibility

- Use the `keyboardSensor` to allow keyboard‑only drag‑and‑drop.
- Set proper `aria` attributes in your draggable and droppable components.
- Provide clear visual focus states.

---

## Contributing

If you believe a preset or component you build could help others, open a pull request in the [dnd‑kit repository](https://github.com/clauderic/dnd-kit). Your contributions help keep the ecosystem healthy and useful.