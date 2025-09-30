# SortableContext – Drag & Drop Sortable List Helper

`SortableContext` is a React component that supplies the context needed by the `useSortable` hook.  
It tells the hook which items are sortable, in which order, and how to calculate the transform for each item during a drag operation.

---

## Props

| Prop      | Type              | Required | Description |
|-----------|-------------------|----------|-------------|
| **items** | `Array<string|number>` | ✅ | An *array of unique identifiers* that represent the items rendered inside this context. The array must be **sorted in the same order** that the items are rendered; otherwise the sort behavior will be unpredictable. |
| **strategy** | `SortingStrategy` | ❌ | A function that returns the transform and transition values for an item. The library ships with several built‑in strategies (see below). The default is `rectSortingStrategy`. |
| **id** | `string` | ❌ | An optional ID for the context. If omitted, one will be auto‑generated. Useful when you need to expose the container ID to custom sensors or when nesting multiple contexts. |

### Built‑in Sorting Strategies

| Strategy | Use Case | Notes |
|----------|----------|-------|
| `rectSortingStrategy` | Default | Works for most lists. Does **not** support virtualised lists. |
| `verticalListSortingStrategy` | Vertical lists | Supports virtualised lists. |
| `horizontalListSortingStrategy` | Horizontal lists | Supports virtualised lists. |
| `rectSwappingStrategy` | Swappable elements | Useful when you want the dragged item to swap places with others. |

You can also create your own custom strategy. It must accept the same arguments that built‑in strategies receive and return the expected values.

---

## Basic Example

```tsx
import React, { useState } from 'react';
import { DndContext } from '@dnd-kit/core';
import { SortableContext } from '@dnd-kit/sortable';

function App() {
  const [items] = useState([1, 2, 3]);

  return (
    <DndContext>
      <SortableContext items={items}>
        {/* Your sortable items go here */}
      </SortableContext>
    </DndContext>
  );
}
```

*Notice: `items` is sorted the same way the list is rendered.*

---

## Nesting Rules

### 1. Must be a descendant of a `<DndContext>`

```tsx
// ❌ Bad – missing parent
<SortableContext>
  {/* ... */}
</SortableContext>
```

```tsx
// ✅ Good
<DndContext>
  <SortableContext>
    {/* ... */}
  </SortableContext>
</DndContext>
```

### 2. Multiple sibling contexts

```tsx
<DndContext>
  <SortableContext>
    {/* list 1 */}
  </SortableContext>
  <SortableContext>
    {/* list 2 */}
  </SortableContext>
</DndContext>
```

### 3. Nested `<DndContext>`s

```tsx
<DndContext>
  <SortableContext items={['A', 'B', 'C']}>
    <DndContext>
      <SortableContext items={['A', 'B', 'C']}>
        {/* nested list */}
      </SortableContext>
    </DndContext>
  </SortableContext>
</DndContext>
```

### 4. Avoid ID collisions

```tsx
// ❌ Bad – duplicate `id` (implicit or explicit)
<DndContext>
  <SortableContext items={['A', 'B', 'C']}>
    <SortableContext items={['A', 'B', 'C']}>
      {/* ... */}
    </SortableContext>
  </SortableContext>
</DndContext>
```

```tsx
// ✅ Good – unique IDs
<DndContext>
  <SortableContext items={['A', 'B', 'C']} id="list-abc">
    <SortableContext items={[1, 2, 3]} id="list-123">
      {/* ... */}
    </SortableContext>
  </SortableContext>
</DndContext>
```

---

## When to Use a Custom Strategy

If your UI requires a non‑standard ordering algorithm (e.g., grid layout, circular arrangement, or a specialized virtualized list), implement a strategy that adheres to the signature:

```tsx
function customStrategy(
  active: Active,
  over: Over | null,
  items: Array<string | number>,
  rects: Map<string | number, DOMRect>,
  ...additionalArgs
): ReturnType
```

Return the same shape as the built‑in strategies: an object with `transform`, `transition`, and optional `offset` data for each sortable item.

---

## Summary

- Wrap sortable items in `<SortableContext>` inside a `<DndContext>`.
- Pass a **sorted** array of unique IDs via the `items` prop.
- Pick an appropriate strategy or provide your own.
- Ensure unique context IDs when nesting to avoid conflicts.

These guidelines guarantee predictable drag‑and‑drop sorting behaviour across your React application.