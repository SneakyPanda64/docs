# useDroppable – Hook API Documentation

`useDroppable` is a React hook from the **@dnd-kit/core** library that turns an element into a droppable area within a `DndContext`.  
The hook returns a set of properties that let you:

- Register the element with the drag‑and‑drop state machine  
- Access the element’s layout information  
- Determine whether a draggable item is over the droppable  
- Retrieve any custom data you attach to the droppable

---

## 1. Installation

```bash
npm i @dnd-kit/core
# or
yarn add @dnd-kit/core
```

```tsx
import { DndContext, useDroppable } from '@dnd-kit/core';
```

---

## 2. Quick Start

```tsx
function MyDroppable({ id }) {
  const { setNodeRef, isOver } = useDroppable({
    id,
  });

  return (
    <div ref={setNodeRef} style={{ padding: 20, border: '1px solid black' }}>
      {isOver ? 'Drop here!' : 'Drag a node onto me'}
    </div>
  );
}

export default function App() {
  return (
    <DndContext>
      <MyDroppable id="my-droppable" />
      {/* Draggable elements go here */}
    </DndContext>
  );
}
```

---

## 3. Arguments

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| **id** | `string | number` | ✓ | Unique identifier for this droppable. No other droppable in the same `DndContext` should share the same id. |
| **disabled** | `boolean` | ✗ | Temporarily disables the droppable. Useful when a droppable should not accept drops at certain times. |
| **data** | `Record<string, any>` | ✗ | Custom data attached to the droppable. Handy for advanced use‑cases such as sortable lists or type filtering. |

> **Tip**: Because hooks cannot be called conditionally, use the `disabled` argument instead of conditionally rendering the hook.

---

## 4. Returning Properties

| Property | Type | Description |
|----------|------|-------------|
| **rect** | `React.MutableRefObject<LayoutRect | null>` | Read‑only reference to the droppable’s bounding rectangle. |
| **isOver** | `boolean` | `true` when a draggable is currently hovering over this droppable. |
| **node** | `React.RefObject<HTMLElement>` | Ref to the actual DOM node attached via `setNodeRef`. |
| **over** | `{ id: UniqueIdentifier } | null` | If another droppable is currently being hovered, contains its id. |
| **setNodeRef** | `(element: HTMLElement | null) => void` | Function that must be attached to the droppable element. |

---

## 5. Using `setNodeRef`

Attach `setNodeRef` to the element you want to become a droppable area.

```tsx
function Droppable({ id }) {
  const { setNodeRef } = useDroppable({ id });

  return (
    <div ref={setNodeRef}>
      {/* Content */}
    </div>
  );
}
```

---

## 6. Example: Using the `data` Property

### 6.1 Store the index in a sortable list

```tsx
const { setNodeRef } = useDroppable({
  id: props.id,
  data: { index: props.index },
});
```

You can then access `over.data.current.index` in event handlers or custom sensors.

### 6.2 Filter draggable types

```tsx
import {
  DndContext,
  useDraggable,
  useDroppable,
} from '@dnd-kit/core';

function Droppable() {
  const { setNodeRef } = useDroppable({
    id: 'droppable',
    data: { accepts: ['type1', 'type2'] },
  });

  return <div ref={setNodeRef}>Drop here</div>;
}

function Draggable() {
  const { attributes, listeners, setNodeRef } = useDraggable({
    id: 'draggable',
    data: { type: 'type1' },
  });

  return (
    <div {...listeners} {...attributes} ref={setNodeRef}>
      Drag me
    </div>
  );
}

export default function App() {
  const handleDragEnd = (event) => {
    const { active, over } = event;
    if (
      over &&
      over.data.current.accepts.includes(active.data.current.type)
    ) {
      console.log('Valid drop');
    }
  };

  return (
    <DndContext onDragEnd={handleDragEnd}>
      <Draggable />
      <Droppable />
    </DndContext>
  );
}
```

---

## 7. Advanced Usage

### 7.1 Accessing `over`

If you need to react when a draggable hovers over *any* droppable, check the `over` value:

```tsx
const { over } = useDroppable({ id: 'drop1' });

if (over) {
  // over.id contains the id of the hovered droppable
}
```

### 7.2 Using `rect`

For custom animations or layout calculations, read the droppable’s bounding rectangle:

```tsx
const { rect } = useDroppable({ id: 'drop1' });

if (rect.current) {
  const { width, height, top, left } = rect.current;
  // use width, height, etc.
}
```

---

## 8. Summary

| Feature | How to Enable |
|---------|---------------|
| Disable droppable | `disabled: true` |
| Attach custom data | `data: { ... }` |
| Detect hover | `isOver` |
| Identify other droppable under hover | `over` |
| Get layout metrics | `rect` |
| Register element | `ref={setNodeRef}` |

With `useDroppable`, you can easily create flexible, type‑aware droppable zones in your React application. Happy dragging!