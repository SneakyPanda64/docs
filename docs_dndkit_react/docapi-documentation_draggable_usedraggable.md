# `useDraggable` – Hook Documentation

`useDraggable` is a React hook from **@dnd-kit/core** that turns any element into a draggable node inside a `DndContext`.  
It provides all the data, event listeners, and helper refs you need to control a draggable item while keeping accessibility and performance in mind.

---

## 1.  Arguments

```ts
interface UseDraggableArguments {
  /** Unique identifier for the draggable. */
  id: string | number;

  /** Optional ARIA attributes. */
  attributes?: {
    role?: string;            // Default: "button"
    roleDescription?: string;
    tabIndex?: number;        // Default: 0 (only for non‑native interactive elements)
  };

  /** Optional data that will be attached to the draggable element’s state. */
  data?: Record<string, any>;

  /** Temporarily disable the draggable. */
  disabled?: boolean;
}
```

- **`id`** – Must be unique among all draggables in the same `DndContext`.  
  Draggables and droppables share the same identifier namespace only if they belong to *different* stores (i.e., a draggable can share an id with a droppable).

- **`data`** – Useful for advanced scenarios such as:
  - Storing the element’s index in a sortable list.
  - Linking draggable elements to a specific droppable type.

  ```tsx
  const { setNodeRef } = useDraggable({
    id: props.id,
    data: { index: props.index }
  });
  ```

- **`disabled`** – Since hooks can’t be called conditionally, set this to `true` to temporarily prevent dragging.

---

## 2.  Return Value

```ts
{
  /** Current active draggable (null when none is being dragged). */
  active: {
    id: UniqueIdentifier;
    node: React.MutableRefObject<HTMLElement>;
    rect: ViewRect;
  } | null;

  /** ARIA attributes that should be spread onto the draggable node. */
  attributes: {
    role: string;
    tabIndex: number;
    "aria-disabled": boolean;
    "aria-roledescription": string;
    "aria-describedby": string;
  };

  /** Flag indicating if this element is the one being dragged. */
  isDragging: boolean;

  /** Synthetic event listeners that must be attached to the *activator* node. */
  listeners: Record<SyntheticListenerName, Function> | undefined;

  /** Ref to the current draggable DOM node. */
  node: React.MutableRefObject<HTMLElement | null>;

  /** Information about the droppable that the draggable is hovering over. */
  over: { id: UniqueIdentifier } | null;

  /** Attach the draggable node. */
  setNodeRef: (node: HTMLElement | null) => void;

  /** Attach the activator node (e.g. a drag handle). */
  setActivatorNodeRef: (node: HTMLElement | null) => void;

  /** Current transform applied while dragging. */
  transform: { x: number; y: number; scaleX: number; scaleY: number } | null;
}
```

---

## 3.  How to Use It

### 3.1 Basic Draggable

```tsx
import { useDraggable } from '@dnd-kit/core';

function Draggable() {
  const { setNodeRef, listeners, attributes } = useDraggable({
    id: 'my-draggable',
  });

  return (
    <button ref={setNodeRef} {...attributes} {...listeners}>
      Drag me
    </button>
  );
}
```

### 3.2 Using a Drag Handle

If you want only a specific element to start dragging:

```tsx
import { useDraggable } from '@dnd-kit/core';

function Draggable() {
  const { setNodeRef, setActivatorNodeRef, listeners, attributes } =
    useDraggable({ id: 'handle-draggable' });

  return (
    <div ref={setNodeRef}>
      <p>Content that doesn’t trigger drag.</p>

      <button
        ref={setActivatorNodeRef}
        {...listeners}
        {...attributes}
      >
        Drag Handle
      </button>
    </div>
  );
}
```

> **Tip:**  
> The `attributes` should be spread onto the same node that receives `listeners` to keep the element accessible.

### 3.3 Multiple Drag Handles

```tsx
import { useDraggable } from '@dnd-kit/core';

function Draggable() {
  const { setNodeRef, listeners, attributes } = useDraggable({
    id: 'multi-handle-draggable',
  });

  return (
    <div ref={setNodeRef}>
      <button {...listeners} {...attributes}>Handle 1</button>
      <button {...listeners} {...attributes}>Handle 2</button>
    </div>
  );
}
```

### 3.4 Advanced Use‑Case: Sortable List

```tsx
import { DndContext, useDraggable, useDroppable } from '@dnd-kit/core';

function SortableItem({ id, index }) {
  const { setNodeRef, listeners, attributes, transform } = useDraggable({
    id,
    data: { index },
  });

  const style = {
    transform: transform
      ? `translate(${transform.x}px, ${transform.y}px)`
      : undefined,
  };

  return (
    <div ref={setNodeRef} style={style} {...attributes} {...listeners}>
      Item {index}
    </div>
  );
}

function DroppableArea() {
  const { setNodeRef } = useDroppable({
    id: 'droppable',
  });

  return <div ref={setNodeRef}>Drop items here</div>;
}

export default function App() {
  const handleDragEnd = ({ active, over }) => {
    if (over) {
      console.log(`Moved ${active.id} to ${over.id}`);
      // Reorder logic here
    }
  };

  return (
    <DndContext onDragEnd={handleDragEnd}>
      <DroppableArea />
      <SortableItem id="item-1" index={1} />
      <SortableItem id="item-2" index={2} />
    </DndContext>
  );
}
```

### 3.5 Restricting Drop Targets

```tsx
import { DndContext, useDraggable, useDroppable } from '@dnd-kit/core';

function Droppable() {
  const { setNodeRef } = useDroppable({
    id: 'droppable',
    data: { type: 'type1' },
  });

  return <div ref={setNodeRef}>Drop Here (type1)</div>;
}

function Draggable() {
  const { attributes, listeners, setNodeRef, transform } = useDraggable({
    id: 'draggable',
    data: { supports: ['type1', 'type2'] },
  });

  return (
    <button ref={setNodeRef} {...attributes} {...listeners}>
      Drag me
    </button>
  );
}

export default function App() {
  return (
    <DndContext
      onDragEnd={({ active, over }) => {
        if (over && active.data.current.supports.includes(over.data.current.type)) {
          // Allowed drop
        }
      }}
    >
      <Droppable />
      <Draggable />
    </DndContext>
  );
}
```

---

## 4.  Accessibility Highlights

| Feature | What It Does | Why It Matters |
|---------|--------------|----------------|
| `role="button"` (default) | Signals that the element is actionable. | Assistive tech announces “button.” |
| `aria-roledescription` | Gives a custom description for screen readers. | E.g., “sortable product” for a sortable list. |
| `tabIndex="0"` (default for non‑native interactive elements) | Makes the element keyboard‑focusable. | Enables keyboard drag‑and‑drop. |

> If you’re using a native `<button>`, you can omit the `role` attribute because the browser already provides the correct semantics.

---

## 5.  Performance Note

`listeners` are React synthetic events, so React attaches a *single* event listener per event type to the document. This reduces the DOM event listener count dramatically compared to manually attaching listeners on each element.

---

## 6.  Summary

* **`id`** – Unique per draggable.  
* **`data`** – Optional payload for custom logic.  
* **`disabled`** – Disable dragging temporarily.  
* **`attributes` / `listeners`** – Spread onto the activator node.  
* **`setNodeRef`** – Attach to the element that should move.  
* **`setActivatorNodeRef`** – Optional, for drag handles.  
* **`active`, `over`, `isDragging`, `transform`** – Read‑only state helpers.

Follow the examples above to integrate `useDraggable` into any React application while keeping accessibility, performance, and flexibility in check. Happy dragging!