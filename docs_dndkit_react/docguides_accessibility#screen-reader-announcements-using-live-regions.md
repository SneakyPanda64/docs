# Drag‑and‑Drop Accessibility Guide  
> **Source:** dnd‑kit core documentation (Accessibility section)

> **Purpose** – Provide a concise, code‑focused reference for making drag‑and‑drop interfaces
>  keyboard‑accessible, screen‑reader friendly, and fully announced via live regions.

---

## Table of Contents

1. [Introduction](#introduction)  
2. [Core Concepts](#core-concepts)  
3. [Keyboard Support](#keyboard-support)  
4. [Screen‑Reader Instructions](#screen-reader-instructions)  
5. [Live‑Region Announcements](#live-region-announcements)  
6. [Putting It All Together](#putting-it-all-together)  
7. [References](#references)

---

## Introduction

Building accessible drag‑and‑drop interactions is challenging but essential.  
The **dnd‑kit** library ships with sensible defaults that satisfy the Web
Accessibility Initiative (WAI) guidelines and give you a solid foundation to
customise for your application.

> **Read more:** *Web Almanac – Accessibility* – https://almanac.httparchive.org/en/2021/accessibility

---

## Core Concepts

When designing accessible interactions, keep these three questions in mind:

| Question | What you need to consider |
|----------|---------------------------|
| **Identity** | *Which element is the user interacting with?* |
| **Operation** | *How can the user interact with the element?* |
| **State** | *What is the current state of the element?* |

These form the basis of keyboard handling, ARIA attributes, and live‑region
announcements.

---

## Keyboard Support

| Requirement | What the library does |
|-------------|-----------------------|
| **Focusable activator** | `useDraggable` attaches `tabindex="0"` to non‑native interactive elements so they can receive focus. |
| **Activation keys** | `Enter` (Windows) / `Return` (macOS) and `Space` start a drag. |
| **Movement keys** | Arrow keys move the item. |
| **Drop keys** | `Enter` / `Return` / `Space` again to drop. |
| **Cancel key** | `Escape` cancels the drag for all sensors. |
| **Movement granularity** | Default is 25 px per arrow press; customizable via `getNextCoordinates` (used internally by `useSortable`). |

### Customising Keyboard Shortcuts

```tsx
import { DndContext, KeyboardSensor, PointerSensor, useSensor, useSensors } from '@dnd-kit/core';

const sensors = useSensors(
  useSensor(PointerSensor),
  useSensor(KeyboardSensor, {
    // Example: 50 px per arrow press
    getNextCoordinates: ({ active, offset, direction, delta }) => ({
      ...offset,
      x: offset.x + direction.x * 50,
      y: offset.y + direction.y * 50,
    }),
  })
);

<DndContext sensors={sensors} />
```

---

## Screen‑Reader Instructions

When focus lands on a draggable, the library automatically announces a set of
instructions. They are:

```
To pick up a draggable item, press space or enter.
While dragging, use the arrow keys to move the item in any given direction.
Press space or enter again to drop the item in its new position, or press escape to cancel.
```

### Customising Instructions

```tsx
<DndContext
  screenReaderInstructions={{
    activate: 'To pick up a grocery list item, press space or enter.',
    move: 'Use the up and down arrow keys to update the position of the item in the grocery list.',
    drop: 'Press space or enter again to drop the item in its new position, or press escape to cancel.',
  }}
/>
```

> **Tip:** Translate the strings if your app supports multiple locales.  
> The component ships only with English to keep bundle size small.

### ARIA Attributes Provided by `useDraggable`

| Attribute | Default | Purpose |
|-----------|---------|---------|
| `role` | `"button"` | Communicates the element is interactive. Use a semantic `<button>` if possible. |
| `aria-roledescription` | `"draggable"` | Human‑readable description for assistive tech. |
| `aria-describedby` | `DndContext-[uniqueId]` | Points to off‑screen instructions for screen readers. |

You can override any of these by passing options to `useDraggable`.

---

## Live‑Region Announcements

Live regions notify screen readers about dynamic changes without moving focus.
`<DndContext>` renders an off‑screen element for this purpose.

### Default Announcements

```ts
const defaultAnnouncements = {
  onDragStart({ active }) {
    return `Picked up draggable item ${active.id}.`;
  },
  onDragOver({ active, over }) {
    if (over) {
      return `Draggable item ${active.id} was moved over droppable area ${over.id}.`;
    }
    return `Draggable item ${active.id} is no longer over a droppable area.`;
  },
  onDragEnd({ active, over }) {
    if (over) {
      return `Draggable item ${active.id} was dropped over droppable area ${over.id}`;
    }
    return `Draggable item ${active.id} was dropped.`;
  },
  onDragCancel({ active }) {
    return `Dragging was cancelled. Draggable item ${active.id} was dropped.`;
  },
};
```

### Customising Announcements

Use position‑based descriptions rather than indices for better accessibility.

```tsx
import { DndContext } from '@dnd-kit/core';
import { useState } from 'react';

function App() {
  const [items, setItems] = useState(['Apple', 'Orange', 'Strawberries', 'Raspberries']);

  const getPosition = (id) => items.indexOf(id) + 1;
  const itemCount = items.length;

  const announcements = {
    onDragStart({ active }) {
      return `Picked up sortable item ${active.id}. Sortable item ${active.id} is in position ${getPosition(active.id)} of ${itemCount}`;
    },
    onDragOver({ active, over }) {
      if (over) {
        return `Sortable item ${active.id} was moved into position ${getPosition(over.id)} of ${itemCount}`;
      }
    },
    onDragEnd({ active, over }) {
      if (over) {
        return `Sortable item ${active.id} was dropped at position ${getPosition(over.id)} of ${itemCount}`;
      }
    },
    onDragCancel({ active }) {
      return `Dragging was cancelled. Sortable item ${active.id} was dropped.`;
    },
  };

  return (
    <DndContext accessibility={announcements}>
      {/* your sortable list here */}
    </DndContext>
  );
}
```

> **Assumption:** The `closestCenter` collision detection strategy is used, so `over` is always defined during `onDragEnd`.

---

## Putting It All Together

```tsx
import {
  DndContext,
  useSensor,
  useSensors,
  PointerSensor,
  KeyboardSensor,
} from '@dnd-kit/core';
import { useState } from 'react';

function MySortableList() {
  const [items, setItems] = useState(['Apple', 'Orange', 'Strawberries', 'Raspberries']);

  const getPosition = (id) => items.indexOf(id) + 1;
  const itemCount = items.length;

  const sensors = useSensors(
    useSensor(PointerSensor),
    useSensor(KeyboardSensor, {
      // 50px movement per arrow key
      getNextCoordinates: ({ active, offset, direction }) => ({
        ...offset,
        x: offset.x + direction.x * 50,
        y: offset.y + direction.y * 50,
      }),
    })
  );

  const announcements = {
    onDragStart({ active }) {
      return `Picked up sortable item ${active.id}. Sortable item ${active.id} is in position ${getPosition(active.id)} of ${itemCount}`;
    },
    onDragOver({ active, over }) {
      if (over) {
        return `Sortable item ${active.id} was moved into position ${getPosition(over.id)} of ${itemCount}`;
      }
    },
    onDragEnd({ active, over }) {
      if (over) {
        return `Sortable item ${active.id} was dropped at position ${getPosition(over.id)} of ${itemCount}`;
      }
    },
    onDragCancel({ active }) {
      return `Dragging was cancelled. Sortable item ${active.id} was dropped.`;
    },
  };

  return (
    <DndContext
      sensors={sensors}
      accessibility={announcements}
      screenReaderInstructions={{
        activate: 'To pick up a grocery list item, press space or enter.',
        move: 'Use the up and down arrow keys to update the position of the item in the grocery list.',
        drop: 'Press space or enter again to drop the item in its new position, or press escape to cancel.',
      }}
    >
      {/* render the sortable list using `useSortable` on each item */}
    </DndContext>
  );
}
```

---

## References

- **WAI‑ARIA Authoring Practices** – https://www.w3.org/WAI/ARIA/apg/  
- **Web Almanac – Accessibility** – https://almanac.httparchive.org/en/2021/accessibility  
- **dnd‑kit Documentation** – https://github.com/clauderic/dnd-kit

---

### Quick Checklist

- [ ] Focusable activator (`tabindex="0"` or semantic `<button>`)  
- [ ] Activation via `Enter`/`Space`  
- [ ] Movement via Arrow keys  
- [ ] Drop via `Enter`/`Space`  
- [ ] Cancel via `Escape`  
- [ ] Provide ARIA attributes (`role`, `aria-roledescription`, `aria-describedby`)  
- [ ] Customisable screen‑reader instructions  
- [ ] Live‑region announcements (position‑based)  
- [ ] Localization for multi‑language support  

Implementing these patterns will give you a drag‑and‑drop experience that is
usable by keyboard, screen‑reader users, and everyone in between.