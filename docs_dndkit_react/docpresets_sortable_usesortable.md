# dnd‑kit – `useSortable` Hook

`useSortable` is a high‑level React hook that combines the
behaviour of `useDraggable` and `useDroppable` to make an element
sortable inside a `SortableContext`.  
It handles all the plumbing needed to:

| Feature | Description |
|---------|-------------|
| **Drag / Drop** | Supports all sensors defined in the surrounding `DndContext`. |
| **Reordering** | Automatically updates the DOM order using transforms. |
| **Accessibility** | Handles focus restoration and keyboard support. |
| **Transition** | Smoothly animates items back to their final positions. |

> **Prerequisite** – The hook must be used inside a descendant of a
> `SortableContext` provider.

---

## Installation

```bash
npm install @dnd-kit/sortable
```

```tsx
import { DndContext } from '@dnd-kit/core'
import { SortableContext, useSortable } from '@dnd-kit/sortable'
import { arrayMove } from '@dnd-kit/sortable' // optional helper
```

---

## Quick Example

```tsx
import * as React from 'react';
import { DndContext, closestCenter } from '@dnd-kit/core';
import { SortableContext, useSortable, arrayMove } from '@dnd-kit/sortable';
import { CSS } from '@dnd-kit/utilities';

const items = ['Apple', 'Banana', 'Cherry'];

function SortableItem({ id }: { id: string }) {
  const { attributes, listeners, setNodeRef, transform, transition } = useSortable({ id });

  const style: React.CSSProperties = {
    transform: CSS.Transform.toString(transform),
    transition,
    padding: '8px',
    border: '1px solid #ccc',
    marginBottom: '4px',
    background: '#fff',
    cursor: 'grab',
  };

  return (
    <li ref={setNodeRef} style={style} {...attributes} {...listeners}>
      {id}
    </li>
  );
}

export function SortableList() {
  const [list, setList] = React.useState(items);

  return (
    <DndContext
      collisionDetection={closestCenter}
      onDragEnd={({ active, over }) => {
        if (active.id !== over?.id) {
          setList((items) => {
            const oldIndex = items.indexOf(active.id);
            const newIndex = items.indexOf(over!.id);
            return arrayMove(items, oldIndex, newIndex);
          });
        }
      }}
    >
      <SortableContext items={list} strategy={sortableKeyboardCoordinates}>
        <ul>
          {list.map((item) => (
            <SortableItem key={item} id={item} />
          ))}
        </ul>
      </SortableContext>
    </DndContext>
  );
}
```

---

## API – `useSortable`

```ts
const {
  attributes,
  listeners,
  setNodeRef,
  setActivatorNodeRef,
  setDraggableNodeRef,
  setDroppableNodeRef,
  transform,
  transition,
} = useSortable(options);
```

| Property | Type | Description |
|----------|------|-------------|
| **attributes** | `React.HTMLAttributes` | Default ARIA attributes for the draggable item. |
| **listeners** | `React.DOMAttributes` | Event handlers that activate the drag. |
| **setNodeRef** | `RefCallback` | Attach to the node you wish to become *draggable* and *droppable*. |
| **setActivatorNodeRef** | `RefCallback` | (Optional) Attach to an *activator* node if it differs from the draggable node – e.g. a drag handle. |
| **setDraggableNodeRef** | `RefCallback` | (Advanced) Connect only the draggable part. |
| **setDroppableNodeRef** | `RefCallback` | (Advanced) Connect only the droppable part. |
| **transform** | `{ x: number; y: number } | null` | Current transform applied to the item during a drag. |
| **transition** | `string | null` | CSS transition string applied to the item. |

---

### Options (Arguments)

```ts
interface SortableOptions {
  id: string | number;          // unique identifier
  disabled?: boolean;           // temporarily disable the item
  transition?: null | {          // transition override
    duration?: number;          // ms
    easing?: string;            // CSS easing
  };
}
```

#### `id`

Must match the value used in the parent `SortableContext`’s `items` array.

#### `disabled`

When `true`, the item is not draggable or droppable.

#### `transition`

Defines the CSS transition to apply to the element:

```ts
transition: {
  duration: 150,                     // ms
  easing: 'cubic-bezier(0.25, 1, 0.5, 1)'
}
```

> Pass `null` to disable transitions entirely.

---

## Common Use Cases

### 1. Drag Handle

```tsx
function SortableItem({ id }: { id: string }) {
  const { attributes, listeners, setNodeRef, setActivatorNodeRef, transform, transition } =
    useSortable({ id });

  const style: React.CSSProperties = {
    transform: CSS.Transform.toString(transform),
    transition,
  };

  return (
    <li ref={setNodeRef} style={style} {...attributes}>
      <span>Item {id}</span>
      <button
        ref={setActivatorNodeRef}
        {...listeners}
        style={{ marginLeft: 'auto', cursor: 'grab' }}
      >
        Drag
      </button>
    </li>
  );
}
```

### 2. Separate Draggable / Droppable Sizes

```tsx
function SortableItem({ id }: { id: string }) {
  const { setDraggableNodeRef, setDroppableNodeRef } = useSortable({ id });

  return (
    <li ref={setDroppableNodeRef}>
      <div style={{ padding: '8px', background: '#f0f0f0' }}>
        <button ref={setDraggableNodeRef}>Drag me</button>
      </div>
    </li>
  );
}
```

---

## Transition & CSS Variables

You can let `useSortable` generate the transition string, or you can
apply it manually:

```tsx
const { transform, transition } = useSortable({ id });

const style: React.CSSProperties = {
  '--translate-x': transform?.x ?? 0,
  '--translate-y': transform?.y ?? 0,
  '--transition': transition ?? 'none',
  transform: `translate3d(var(--translate-x), var(--translate-y), 0)`,
  transition: 'var(--transition)',
};
```

---

## Accessibility

* Focus is automatically restored to the first focusable element inside the activator node when a drag is initiated via keyboard.
* If no activator node is defined, focus returns to the first focusable element inside the draggable node.

---

## Summary

`useSortable` simplifies creating sortable lists with minimal boilerplate:
* Attach `setNodeRef` to the container that should move.
* Attach `listeners` (or a separate activator) to start the drag.
* Apply `transform` and `transition` to animate the element.
* Use optional `setActivatorNodeRef`, `setDraggableNodeRef`, and `setDroppableNodeRef` for advanced layouts.

For more details, refer to the full `dnd‑kit` API docs and sensor
configuration.