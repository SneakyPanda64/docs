# Draggable – @dnd‑kit/core

The **`useDraggable`** hook turns any DOM node into a draggable source that can be picked up, moved and dropped over a droppable container.  
It is intentionally flexible and does not impose a particular application structure.  

---

## 1. Install

```bash
npm install @dnd-kit/core @dnd-kit/utilities
```

```tsx
import { useDraggable } from '@dnd-kit/core';
import { CSS } from '@dnd-kit/utilities';
```

---

## 2. Basic Usage

```tsx
function Draggable() {
  const {
    attributes,
    listeners,
    setNodeRef,
    transform,
  } = useDraggable({
    id: 'unique-id',
  });

  const style = {
    transform: CSS.Translate.toString(transform),
  };

  return (
    <button
      ref={setNodeRef}
      style={style}
      {...listeners}
      {...attributes}
    >
      {/* Render whatever you like within */}
    </button>
  );
}
```

> **Tip** – Always use the most semantically appropriate element (e.g. `<button>` for a clickable element).  
> Read the [Accessibility guide](#accessibility) for more on making draggable items screen‑reader friendly.

---

## 3. Hook Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | Unique identifier for this draggable. No two draggable elements may share the same `id` within a single `DndContext`. |

---

## 4. Hook Return Values

| Value | Type | Purpose |
|-------|------|---------|
| `setNodeRef` | `(node: HTMLElement | null) => void` | Attach the returned ref to the DOM node that should be tracked for collisions/intersections. |
| `listeners` | `React.HTMLAttributes<HTMLElement>` | Event handlers that must be spread onto the *activator* element (the element that starts the drag). |
| `attributes` | `React.HTMLAttributes<HTMLElement>` | Default accessibility attributes that should be spread onto the activator. |
| `transform` | `{ x: number; y: number; scaleX: number; scaleY: number; }` | Current translate/scale values. Use `CSS.Translate.toString` to turn into a CSS string. |

---

## 5. Drag Handles

You can create a separate *drag handle* from the element that is actually moved. Attach the listeners and attributes to the handle, but keep `setNodeRef` on the wrapper.

```tsx
function Draggable() {
  const { attributes, listeners, setNodeRef } = useDraggable({
    id: 'unique-id',
  });

  return (
    <div ref={setNodeRef}>
      {/* Other non‑draggable content */}
      <button {...listeners} {...attributes}>Drag handle</button>
    </div>
  );
}
```

Multiple handles are also possible:

```tsx
function Draggable() {
  const { attributes, listeners, setNodeRef } = useDraggable({
    id: 'unique-id',
  });

  return (
    <div ref={setNodeRef}>
      <button {...listeners} {...attributes}>Handle 1</button>
      {/* Non‑draggable content */}
      <button {...listeners} {...attributes}>Handle 2</button>
    </div>
  );
}
```

> **Why this pattern?**  
> It lets you use React’s synthetic event system (single listeners on the document) for better performance than attaching native listeners to every node.

---

## 6. Moving the Item

The `transform` object gives you the delta (`x`, `y`) and scale (`scaleX`, `scaleY`) values you need to apply. For the best performance, use the CSS `transform` property rather than `top/left/margin`.

```tsx
const style = {
  transform: CSS.Translate.toString(transform), // => "translate3d(10px, 20px, 0)"
};
```

> The helper `CSS.Translate.toString` simply returns `translate3d(${x}, ${y}, 0)`.  
> You can also build the string manually.

---

## 7. Accessibility Attributes

| Attribute | Default Value | Notes |
|-----------|---------------|-------|
| `role` | `"button"` | Use a native `<button>` if possible. If your element isn’t interactive, keep the role. |
| `tabIndex` | `"0"` | Enables keyboard focus for non‑interactive elements. |
| `aria-roledescription` | `"draggable"` | Customise to match your use‑case (e.g. `"moveable"`). |
| `aria-describedby` | `"DndContext-[uniqueId]"` | Links to screen‑reader instructions. |

> Attach all of these to the *activator* element (the one with the listeners).

---

## 8. `touch-action` Recommendation

Prevent unintended scrolling on touch devices:

```css
/* Apply to the drag handle or the draggable element itself */
.element {
  touch-action: none;
}
```

* **Why?**  
  - On touch devices, you can’t cancel the default scroll behaviour in pointer events.  
  - `touch-action: none` stops the browser from scrolling while dragging.  
* **Caveats**  
  - Changes to `touch-action` after a `pointerdown`/`touchstart` are ignored.  
  - For scrollable lists, set `touch-action: none` only on the handle so the list itself remains scrollable.

---

## 9. Drag Overlay (Optional)

The `<DragOverlay>` component lets you render a draggable element that is removed from normal document flow and positioned relative to the viewport. This is useful when you want the dragged item to appear above everything else.

```tsx
import { DragOverlay } from '@dnd-kit/core';

<DragOverlay>
  {active && (
    <div>
      {/* Custom overlay UI */}
    </div>
  )}
</DragOverlay>
```

See the [Drag Overlay guide](https://docs.dndkit.com/guides/drag-overlay) for deeper usage patterns.

---

## 10. Full Example

```tsx
import { DndContext, useDraggable, DragOverlay } from '@dnd-kit/core';
import { CSS } from '@dnd-kit/utilities';

function DraggableBox() {
  const { attributes, listeners, setNodeRef, transform } = useDraggable({
    id: 'box-1',
  });

  const style = {
    transform: CSS.Translate.toString(transform),
    width: 100,
    height: 100,
    background: 'steelblue',
    color: '#fff',
    display: 'flex',
    alignItems: 'center',
    justifyContent: 'center',
  };

  return (
    <div ref={setNodeRef} style={style} {...listeners} {...attributes}>
      Drag Me
    </div>
  );
}

export default function App() {
  const [active, setActive] = React.useState(null);

  return (
    <DndContext
      onDragStart={event => setActive(event.active.id)}
      onDragEnd={() => setActive(null)}
    >
      <DraggableBox />

      <DragOverlay>
        {active && (
          <div style={{ ...style, opacity: 0.8 }}>
            Dragging {active}
          </div>
        )}
      </DragOverlay>
    </DndContext>
  );
}
```

---

## 11. Accessibility Guide

- [Accessibility](https://docs.dndkit.com/guides/accessibility) – Learn best practices for screen‑reader support, keyboard interactions, and ARIA roles.  
- Customise `aria-roledescription` and provide descriptive labels where necessary.  

---

## 12. Further Reading

| Topic | Link |
|-------|------|
| Drag Overlay guide | https://docs.dndkit.com/guides/drag-overlay |
| Accessibility guide | https://docs.dndkit.com/guides/accessibility |
| Core API reference | https://docs.dndkit.com/api-core/ |

---