## `useDndMonitor` – Hook Documentation

The **`useDndMonitor`** hook is designed to be used inside a component that is wrapped by a `DndContext` provider.  
It allows you to listen for and react to the various drag‑and‑drop events that are emitted by that context.

---

### Import

```js
import { DndContext, useDndMonitor } from '@dnd-kit/core';
```

---

### Usage Example

```tsx
// App component that provides the DndContext
function App() {
  return (
    <DndContext>
      <Component />
    </DndContext>
  );
}

// Component that monitors DnD events
function Component() {
  // Register event listeners for the parent DndContext
  useDndMonitor({
    onDragStart(event)  { /* handle drag start */ },
    onDragMove(event)   { /* handle drag move */ },
    onDragOver(event)   { /* handle drag over */ },
    onDragEnd(event)    { /* handle drag end */ },
    onDragCancel(event) { /* handle drag cancel */ },
  });

  return null; // this component only registers listeners
}
```

---

### API

| Parameter | Type | Description |
|-----------|------|-------------|
| `onDragStart` | `(event: DragStartEvent) => void` | Callback invoked when a drag starts. |
| `onDragMove` | `(event: DragMoveEvent) => void` | Callback invoked as the dragged item moves. |
| `onDragOver` | `(event: DragOverEvent) => void` | Callback invoked when the dragged item hovers over a droppable area. |
| `onDragEnd` | `(event: DragEndEvent) => void` | Callback invoked when a drag operation finishes. |
| `onDragCancel` | `(event: DragCancelEvent) => void` | Callback invoked when a drag operation is cancelled. |

> **Note**: All events receive the same shape of event object that includes details such as the `active` draggable item, the `over` droppable target, and the current coordinates.

---

### When to Use

- **Logging**: Track drag lifecycle for analytics or debugging.
- **Custom UI**: Show tooltips or status indicators that depend on drag state.
- **Conditional Logic**: Execute side effects only when a particular drag event occurs.

---

### Further Reading

- **`DndContext`** – The top‑level provider that holds the drag‑and‑drop state.
- **`Droppable` & `Draggable`** – Components that register as drop targets and drag sources.
- **Sensors** – Plugins that determine how pointer or keyboard input triggers drags.
- **Modifiers** – Helpers to transform drag behaviour (e.g., collision detection, snapping).

---

> **Last Updated**: 4 years ago (as of the original source). The hook remains unchanged in the latest `@dnd-kit/core` releases.