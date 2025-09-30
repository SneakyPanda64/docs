# @dnd-kit – A Lightweight, Modular, Performance‑Optimised Drag & Drop Toolkit for React

`@dnd-kit` is a **React‑only** drag & drop library that gives you full control over the behaviour, style and accessibility of draggable and droppable elements.  
It ships with a tiny core (≈10 KB minified, no external dependencies) and a rich set of extension points so you can build anything from simple lists to complex 2‑D games.

> *All code examples are written in JSX/JavaScript and are meant to be copied‑and‑pasted directly into a React project.*

---

## Table of Contents

1. [Installation](#installation)
2. [Quick Start](#quick-start)
3. [Core API](#core-api)
   - [`<DndContext>`](#dndcontext)
   - [`useDraggable`](#usedraggable)
   - [`useDroppable`](#usedroppable)
   - [`<DragOverlay>`](#dragoverlay)
4. [Extensibility](#extensibility)
   - [Sensors](#sensors)
   - [Modifiers](#modifiers)
   - [Custom Collision Detection](#custom-collision-detection)
5. [Accessibility](#accessibility)
6. [Architecture](#architecture)
7. [Performance](#performance)
8. [Presets](#presets)
9. [References & Further Reading](#references)

---

## 1. Installation

```bash
# npm
npm install @dnd-kit/core

# yarn
yarn add @dnd-kit/core
```

> For sortable lists, install the preset:  
> `npm install @dnd-kit/sortable` (or `yarn add @dnd-kit/sortable`).

---

## 2. Quick Start

Below is a minimal example that demonstrates:

- A draggable item
- A droppable container
- A draggable overlay that follows the cursor
- Keyboard support (via the built‑in pointer, mouse, touch, and keyboard sensors)

```tsx
import * as React from 'react';
import {
  DndContext,
  useDraggable,
  useDroppable,
  DragOverlay,
} from '@dnd-kit/core';

function DraggableItem() {
  const { attributes, listeners, setNodeRef, transform } = useDraggable({
    id: 'draggable-1',
  });

  const style = {
    transform: transform ? `translate3d(${transform.x}px, ${transform.y}px, 0)` : undefined,
    cursor: 'grab',
  };

  return (
    <div ref={setNodeRef} style={style} {...listeners} {...attributes}>
      Drag me
    </div>
  );
}

function DroppableArea() {
  const { setNodeRef, isOver } = useDroppable({
    id: 'droppable-1',
  });

  return (
    <div
      ref={setNodeRef}
      style={{
        padding: '1rem',
        backgroundColor: isOver ? '#e0ffe0' : '#fff',
        border: '2px dashed #aaa',
      }}
    >
      Drop here
    </div>
  );
}

export default function App() {
  const [activeId, setActiveId] = React.useState<string | null>(null);

  return (
    <DndContext
      collisionDetection={closestCenter}
      onDragStart={(event) => setActiveId(event.active.id)}
      onDragEnd={() => setActiveId(null)}
    >
      <DroppableArea />
      <DraggableItem />

      {/* The overlay will render *outside* of the normal DOM flow */}
      <DragOverlay>
        {activeId ? (
          <div style={{ padding: '0.5rem', background: '#fafafa', border: '1px solid #ddd' }}>
            {activeId}
          </div>
        ) : null}
      </DragOverlay>
    </DndContext>
  );
}
```

> **Tip:** The `transform` returned by `useDraggable` is only updated while an item is being dragged. If you prefer to update the DOM directly during the drag, you can also manipulate the element’s style inside the `onDragMove` callback.

---

## 3. Core API

### `<DndContext>`

The top‑level provider that holds the drag & drop state.  
It accepts a number of props that control behaviour:

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `collisionDetection` | `CollisionDetection` | `closestCenter` | Algorithm to compute potential drop targets. |
| `sensors` | `Sensors` | built‑in pointer, mouse, touch, keyboard | List of sensor instances that trigger drag events. |
| `onDragStart` | `Callback` | – | Invoked when dragging starts. |
| `onDragMove` | `Callback` | – | Invoked on every frame while dragging. |
| `onDragEnd` | `Callback` | – | Invoked when the drag ends. |
| `onDragCancel` | `Callback` | – | Invoked if a drag is cancelled. |

---

#### `useDraggable`

```tsx
const {
  attributes,
  listeners,
  setNodeRef,
  transform,
  isDragging,
} = useDraggable({
  id: string,
  data?: any,        // any data you wish to associate
  disabled?: boolean
});
```

* `attributes` – Spread on the root element to expose `aria-*` attributes.  
* `listeners` – Spread on the root element to receive pointer/touch/keyboard events.  
* `setNodeRef` – Ref callback to register the element.  
* `transform` – Current drag offset (`{x, y}`).  
* `isDragging` – Boolean flag that indicates if this element is currently being dragged.

---

#### `useDroppable`

```tsx
const { setNodeRef, isOver, active } = useDroppable({
  id: string,
  data?: any,
  disabled?: boolean
});
```

* `setNodeRef` – Register the droppable container.  
* `isOver` – `true` if a draggable element is hovering.  
* `active` – The `Draggable` object that is currently over this droppable.

---

#### `<DragOverlay>`

Renders a “ghost” of the dragged element that follows the cursor and is removed from the normal document flow.  
It should be a direct child of `<DndContext>`.

```tsx
<DragOverlay>
  {activeId && <YourCustomOverlay />}
</DragOverlay>
```

When the overlay component is `null`, the overlay is hidden.

---

## 4. Extensibility

`@dnd-kit` was built around the idea that you only ship the features you need.  
All core functionality is exposed as extension points.

### Sensors

Sensors translate low‑level input events into the drag‑drop lifecycle.

* Built‑in sensors: `PointerSensor`, `KeyboardSensor`, `TouchSensor`, `MouseSensor`.  
* Custom sensors can be written by extending the `Sensor` interface.

**Example – custom sensor that triggers on a long‑press**

```tsx
import { useSensor, useSensorOptions, Sensor } from '@dnd-kit/core';

class LongPressSensor extends Sensor {
  static id = 'long-press';

  constructor(options: useSensorOptions) {
    super(options);
    this.onTouchStart = this.onTouchStart.bind(this);
  }

  activate({ id }) {
    this.id = id;
    window.addEventListener('touchstart', this.onTouchStart, { passive: false });
  }

  deactivate() {
    window.removeEventListener('touchstart', this.onTouchStart);
  }

  onTouchStart(event) {
    // start a timer, if held > 500ms emit dragStart, otherwise ignore
  }
}
```

Add it to `<DndContext>`:

```tsx
<DndContext sensors={[pointerSensor, new LongPressSensor()]}>...</DndContext>
```

---

### Modifiers

Modifiers adjust the position of a draggable item after each move.  
Built‑in modifiers include:

* `restrictToParentElement`
* `restrictToVerticalAxis`
* `restrictToHorizontalAxis`
* `snapCenter`

**Example – constrain to a bounding rectangle**

```tsx
import { restrictToBoundingBox } from '@dnd-kit/modifiers';

<DndContext
  modifiers={[restrictToBoundingBox({ left: 0, top: 0, right: 200, bottom: 200 })]}
>
  ...
</DndContext>
```

---

### Custom Collision Detection

`@dnd-kit` ships with several algorithms (`closestCenter`, `pointerWithin`, `closestCorner`).  
You can write your own by implementing the `CollisionDetection` interface.

```tsx
import { CollisionDetection, Collision, Over } from '@dnd-kit/core';

function myCustomCollisionDetection( { droppableContainers, active }) : Collision[] {
  // Your logic
  return [{ id: 'drop-1', intersection: 0.8 }];
}
```

Pass it to `<DndContext>`:

```tsx
<DndContext collisionDetection={myCustomCollisionDetection}>...</DndContext>
```

---

## 5. Accessibility

`@dnd-kit` prioritises accessible drag & drop.

* **Keyboard support** – Arrow keys, `Space/Enter` to pick up, `Esc` to cancel.  
* **ARIA attributes** – Automatically added to draggable elements (`role="draggable"`, `aria-grabbed`, etc.).  
* **Screen‑reader announcements** – Live region updates when dragging starts, ends or an item is dropped.  
* **Customisable instructions** – Provide your own `aria-label` or `role="button"` if the default is unsuitable.

**Example – overriding screen‑reader text**

```tsx
<DndContext
  screenReaderInstructions={{
    dragging: 'You are dragging a draggable item',
    dropped: 'Item dropped',
  }}
>
  ...
</DndContext>
```

---

## 6. Architecture

Unlike libraries that rely on the HTML5 Drag & Drop API, `@dnd-kit` implements its own drag logic entirely in JavaScript.  

**Why not use HTML5?**  

* **Touch support** – HTML5 drag & drop fails on touch devices.  
* **Custom preview** – Native API offers limited control over drag preview.  
* **Animation & constraints** – Easier to implement with a pure‑JS approach.  

**Trade‑off:**  
You cannot drag between windows or from desktop to browser. For such use‑cases, consider a library built on the HTML5 API (e.g., `react-dnd`).

---

## 7. Performance

Performance is a core principle.  Here’s how `@dnd-kit` keeps things fast:

1. **Lazy calculations** – Positions of droppable containers are only measured at drag start.  
2. **CSS transforms** – Items are moved with `transform: translate3d(...)` to avoid re‑painting.  
3. **Synthetic events** – Sensors use synthetic listeners that are attached once to the document, rather than per draggable element.  
4. **Minimal DOM mutation** – During drag we rarely touch the DOM; we compute new positions purely in JS.

**Tip** – Avoid complex layout changes in `onDragMove` callbacks; if you need to update the DOM, debounce or batch the changes.

---

## 8. Presets

Presets are thin layers that expose higher‑level behaviour built on top of the core.

| Preset | What it adds |
|--------|--------------|
| `@dnd-kit/sortable` | Sortable lists, grid reordering, auto‑scroll, drag handles, animation hooks |
| `@dnd-kit/grid` (coming soon) | Fixed‑grid drag & drop |
| `@dnd-kit/virtual` (planned) | Virtualized lists support |

**Example – simple sortable list**

```tsx
import { SortableContext, arrayMove, useSortable } from '@dnd-kit/sortable';
import { CSS } from '@dnd-kit/utilities';

function SortableItem({ id }) {
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

export default function SortableList() {
  const [items, setItems] = React.useState(['a', 'b', 'c', 'd']);

  function handleDragEnd(event) {
    const { active, over } = event;
    if (active.id !== over?.id) {
      setItems((items) => arrayMove(items, items.indexOf(active.id), items.indexOf(over!.id)));
    }
  }

  return (
    <DndContext onDragEnd={handleDragEnd}>
      <SortableContext items={items} strategy={verticalListSortingStrategy}>
        {items.map((id) => <SortableItem key={id} id={id} />)}
      </SortableContext>
    </DndContext>
  );
}
```

---

## 9. References & Further Reading

* Official docs: https://docs.dndkit.com  
* API reference (auto‑generated): https://docs.dndkit.com/api-reference  
* Accessibility guide: https://docs.dndkit.com/accessibility  
* Performance guide: https://docs.dndkit.com/performance  
* Preset examples: https://github.com/clauderic/dnd-kit/tree/main/packages/sortable/src/examples

---

**Enjoy building smooth, accessible, and highly‑customisable drag & drop interfaces with `@dnd-kit`!**