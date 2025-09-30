# dnd‑kit – Accessible Drag‑and‑Drop for React

> A lightweight, hook‑centric library that makes building accessible drag‑and‑drop interfaces fast and easy.

---

## Table of Contents

| Section | Description |
|---------|-------------|
| [Overview](#overview) | What the library does and why it matters |
| [Installation & Quick Start](#installation--quick-start) | Get up & running in seconds |
| [API Reference](#api-reference) | `DndContext`, `Draggable`, `Droppable`, `useSortable`, … |
| [Accessibility Guide](#accessibility-guide) | Keyboard support, screen‑reader instructions, live‑region announcements |
| [Examples](#examples) | Working demos that you can copy & paste |
| [Customisation](#customisation) | Tailor keyboard behaviour, announcements, localisation |
| [FAQ & Troubleshooting](#faq--troubleshooting) | Common questions & answers |

---

## Overview

`dnd-kit` provides a **fully keyboard‑accessible** drag‑and‑drop experience out‑of‑the‑box.  
It follows the three pillars of accessibility:

1. **Identity** – *What element is the user interacting with?*  
2. **Operation** – *How can the user interact with that element?*  
3. **State** – *What is the current state of the element?*

The library ships with sensible defaults that you can later override, but the goal is to give you a solid, accessible starting point.

---

## Installation & Quick Start

```bash
# npm
npm install @dnd-kit/core @dnd-kit/sortable

# yarn
yarn add @dnd-kit/core @dnd-kit/sortable
```

```tsx
// App.tsx
import { DndContext } from '@dnd-kit/core';
import { useSortable } from '@dnd-kit/sortable';
import { CSS } from '@dnd-kit/utilities';

function SortableItem({ id, children }) {
  const { attributes, listeners, setNodeRef, transform, transition } =
    useSortable({ id });

  const style = {
    transform: CSS.Transform.toString(transform),
    transition,
    border: '1px solid #ccc',
    padding: '8px',
    marginBottom: 4,
    background: '#fff',
  };

  return (
    <div ref={setNodeRef} style={style} {...attributes} {...listeners}>
      {children}
    </div>
  );
}

export default function App() {
  const [items, setItems] = useState(['Apple', 'Orange', 'Strawberry', 'Raspberry']);

  const handleDragEnd = (event) => {
    const {active, over} = event;
    if (over && active.id !== over.id) {
      const oldIndex = items.indexOf(active.id);
      const newIndex = items.indexOf(over.id);
      setItems((prev) => arrayMove(prev, oldIndex, newIndex));
    }
  };

  return (
    <DndContext onDragEnd={handleDragEnd}>
      {items.map((item) => (
        <SortableItem key={item} id={item}>
          {item}
        </SortableItem>
      ))}
    </DndContext>
  );
}
```

> **Tip** – The `DndContext` automatically enables the keyboard sensor and provides live‑region announcements and screen‑reader instructions.

---

## API Reference

### `DndContext`

| Prop | Type | Description |
|------|------|-------------|
| `children` | `ReactNode` | Elements that may contain drag‑able or drop‑able items. |
| `onDragStart` | `EventHandler` | Called when a drag starts. |
| `onDragEnd` | `EventHandler` | Called when a drag ends. |
| `onDragCancel` | `EventHandler` | Called when a drag is cancelled. |
| `onDragOver` | `EventHandler` | Called while a draggable is over a drop zone. |
| `accessibility` | `AccessibilityOptions` | Override default announcements or screen‑reader instructions. |
| `sensors` | `Sensor[]` | Custom sensors (e.g., `KeyboardSensor`, `TouchSensor`). |

**Default Accessibility Options**

```ts
const defaultAccessibility = {
  screenReaderInstructions: 'To pick up a draggable item, press space or enter. While dragging, use the arrow keys to move the item in any direction. Press space or enter again to drop the item in its new position, or press escape to cancel.',
  announcements: {
    onDragStart({ active }) { return `Picked up draggable item ${active.id}.`; },
    onDragOver({ active, over }) {
      if (over) return `Draggable item ${active.id} was moved over droppable area ${over.id}.`;
      return `Draggable item ${active.id} is no longer over a droppable area.`;
    },
    onDragEnd({ active, over }) {
      if (over) return `Draggable item ${active.id} was dropped over droppable area ${over.id}`;
      return `Draggable item ${active.id} was dropped.`;
    },
    onDragCancel({ active }) { return `Dragging was cancelled. Draggable item ${active.id} was dropped.`; }
  }
}
```

---

### `useDraggable`

```ts
const {attributes, listeners, setNodeRef, transform, transition, isDragging} = useDraggable({
  id: string | number,
  data?: any,
  disabled?: boolean,
  activationConstraint?: ActivationConstraint,
  role?: string,
  ariaLabel?: string,
  ariaDescribedby?: string,
  ariaLabelledby?: string,
  ariaDescribedbyDefault?: string,
  ariaLabelDefault?: string,
  // ...more
});
```

| Option | Default | Note |
|--------|---------|------|
| `role` | `"button"` | Prefer semantic `<button>` where possible. |
| `aria-roledescription` | `"draggable"` | Localise for your app. |
| `aria-describedby` | `"DndContext-[uniqueId]"` | Points to screen‑reader instructions. |
| `tabIndex` | `0` (auto‑set when element isn’t naturally focusable) | Ensures keyboard focus. |

---

### `useSortable`

A convenience hook that extends `useDraggable` & `useDroppable` for lists.

```ts
const { attributes, listeners, setNodeRef, transform, transition, isDragging, isSorting } = useSortable({
  id: string | number,
  disabled?: boolean,
  // ...
});
```

`useSortable` internally uses a keyboard sensor that moves to the next sortable item when arrow keys are pressed.

---

### `Droppable`

```ts
const { setNodeRef, isOver, dropAnimation, active } = useDroppable({
  id: string | number,
  data?: any,
  // ...
});
```

| Option | Default |
|--------|---------|
| `id` | required |
| `data` | `undefined` |

---

### `Sensor` (Keyboard, Mouse, Touch, Pointer)

Sensors determine how a user initiates drag operations.  
`KeyboardSensor` is enabled by default in `DndContext`.

```ts
import { KeyboardSensor, MouseSensor, TouchSensor } from '@dnd-kit/core';

const sensors = [
  new MouseSensor(),
  new TouchSensor(),
  new KeyboardSensor()
];
```

---

## Accessibility Guide

### Keyboard Support

| Action | Key | Notes |
|--------|-----|-------|
| Pick up | `Space` / `Enter` | Initiates drag on focus. |
| Move | Arrow keys | Moves in 25‑px steps (customisable). |
| Drop | `Space` / `Enter` | Releases the item. |
| Cancel | `Escape` | Aborts the drag operation. |

> The library automatically sets `tabIndex="0"` on draggable activators that are not natively focusable.

---

### Screen‑Reader Instructions

`<DndContext>` renders an off‑screen element that contains the default instructions.  
You can customise them via `accessibility.screenReaderInstructions`.

```tsx
<DndContext
  accessibility={{
    screenReaderInstructions:
      'To pick up a grocery list item, press space or enter. Use the up and down arrow keys to update the position of the item in the grocery list. Press space or enter again to drop the item, or press escape to cancel.'
  }}
>
  {/* ... */}
</DndContext>
```

---

### Live‑Region Announcements

The library automatically announces drag events.  
Customise the messages using `accessibility.announcements`.

```tsx
const customAnnouncements = {
  onDragStart({ active }) {
    return `Picked up sortable item ${active.id}.`;
  },
  onDragOver({ active, over }) {
    if (over) return `Sortable item ${active.id} was moved into position ${getPosition(over.id)} of ${itemCount}.`;
  },
  onDragEnd({ active, over }) {
    if (over) return `Sortable item ${active.id} was dropped at position ${getPosition(over.id)} of ${itemCount}.`;
  },
  onDragCancel({ active }) {
    return `Dragging was cancelled. Sortable item ${active.id} was dropped.`;
  }
};

<DndContext accessibility={customAnnouncements}>{/* ... */}</DndContext>
```

> **Tip** – Use *positions* (e.g., “position 3 of 5”) instead of indices (“index 2”) for natural‑language announcements.

---

## Examples

### 1️⃣ Simple Drag & Drop

```tsx
<DndContext>
  <div
    ref={setNodeRef}
    {...attributes}
    {...listeners}
    style={{ background: isDragging ? '#e0f7fa' : '#fff' }}
  >
    Drag Me!
  </div>
</DndContext>
```

### 2️⃣ Sortable List

```tsx
function SortableList() {
  const [items, setItems] = useState(['Apple', 'Orange', 'Strawberry', 'Raspberry']);

  const handleDragEnd = (event) => {
    const { active, over } = event;
    if (over && active.id !== over.id) {
      setItems((prev) => arrayMove(prev, items.indexOf(active.id), items.indexOf(over.id)));
    }
  };

  return (
    <DndContext onDragEnd={handleDragEnd}>
      {items.map((item) => (
        <SortableItem key={item} id={item}>
          {item}
        </SortableItem>
      ))}
    </DndContext>
  );
}
```

### 3️⃣ Drag‑and‑Drop With Custom Keyboard Sensor

```tsx
const customKeyboardSensor = new KeyboardSensor({
  // Move 10px per arrow key press instead of 25px
  getNextCoordinates: (event) => {
    const { direction } = event;
    return direction === 'right' ? { x: 10, y: 0 } : { x: 0, y: 10 };
  }
});

<DndContext sensors={[customKeyboardSensor, new MouseSensor()]}>
  {/* ... */}
</DndContext>
```

---

## Customisation

| Feature | Default | How to override |
|---------|---------|-----------------|
| Keyboard movement step | 25 px | Pass `getNextCoordinates` to `KeyboardSensor` |
| Screen‑reader instructions | English | `accessibility.screenReaderInstructions` |
| Live‑region announcements | Simple English messages | `accessibility.announcements` |
| Locale | `en` | Provide translated strings in the above props |

---

## FAQ & Troubleshooting

| Question | Answer |
|----------|--------|
| **Why does my draggable not receive focus?** | Ensure the element is natively focusable (`button`, `a`, etc.) or add `tabIndex="0"`. `useDraggable` auto‑sets this when needed. |
| **How do I prevent a drag if the item is disabled?** | Pass `disabled={true}` to `useDraggable` / `useSortable`. |
| **I get duplicate IDs in the live‑region. Why?** | The unique IDs are generated per `<DndContext>`. Don’t render multiple `DndContext` instances that share the same ID space. |
| **How to support touch devices?** | Add `TouchSensor` to the `sensors` array of `<DndContext>`. |
| **How do I localise screen‑reader announcements?** | Replace the default `accessibility.announcements` with functions that return translated strings. |
| **What if my list is huge?** | Use virtualization libraries (e.g., `react-window`) but keep the `DndContext` at the root of the virtualised list. |

---

## Contributing

Pull requests are welcome! If you’re adding a new sensor or improving accessibility, please add relevant tests and update the docs accordingly.

---

## License

MIT © 2025 dnd‑kit contributors

---