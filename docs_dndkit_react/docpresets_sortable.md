# @dnd‑kit/sortable – Documentation

> **@dnd‑kit/sortable** is a preset that builds on top of the primitives exposed by `@dnd-kit/core` to make sortable lists and grids easy to implement.  
> It adds three main concepts:  
> *`SortableContext`* – a provider that gives the sortable items a shared context.  
> *`useSortable`* – a hook that composes `useDraggable` + `useDroppable`.  
> *Sorting strategies* – algorithms that determine how the items rearrange themselves.

---

## Table of Contents

1. [Installation](#installation)
2. [Quick Start](#quick-start)
3. [API Reference](#api-reference)
   * [SortableContext](#sortablecontext)
   * [useSortable](#usesortable)
   * [Sensors](#sensors)
   * [Sorting Strategies](#sorting-strategies)
   * [Collision Detection](#collision-detection)
   * [DragOverlay](#dragoverlay)
4. [Guides & Concepts](#guides-concepts)
   * [Single Container](#single-container)
   * [Multiple Containers](#multiple-containers)
   * [Accessibility](#accessibility)
5. [Examples](#examples)

---

## 1. Installation

```bash
npm install @dnd-kit/sortable
# or
yarn add @dnd-kit/sortable
```

---

## 2. Quick Start

Below is a minimal, working example that demonstrates a vertical sortable list.

```tsx
// App.jsx
import React, { useState } from 'react';
import {
  DndContext,
  closestCenter,
  KeyboardSensor,
  PointerSensor,
  useSensor,
  useSensors,
} from '@dnd-kit/core';
import {
  arrayMove,
  SortableContext,
  sortableKeyboardCoordinates,
  verticalListSortingStrategy,
} from '@dnd-kit/sortable';
import { SortableItem } from './SortableItem';

export function App() {
  const [items, setItems] = useState(['1', '2', '3']);

  const sensors = useSensors(
    useSensor(PointerSensor),
    useSensor(KeyboardSensor, {
      coordinateGetter: sortableKeyboardCoordinates,
    })
  );

  return (
    <DndContext
      sensors={sensors}
      collisionDetection={closestCenter}
      onDragEnd={handleDragEnd}
    >
      <SortableContext
        items={items}
        strategy={verticalListSortingStrategy}
      >
        {items.map(id => (
          <SortableItem key={id} id={id} />
        ))}
      </SortableContext>
    </DndContext>
  );

  function handleDragEnd(event) {
    const { active, over } = event;

    if (active.id !== over?.id) {
      setItems(prev =>
        arrayMove(prev, prev.indexOf(active.id), prev.indexOf(over.id))
      );
    }
  }
}
```

```tsx
// SortableItem.jsx
import React from 'react';
import { useSortable } from '@dnd-kit/sortable';
import { CSS } from '@dnd-kit/utilities';

export function SortableItem({ id }) {
  const {
    attributes,
    listeners,
    setNodeRef,
    transform,
    transition,
  } = useSortable({ id });

  const style = {
    transform: CSS.Transform.toString(transform),
    transition,
  };

  return (
    <div ref={setNodeRef} style={style} {...attributes} {...listeners}>
      Item {id}
    </div>
  );
}
```

---

## 3. API Reference

### 3.1 SortableContext

```tsx
<SortableContext
  items={Array<string | number>}
  strategy={SortingStrategy}
>
  {/* Render sortable items here */}
</SortableContext>
```

* `items` – **required**. The array of unique IDs that correspond to the rendered items.  
  The order of `items` must match the order in which the items are rendered.  
* `strategy` – *optional*. Determines how items are repositioned. Default is `rectSortingStrategy`.

> **Note:** `SortableContext` must be a descendant of a `DndContext`.  
> Multiple `SortableContext`s can live under the same `DndContext` – useful for multi‑column lists.

---

### 3.2 useSortable

```tsx
const {
  attributes,
  listeners,
  setNodeRef,
  transform,
  transition,
} = useSortable({
  id: string | number,
  transition: { duration?: number, easing?: string } | null,
});
```

* `id` – **required**. The unique identifier for this sortable item.  
* `transition` – *optional*. Override the default CSS transition (`250 ms` ease).  
  Pass `null` to disable transitions.

The hook returns:

| Property | Description |
|---|---|
| `attributes` | Props to spread onto the draggable element (e.g., `role`, `aria-*`). |
| `listeners` | Keyboard and pointer event listeners. |
| `setNodeRef` | Ref setter for the root node. |
| `transform` | A `CSSTransform` object representing the item's current displacement. |
| `transition` | CSS transition string (or `null`). |

> **Tip:** If you’re using a `DragOverlay`, render a *presentational* component inside the overlay and keep `useSortable` in the list component.

---

### 3.3 Sensors

Sensors abstract the input mechanisms (`mouse`, `touch`, `keyboard`, etc.).

```tsx
import {
  DndContext,
  KeyboardSensor,
  PointerSensor,
  useSensor,
  useSensors,
} from '@dnd-kit/core';
import { sortableKeyboardCoordinates } from '@dnd-kit/sortable';

const sensors = useSensors(
  useSensor(PointerSensor),
  useSensor(KeyboardSensor, { coordinateGetter: sortableKeyboardCoordinates })
);
```

| Sensor | Description | Example options |
|---|---|---|
| `PointerSensor` | Mouse + touch combined. | — |
| `KeyboardSensor` | Arrow‑key navigation. | `coordinateGetter: sortableKeyboardCoordinates` |
| `MouseSensor` | Mouse only. | `activationConstraint: { distance: 10 }` |
| `TouchSensor` | Touch only. | `activationConstraint: { delay: 250, tolerance: 5 }` |

---

### 3.4 Sorting Strategies

```tsx
import {
  rectSortingStrategy,
  verticalListSortingStrategy,
  horizontalListSortingStrategy,
  rectSwappingStrategy,
} from '@dnd-kit/sortable';
```

| Strategy | Use case | Supports virtualized lists? |
|---|---|---|
| `rectSortingStrategy` | Default, works for most lists. | ❌ |
| `verticalListSortingStrategy` | Vertical lists, columns, grids. | ✅ |
| `horizontalListSortingStrategy` | Horizontal lists. | ✅ |
| `rectSwappingStrategy` | Swap items without moving others. | ❌ |

> **Guideline:** Pick the strategy that matches your layout; using an incompatible strategy may lead to visual glitches.

---

### 3.5 Collision Detection

```tsx
import { DndContext, closestCenter } from '@dnd-kit/core';

<DndContext collisionDetection={closestCenter}>
  {/* ... */}
</DndContext>
```

| Algorithm | When to use | Notes |
|---|---|---|
| `closestCenter` | Sortable lists (most forgiving). | Detects the nearest center of the target. |
| `closestCorners` | When you need more precise collision on corners. | |
| `rectIntersection` | Default. | Requires the draggable and droppable to intersect. |

For sortable lists, `closestCenter` or `closestCorners` are recommended.

---

### 3.6 DragOverlay

The `DragOverlay` renders a floating clone of the dragged item outside the normal document flow.

```tsx
import { DragOverlay } from '@dnd-kit/core';
import { Item } from './Item';

<DragOverlay>
  {activeId ? <Item id={activeId} /> : null}
</DragOverlay>
```

**Common pitfall** – Don’t call `useSortable` inside the overlay component.  
Instead, render a *presentational* component (`Item`) inside the overlay and keep `useSortable` in the list.

---

## 4. Guides & Concepts

### 4.1 Single Container

```tsx
<DndContext onDragEnd={handleDragEnd}>
  <SortableContext items={items} strategy={verticalListSortingStrategy}>
    {items.map(id => <SortableItem key={id} id={id} />)}
  </SortableContext>
</DndContext>
```

### 4.2 Multiple Containers

```tsx
<DndContext onDragEnd={handleDragEnd}>
  <SortableContext items={colA} strategy={verticalListSortingStrategy}>
    {colA.map(id => <SortableItem key={id} id={id} />)}
  </SortableContext>

  <SortableContext items={colB} strategy={verticalListSortingStrategy}>
    {colB.map(id => <SortableItem key={id} id={id} />)}
  </SortableContext>
</DndContext>
```

Use `onDragOver` to detect when an item is dragged over a different container and update the state accordingly.

---

### 4.3 Accessibility

`useSortable` already adds necessary ARIA attributes (`role="listitem"`, etc.).  
Keyboard navigation is enabled by the `KeyboardSensor` with the `sortableKeyboardCoordinates` getter – it moves the active item to the nearest sortable element in the pressed direction.

---

## 5. Examples

Below are concise, self‑contained examples of common scenarios.

### 5.1 Vertical List with DragOverlay

```tsx
import React, { useState } from 'react';
import {
  DndContext,
  DragOverlay,
  closestCenter,
  KeyboardSensor,
  PointerSensor,
  useSensor,
  useSensors,
} from '@dnd-kit/core';
import {
  arrayMove,
  SortableContext,
  sortableKeyboardCoordinates,
  verticalListSortingStrategy,
} from '@dnd-kit/sortable';
import { SortableItem } from './SortableItem';
import { Item } from './Item';

export function App() {
  const [activeId, setActiveId] = useState(null);
  const [items, setItems] = useState(['1', '2', '3']);

  const sensors = useSensors(
    useSensor(PointerSensor),
    useSensor(KeyboardSensor, { coordinateGetter: sortableKeyboardCoordinates })
  );

  return (
    <DndContext
      sensors={sensors}
      collisionDetection={closestCenter}
      onDragStart={handleDragStart}
      onDragEnd={handleDragEnd}
    >
      <SortableContext
        items={items}
        strategy={verticalListSortingStrategy}
      >
        {items.map(id => <SortableItem key={id} id={id} />)}
      </SortableContext>

      <DragOverlay>
        {activeId ? <Item id={activeId} /> : null}
      </DragOverlay>
    </DndContext>
  );

  function handleDragStart(event) {
    setActiveId(event.active.id);
  }

  function handleDragEnd(event) {
    const { active, over } = event;
    if (active.id !== over?.id) {
      setItems(prev =>
        arrayMove(prev, prev.indexOf(active.id), prev.indexOf(over.id))
      );
    }
    setActiveId(null);
  }
}
```

### 5.2 Multi‑Column (Two Containers) without DragOverlay

```tsx
import React, { useState } from 'react';
import { DndContext, closestCenter } from '@dnd-kit/core';
import {
  arrayMove,
  SortableContext,
  verticalListSortingStrategy,
} from '@dnd-kit/sortable';
import { SortableItem } from './SortableItem';

export function App() {
  const [colA, setColA] = useState(['1', '2']);
  const [colB, setColB] = useState(['3', '4']);

  const handleDragEnd = (event) => {
    const { active, over } = event;
    if (!over) return;

    const activeList = colA.includes(active.id) ? colA : colB;
    const overList = colA.includes(over.id) ? colA : colB;

    const activeIndex = activeList.indexOf(active.id);
    const overIndex = overList.indexOf(over.id);

    if (activeList === overList) {
      // Reorder within same column
      const newOrder = arrayMove(activeList, activeIndex, overIndex);
      activeList === colA ? setColA(newOrder) : setColB(newOrder);
    } else {
      // Move to another column
      const updatedActive = [...activeList];
      updatedActive.splice(activeIndex, 1);
      const updatedOver = [...overList];
      updatedOver.splice(overIndex, 0, active.id);

      activeList === colA ? setColA(updatedActive) : setColB(updatedActive);
      overList === colA ? setColA(updatedOver) : setColB(updatedOver);
    }
  };

  return (
    <DndContext collisionDetection={closestCenter} onDragEnd={handleDragEnd}>
      <div style={{ display: 'flex', gap: '20px' }}>
        <SortableContext items={colA} strategy={verticalListSortingStrategy}>
          {colA.map(id => <SortableItem key={id} id={id} />)}
        </SortableContext>

        <SortableContext items={colB} strategy={verticalListSortingStrategy}>
          {colB.map(id => <SortableItem key={id} id={id} />)}
        </SortableContext>
      </div>
    </DndContext>
  );
}
```

---

## 6. Resources

* **Getting Started Guide** – https://docs.dndkit.com/getting-started
* **Sensors Reference** – https://docs.dndkit.com/sensors
* **Collision Detection Algorithms** – https://docs.dndkit.com/collision-detection

---

*All examples above are fully functional. Replace the placeholder `Item` component with any presentational markup you wish to render in the `DragOverlay`.*