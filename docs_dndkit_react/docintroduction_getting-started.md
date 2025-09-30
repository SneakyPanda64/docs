# @dnd-kit – Quick‑Start Documentation

> **⚠️** This document is a cleaned‑up, English‑only version of the original quick‑start guide.  
> It keeps all example code, but removes any unnecessary clutter.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Installation](#installation)
3. [Core Concepts](#core-concepts)
   * [Context Provider](#context-provider)
   * [Droppable](#droppable)
   * [Draggable](#draggable)
4. [Putting It All Together](#assembling-all-the-pieces)
5. [More Complex Scenarios](#pushing-things-a-bit-further)
6. [References](#references)

---

## Prerequisites

```bash
# Assuming you are using npm
npm install @dnd-kit/core @dnd-kit/utilities
```

---

## Installation

See the **Installation** guide in the repository for detailed steps (Node 18+, React 18+).

---

## Core Concepts

### Context Provider

All drag‑and‑drop hooks (`useDraggable`, `useDroppable`) work only when the components that use them are wrapped in a `<DndContext>`.

```tsx
// App.jsx
import React from 'react';
import { DndContext } from '@dnd-kit/core';
import { Draggable } from './Draggable';
import { Droppable } from './Droppable';

export default function App() {
  return (
    <DndContext>
      <Draggable />
      <Droppable />
    </DndContext>
  );
}
```

### Droppable

A droppable area accepts a unique `id` and a `ref` to the DOM node.  
It also receives an `isOver` flag that becomes `true` when a draggable element is hovered over it.

```tsx
// Droppable.jsx
import React from 'react';
import { useDroppable } from '@dnd-kit/core';

export function Droppable({ children }) {
  const { isOver, setNodeRef } = useDroppable({ id: 'droppable' });

  const style = {
    color: isOver ? 'green' : undefined,
  };

  return (
    <div ref={setNodeRef} style={style}>
      {children}
    </div>
  );
}
```

### Draggable

A draggable element needs a unique `id`, a `ref`, and listeners/attributes from `useDraggable`.  
It receives a `transform` object containing `x`, `y`, `scaleX`, and `scaleY`, which you use to move the element.

```tsx
// Draggable.jsx
import React from 'react';
import { useDraggable } from '@dnd-kit/core';
import { CSS } from '@dnd-kit/utilities';

export function Draggable({ children }) {
  const { attributes, listeners, setNodeRef, transform } = useDraggable({
    id: 'draggable',
  });

  const style = transform
    ? { transform: CSS.Translate.toString(transform) }
    : undefined;

  return (
    <button
      ref={setNodeRef}
      style={style}
      {...listeners}
      {...attributes}
    >
      {children}
    </button>
  );
}
```

**Performance tip:** Use `transform` (not `top/left`) for smooth movement and consider raising the element’s `z‑index` while dragging.

---

## Assembling All The Pieces

Add `onDragEnd` to the `<DndContext>` to respond to drop events.  
Below we move a single draggable from outside into a droppable container.

```tsx
// App.jsx
import React, { useState } from 'react';
import { DndContext } from '@dnd-kit/core';
import { Droppable } from './Droppable';
import { Draggable } from './Draggable';

export default function App() {
  const [isDropped, setIsDropped] = useState(false);
  const draggableMarkup = <Draggable>Drag me</Draggable>;

  return (
    <DndContext onDragEnd={handleDragEnd}>
      {!isDropped && draggableMarkup}
      <Droppable>
        {isDropped ? draggableMarkup : 'Drop here'}
      </Droppable>
    </DndContext>
  );

  function handleDragEnd(event) {
    if (event.over && event.over.id === 'droppable') {
      setIsDropped(true);
    }
  }
}
```

---

## Pushing Things a Bit Further

A common scenario: multiple droppable containers that can accept the same draggable.

```tsx
// App.jsx
import React, { useState } from 'react';
import { DndContext } from '@dnd-kit/core';
import { Droppable } from './Droppable';
import { Draggable } from './Draggable';

export default function App() {
  const containers = ['A', 'B', 'C'];
  const [parent, setParent] = useState<string | null>(null);
  const draggableMarkup = <Draggable id="draggable">Drag me</Draggable>;

  return (
    <DndContext onDragEnd={handleDragEnd}>
      {parent === null && draggableMarkup}
      {containers.map((id) => (
        <Droppable key={id} id={id}>
          {parent === id ? draggableMarkup : 'Drop here'}
        </Droppable>
      ))}
    </DndContext>
  );

  function handleDragEnd(event) {
    const { over } = event;
    setParent(over ? over.id : null);
  }
}
```

> **Note:** Ensure your `Droppable` component accepts an `id` prop and forwards it to `useDroppable`.

---

## References

- **@dnd-kit/core** – `DndContext`, `useDraggable`, `useDroppable`, etc.
- **@dnd-kit/utilities** – `CSS.Translate.toString`.

Explore the full API docs for more options (modifiers, sensors, drag overlay, presets, etc.).

---