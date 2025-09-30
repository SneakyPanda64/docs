# `useDroppable` – API Documentation

`useDroppable` is a hook from **@dnd‑kit/core** that turns a DOM node into a *droppable* area.  
Draggable elements can be dropped onto these nodes, and the hook handles collision detection and intersection logic.

> **Tip** – If you’re new to React refs, read the official React docs on [Refs](https://reactjs.org/docs/refs-and-the-dom.html) first.

---

## Getting Started

```bash
# install @dnd-kit/core
npm i @dnd-kit/core
```

```tsx
// ─────────────────────────────────────────────────────────────────────
//  Example 1 – Basic Usage
// ─────────────────────────────────────────────────────────────────────
import { useDroppable } from '@dnd-kit/core';

function Droppable() {
  const { setNodeRef } = useDroppable({
    id: 'unique-id', // <-- required unique id
  });

  return <div ref={setNodeRef}>/* Any content */</div>;
}
```

> **Requirement** – Every droppable must have a *unique `id`* and its own unique DOM node.

---

## Multiple Droppable Containers

```tsx
// ─────────────────────────────────────────────────────────────────────
//  Example 2 – Two Independent Droppables
// ─────────────────────────────────────────────────────────────────────
function MultipleDroppables() {
  const { setNodeRef: setFirstDroppableRef } = useDroppable({
    id: 'droppable-1',
  });

  const { setNodeRef: setSecondDroppableRef } = useDroppable({
    id: 'droppable-2',
  });

  return (
    <section>
      <div ref={setFirstDroppableRef}>/* First container */</div>
      <div ref={setSecondDroppableRef}>/* Second container */</div>
    </section>
  );
}
```

> **Important** – Don’t share a single ref between multiple droppable nodes.

---

## Re‑usable Droppable Component

If you need to render many droppables (e.g. from a list), create a reusable component.

```tsx
// ─────────────────────────────────────────────────────────────────────
//  Example 3 – Re‑usable Droppable
// ─────────────────────────────────────────────────────────────────────
function Droppable({ id, children }: { id: string; children: React.ReactNode }) {
  const { setNodeRef } = useDroppable({ id });

  return <div ref={setNodeRef}>{children}</div>;
}

function MultipleDroppables() {
  const droppables = ['1', '2', '3', '4'];

  return (
    <section>
      {droppables.map((id) => (
        <Droppable id={id} key={id}>
          Droppable container id: {id}
        </Droppable>
      ))}
    </section>
  );
}
```

---

## API Reference

| Property | Type | Description |
|----------|------|-------------|
| `id` | `string` (required) | Unique identifier for the droppable. |
| `data` | `any` | Optional data attached to the droppable. |
| `disabled` | `boolean` | If `true`, the droppable will ignore drag events. |
| `allowDropFromOutsideContainer` | `boolean` | When `true`, items dragged from outside the container can be dropped inside. |
| `onActive` | `(args: { active: DroppableData }) => void` | Called when an item becomes active over this droppable. |
| `onInactive` | `(args: { active: DroppableData }) => void` | Called when the active item leaves the droppable. |
| `onDrop` | `(args: { active: DroppableData, droppable: DroppableData }) => void` | Called when an item is dropped onto this droppable. |

### Returned Values

`useDroppable` returns an object with the following properties:

| Property | Type | Usage |
|----------|------|-------|
| `setNodeRef` | `(node: HTMLElement | null) => void` | Attach to the `ref` prop of the droppable DOM element. |
| `attributes` | `Record<string, string>` | Attributes to spread onto the droppable element for accessibility. |
| `listeners` | `Record<string, EventListener>` | Listeners to spread onto the droppable element for drag interactions. |
| `isOver` | `boolean` | `true` if an active draggable is hovering over this droppable. |
| `isDragging` | `boolean` | `true` if this droppable’s content is currently being dragged. |
| `isDisabled` | `boolean` | Mirrors the `disabled` option. |

> **Note** – Spread `attributes` and `listeners` onto the droppable element if you need full keyboard or pointer support.

---

## Usage Tips

- **Unique IDs** – Always provide a unique `id`. Duplicate IDs will break drag‑and‑drop logic.
- **Separate refs** – Each droppable must have its own ref; avoid reusing a single ref.
- **Dynamic lists** – Wrap `useDroppable` in a reusable component and render it inside loops or lists.
- **Accessibility** – Consider adding ARIA attributes via `attributes` for screen‑reader friendliness.

---

## Further Reading

- [DndContext](#) – Wrap your application to enable drag‑and‑drop.
- [Draggable](#) – Create draggable components that interact with droppables.
- [Sensors](#) – Configure how user input is translated into drag events.
- [Modifiers](#) – Fine‑tune drag behavior (e.g., collision detection).
- [Sorting](#) – Use the built‑in `Sortable` preset for list reordering.

---