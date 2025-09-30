# DragOverlay – @dnd‑kit/core

`<DragOverlay>` renders a draggable preview that is **removed from the normal
document flow** and positioned relative to the viewport.  
It’s useful when you need:

* A preview that can be styled or animated independently of the source
* The source element to unmount/move while dragging (e.g. when it moves
  between containers or inside a virtualized list)
* Smooth drop animations without writing custom logic
* Dragging inside scrollable or overflow‑clipped containers

---

## Table of contents

1.  [When to use a DragOverlay](#when-to-use-a-dragoverlay)
2.  [Basic usage](#basic-usage)
3.  [Patterns](#patterns)
    * Presentational components  
    * Wrapper nodes  
    * Ref forwarding  
    * Portals
4.  [Props](#props)
    * `adjustScale`
    * `children`
    * `className`
    * `dropAnimation`
    * `style`
    * `transition`
    * `modifiers`
    * `wrapperElement`
    * `zIndex`
5.  [Examples](#examples)

---

## 1. When to use a DragOverlay

| Scenario | Why use a DragOverlay? |
|----------|------------------------|
| **Showing a preview of where the item will land** | The preview can be positioned independently of the source while the source itself is moved. |
| **Moving items between containers** | The source element can unmount from its original container and re‑mount in the new container without affecting the overlay. |
| **Dragging inside a scrollable or overflowed container** | The overlay is positioned relative to the viewport, so it isn’t affected by scroll or clipping. |
| **Virtualized lists** | The original item can be unmounted as the list scrolls; the overlay keeps the visual continuity. |
| **Drop animations** | `<DragOverlay>` automatically plays a drop animation when `dropAnimation` is set. |

---

## 2. Basic usage

The overlay must **always remain mounted** for the drop animation to work.
You can conditionally render its **children** instead.

```tsx
import React, { useState } from 'react';
import { DndContext, DragOverlay } from '@dnd-kit/core';
import { Draggable } from './Draggable';
import Item from './Item';

export function App() {
  const [items] = useState(['1', '2', '3', '4', '5']);
  const [activeId, setActiveId] = useState<string | null>(null);

  return (
    <DndContext onDragStart={handleDragStart} onDragEnd={handleDragEnd}>
      <ScrollableList>
        {items.map(id => (
          <Draggable key={id} id={id}>
            <Item value={`Item ${id}`} />
          </Draggable>
        ))}
      </ScrollableList>

      <DragOverlay>
        {activeId ? <Item value={`Item ${activeId}`} /> : null}
      </DragOverlay>
    </DndContext>
  );

  function handleDragStart(event: any) {
    setActiveId(event.active.id);
  }

  function handleDragEnd() {
    setActiveId(null);
  }
}
```

---

## 3. Patterns

### Presentational components

Create a presentational component that’s decoupled from drag‑and‑drop logic, and a wrapper that attaches the hooks.

```tsx
// Item.tsx – presentational
import React, { forwardRef } from 'react';

export const Item = forwardRef<HTMLLIElement, { value: string }>((props, ref) => (
  <li ref={ref} {...props}>
    {props.value}
  </li>
));
```

```tsx
// DraggableItem.tsx – draggable wrapper
import { useDraggable } from '@dnd-kit/core';
import { Item } from './Item';

export function DraggableItem({ id, value }: { id: string; value: string }) {
  const { attributes, listeners, setNodeRef } = useDraggable({ id });

  return (
    <Item
      ref={setNodeRef}
      {...attributes}
      {...listeners}
      value={value}
    />
  );
}
```

Usage:

```tsx
<DraggableItem id="foo" value="Foo" />

<DragOverlay>
  {isDragging ? <Item value="Foo" /> : null}
</DragOverlay>
```

### Wrapper nodes

If you need a custom wrapper element for the draggable, you can provide it.

```tsx
import { useDraggable } from '@dnd-kit/core';
import React from 'react';

export function Draggable({ id, element = 'div', children }) {
  const { attributes, listeners, setNodeRef } = useDraggable({ id });

  const Element = element;

  return (
    <Element ref={setNodeRef} {...attributes} {...listeners}>
      {children}
    </Element>
  );
}
```

### Ref forwarding

When the presentational component already forwards its ref, you can skip
the wrapper:

```tsx
import { useDraggable } from '@dnd-kit/core';
import { Item } from './Item';

export function DraggableItem({ id, value }) {
  const { attributes, listeners, setNodeRef } = useDraggable({ id });

  return (
    <Item
      ref={setNodeRef}
      {...attributes}
      {...listeners}
      value={value}
    />
  );
}
```

### Portals

If you need the overlay in a different DOM tree (e.g., `document.body`):

```tsx
import { createPortal } from 'react-dom';
import { DragOverlay } from '@dnd-kit/core';

function App() {
  return (
    <DndContext>
      {createPortal(
        <DragOverlay>{/* children */}</DragOverlay>,
        document.body
      )}
    </DndContext>
  );
}
```

---

## 4. Props

| Prop | Type | Description |
|------|------|-------------|
| `adjustScale` | `boolean` | If `true`, the overlay will scale to match the drag sensor’s scale. |
| `children` | `React.ReactNode` | JSX to render inside the overlay. |
| `className` | `string` | CSS class applied to the wrapper element. |
| `dropAnimation` | `DropAnimation | null` | Configures the drop animation. See section below. |
| `style` | `React.CSSProperties` | Inline styles for the wrapper. |
| `transition` | `string \| TransitionGetter` | Transition used when the overlay is activated (e.g., keyboard). |
| `modifiers` | `Modifiers` | Array of functions that modify drag coordinates. |
| `wrapperElement` | `keyof JSX.IntrinsicElements` | HTML element type used to wrap the children. |
| `zIndex` | `number` | CSS `z-index`. Default is `999`. |

### DropAnimation

```ts
interface DropAnimation {
  duration: number; // milliseconds, default 250
  easing: string;   // CSS easing, default 'ease'
}
```

Example:

```tsx
<DragOverlay
  dropAnimation={{
    duration: 500,
    easing: 'cubic-bezier(0.18, 0.67, 0.6, 1.22)'
  }}
>
  {/* ... */}
</DragOverlay>
```

To disable drop animations:

```tsx
<DragOverlay dropAnimation={null}> {/* ... */} </DragOverlay>
```

---

## 5. Examples

### Restrict overlay to window bounds

```tsx
import { DndContext, DragOverlay } from '@dnd-kit/core';
import { restrictToWindowEdges } from '@dnd-kit/modifiers';

function App() {
  return (
    <DndContext>
      {/* draggable components */}
      <DragOverlay modifiers={[restrictToWindowEdges]}>
        {/* overlay content */}
      </DragOverlay>
    </DndContext>
  );
}
```

### Custom wrapper element

When dragging a list item (`<li>`), wrap the overlay in `<ul>`:

```tsx
<DragOverlay wrapperElement="ul">
  {/* list item(s) */}
</DragOverlay>
```

### Transition example

The default transition applies only when dragging via the keyboard:

```ts
function defaultTransition(activatorEvent) {
  const isKeyboard = activatorEvent instanceof KeyboardEvent;
  return isKeyboard ? 'transform 250ms ease' : undefined;
}
```

You can override it by passing a custom `transition` prop.

### z-index

If you need the overlay to appear behind other elements, set a lower z-index:

```tsx
<DragOverlay zIndex={200}> {/* ... */} </DragOverlay>
```

---

### Full example – drag a list of items with a preview overlay

```tsx
import React, { useState } from 'react';
import { DndContext, DragOverlay } from '@dnd-kit/core';
import { DraggableItem } from './DraggableItem';
import Item from './Item';

export function App() {
  const [items] = useState(['1', '2', '3']);
  const [activeId, setActiveId] = useState<string | null>(null);

  return (
    <DndContext onDragStart={handleDragStart} onDragEnd={handleDragEnd}>
      <ul>
        {items.map(id => (
          <DraggableItem key={id} id={id} value={`Item ${id}`} />
        ))}
      </ul>

      <DragOverlay>
        {activeId ? <Item value={`Item ${activeId}`} /> : null}
      </DragOverlay>
    </DndContext>
  );

  function handleDragStart(event) {
    setActiveId(event.active.id);
  }

  function handleDragEnd() {
    setActiveId(null);
  }
}
```

---

**Note**:  
*Never* conditionally render the `<DragOverlay>` component itself; always keep it mounted.  
Render its `children` conditionally instead to maintain the drop animation.