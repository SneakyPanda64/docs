# `useDraggable` – Hook API (dnd‑kit)

`useDraggable` turns any DOM element into a draggable item that can be interacted with
via mouse, touch, or keyboard.  
The hook is part of the core dnd‑kit library and works inside a
`<DndContext>` provider.

---

## 1.  Arguments

```ts
interface UseDraggableArguments {
  /** Unique id – must not be reused for other draggables in the same context. */
  id: string | number;

  /** Optional ARIA attributes that are added to the activator element. */
  attributes?: {
    /** Explicit role. Default is “button”. */
    role?: string;
    /** ARIA “roledescription” (screen‑reader friendly description). */
    roleDescription?: string;
    /** Tab index. Defaults to 0 for non‑interactive elements. */
    tabIndex?: number;
  };

  /** Arbitrary data that will be attached to the draggable item and exposed
   * in events, sensors, and modifiers. */
  data?: Record<string, any>;

  /** Set to true to temporarily disable dragging. */
  disabled?: boolean;
}
```

### 1.1  Identifier (`id`)

- Must be **unique** inside the current `<DndContext>`.
- Draggable and droppable items can share the same `id` because they are stored in separate
  key‑value stores.

### 1.2  Data (`data`)

Use this to attach any extra information that you need during drag events.
Typical use‑cases:

- **Sortable lists** – store the list index.
- **Droppable matching** – store a type or list of supported types.

```tsx
const { setNodeRef } = useDraggable({
  id: props.id,
  data: { index: props.index },
});
```

```tsx
import { DndContext, useDraggable, useDroppable } from '@dnd-kit/core';

function Droppable() {
  const { setNodeRef } = useDroppable({
    id: 'droppable',
    data: { type: 'type1' },
  });
  return <div ref={setNodeRef}>Drop here</div>;
}

function Draggable() {
  const { attributes, listeners, setNodeRef, transform } = useDraggable({
    id: 'draggable',
    data: { supports: ['type1', 'type2'] },
  });
  return (
    <div ref={setNodeRef} style={{ transform: `translate(${transform.x}px, ${transform.y}px)` }}>
      <button {...listeners} {...attributes}>Drag me</button>
    </div>
  );
}

function App() {
  return (
    <DndContext onDragEnd={handleDragEnd}>
      <Droppable />
      <Draggable />
    </DndContext>
  );

  function handleDragEnd(event) {
    const { active, over } = event;
    if (over && active.data.current.supports.includes(over.data.current.type)) {
      // handle drop
    }
  }
}
```

### 1.3  Disabled (`disabled`)

Since hooks cannot be called conditionally, pass `disabled={true}` to temporarily block
dragging.

```tsx
const { setNodeRef } = useDraggable({
  id: 'foo',
  disabled: isDisabled,
});
```

### 1.4  Attributes (`attributes`)

`role`, `roleDescription`, and `tabIndex` are automatically supplied with sensible
defaults (role = “button”, tabIndex = 0).  
Only attach the attributes that make sense in your UI – for example, omit `role="button"`
on an actual `<button>` element.

---

## 2.  Return Value

```ts
{
  /** The active draggable, if any. */
  active: { id: UniqueIdentifier; node: React.MutableRefObject<HTMLElement>; rect: ViewRect } | null;

  /** Auto‑generated ARIA attributes. */
  attributes: {
    role: string;
    tabIndex: number;
    'aria-disabled': boolean;
    'aria-roledescription': string;
    'aria-describedby': string;
  };

  /** Whether this item is currently being dragged. */
  isDragging: boolean;

  /** Event listeners that must be attached to the activator element. */
  listeners: Record<SyntheticListenerName, Function> | undefined;

  /** Ref to the draggable DOM node. */
  node: React.MutableRefObject<HTMLElement | null>;

  /** The droppable that the active draggable is currently over. */
  over: { id: UniqueIdentifier } | null;

  /** Attach the draggable node. */
  setNodeRef(HTMLElement | null): void;

  /** Optional: attach an activator node (e.g. drag handle). */
  setActivatorNodeRef(HTMLElement | null): void;

  /** Current transform for the active drag. */
  transform: { x: number; y: number; scaleX: number; scaleY: number } | null;
}
```

---

## 3.  Property Details

### 3.1  `active`

- `active` contains the **id**, **node ref**, and **bounding rect** of the draggable
  element that is currently in the “dragging” state.
- When no item is being dragged, it is `null`.

### 3.2  `isDragging`

`true` only when the element you called `useDraggable` on is the one that is being dragged.

### 3.3  `listeners`

These listeners must be spread onto the element that should trigger the drag
(e.g. a drag handle).  
You can attach them to **any** DOM node; the library will still track the draggable
element properly.

#### Example: Drag handle

```tsx
function Draggable() {
  const { attributes, listeners, setNodeRef } = useDraggable({
    id: 'unique-id',
  });

  return (
    <div ref={setNodeRef}>
      <p>Content that does not start a drag.</p>
      <button {...listeners} {...attributes}>Drag handle</button>
    </div>
  );
}
```

#### Multiple handles

```tsx
function Draggable() {
  const { attributes, listeners, setNodeRef } = useDraggable({
    id: 'unique-id',
  });

  return (
    <div ref={setNodeRef}>
      <button {...listeners} {...attributes}>Drag handle 1</button>
      <button {...listeners} {...attributes}>Drag handle 2</button>
    </div>
  );
}
```

### 3.4  `setNodeRef`

Attach this to the DOM element you want to make draggable.  
It can be the same element as the activator, or a wrapper that encloses the
activator.

```tsx
const { setNodeRef } = useDraggable({ id: 'foo' });

return <button ref={setNodeRef}>Drag me</button>;
```

### 3.5  `setActivatorNodeRef`

Use this when the activator is a different element than the draggable node.
It improves focus management and helps sensors determine activation constraints.

```tsx
const { setNodeRef, setActivatorNodeRef, listeners } = useDraggable({ id: 'foo' });

return (
  <div ref={setNodeRef}>
    <button ref={setActivatorNodeRef} {...listeners}>Drag handle</button>
  </div>
);
```

### 3.6  `over`

If the active draggable is hovering over a droppable area, `over` contains that
area’s id.  
Use it to style the draggable or to perform custom logic while dragging.

### 3.7  `transform`

When an item is dragged, this object gives you the CSS translate values to apply
to the element:

```tsx
style={{ transform: `translate(${transform.x}px, ${transform.y}px)` }}
```

`scaleX` and `scaleY` can be useful when the dragged item needs to resize
in response to the droppable it is over.

---

## 4.  Usage Summary

```tsx
import { DndContext, useDraggable } from '@dnd-kit/core';

function Item({ id, label }) {
  const { attributes, listeners, setNodeRef, transform } = useDraggable({
    id,
    data: { label },
  });

  return (
    <div
      ref={setNodeRef}
      style={{
        transform: `translate(${transform?.x ?? 0}px, ${transform?.y ?? 0}px)`,
        cursor: 'grab',
      }}
    >
      <button {...listeners} {...attributes}>{label}</button>
    </div>
  );
}

function App() {
  return (
    <DndContext onDragEnd={handleDragEnd}>
      <Item id="1" label="Item 1" />
      <Item id="2" label="Item 2" />
    </DndContext>
  );
}
```

---

### Common Pitfalls

| Issue | Fix |
|-------|-----|
| **Conditional hook** | Pass `disabled={true}` instead of omitting the call. |
| **Duplicate `role`** | Omit `role="button"` on real `<button>` elements. |
| **No `tabindex`** | Use `setNodeRef` on a non‑interactive element and rely on the default `tabIndex=0`. |
| **Activator not attached** | When using a separate drag handle, call `setActivatorNodeRef` on that handle. |

---

With these guidelines you can build fully accessible, keyboard‑friendly
drag‑and‑drop interfaces that work across mouse, touch, and assistive technologies.