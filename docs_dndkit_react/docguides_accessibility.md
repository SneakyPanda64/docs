# dnd‑kit/core – Accessible Drag & Drop

`dnd‑kit/core` is a lightweight, highly‑customizable drag‑and‑drop engine for React.  
It ships with sensible accessibility defaults (keyboard support, ARIA attributes, live‑region
announcements) that you can adapt to any UI.  The following docs give you an overview, how to
install it, a quick start, and a reference to the core API.  All examples are kept verbatim
from the original source so that you can copy‑paste them straight into your project.

> **TL;DR**  
> ```bash
> npm i @dnd-kit/core
> # or
> yarn add @dnd-kit/core
> ```
> ```tsx
> import {DndContext, useDraggable, useSortable, arrayMove} from '@dnd-kit/core';
> ```
> 
> ```tsx
> <DndContext
>   sensors={[keyboardSensor]}
>   accessibility={{
>     announcements,
>     screenReaderInstructions,
>   }}
> >
>   …
> </DndContext>
> ```

---

## Table of Contents

| Section | Description |
|---------|-------------|
| [Introduction](#introduction) | Why accessibility matters in DnD. |
| [Installation](#installation) | npm / yarn setup. |
| [Quick Start](#quick-start) | Minimal example. |
| [API Documentation](#api-documentation) | Detailed reference. |
|   - [DndContext](#dndcontext) | The root provider. |
|   - [Droppable](#droppable) | Droppable areas. |
|   - [Draggable](#draggable) | Draggable elements. |
|   - [Sensors](#sensors) | Keyboard, mouse, touch sensors. |
|   - [Modifiers](#modifiers) | Custom drag‑behaviour modifiers. |
|   - [PRESETS](#presets) | Sortable, drag‑drop‑group, etc. |
|   - [Guides](#guides) | Accessibility, keyboard, screen‑reader. |
| [Examples](#examples) | Full‑featured snippets. |
| [FAQ](#faq) | Common questions. |

---

## Introduction

Drag‑and‑drop interfaces can be a nightmare for people using assistive technologies.  
`dnd‑kit/core` tackles the problem head‑on:

* **Keyboard support** – All interactions are possible with a keyboard, following ARIA
  rule #3.
* **Screen‑reader instructions** – Voice‑over descriptions are automatically attached.
* **Live‑region announcements** – Real‑time updates for screen‑reader users.
* **Customisable** – Default behaviour is just a starting point; tailor it to your app.

> *“A digital product is not complete if it is not usable by everyone.”*  
> — Web Almanac, HTTP Archive

---

## Installation

```bash
# npm
npm install @dnd-kit/core

# yarn
yarn add @dnd-kit/core
```

> **Tip** – If you’re only using the sortable preset, you can also install
> `@dnd-kit/sortable` separately to keep bundle sizes down.

---

## Quick Start

```tsx
import {DndContext, useDraggable, arrayMove} from '@dnd-kit/core';
import {useState} from 'react';

function App() {
  const [items, setItems] = useState(['Apple', 'Orange', 'Strawberries']);

  return (
    <DndContext
      collisionDetection={closestCenter}
      onDragEnd={({active, over}) => {
        if (over) {
          setItems((prev) => arrayMove(prev, active.index, over.index));
        }
      }}
    >
      {items.map((item, index) => (
        <DraggableItem key={item} id={item} index={index} />
      ))}
    </DndContext>
  );
}

function DraggableItem({id, index}) {
  const {attributes, listeners, setNodeRef, transform} = useDraggable({
    id,
    index,
  });

  const style = {
    transform: `translate3d(${transform?.x ?? 0}px,${transform?.y ?? 0}px,0)`,
  };

  return (
    <div ref={setNodeRef} style={style} {...listeners} {...attributes}>
      {id}
    </div>
  );
}
```

> **What’s happening?**  
> * `useDraggable` attaches ARIA attributes (`role="button"`, `aria-roledescription="draggable"`, etc.).  
> * The `DndContext` handles all sensor events, collision detection, and state updates.  
> * `arrayMove` is a small utility to reorder an array.

---

## API Documentation

### DndContext

```tsx
<DndContext
  sensors={Array<Sensor>}
  collisionDetection={(args) => CollisionDetection}
  onDragStart={(event) => void}
  onDragOver={(event) => void}
  onDragEnd={(event) => void}
  onDragCancel={(event) => void}
  accessibility={{
    announcements: CustomAnnouncements,
    screenReaderInstructions: CustomInstructions,
  }}
>
  {children}
</DndContext>
```

* **Sensors** – Default includes `KeyboardSensor` and `PointerSensor`.  
* **Accessibility prop** – Two sub‑objects:
  * `announcements` – Live‑region messages.  
  * `screenReaderInstructions` – Off‑screen instructions for keyboard users.

#### Example: Custom Announcements

```tsx
const announcements = {
  onDragStart({active}) {
    return `Picked up draggable item ${active.id}.`;
  },
  onDragOver({active, over}) {
    if (over) {
      return `Draggable item ${active.id} was moved over droppable area ${over.id}.`;
    }
    return `Draggable item ${active.id} is no longer over a droppable area.`;
  },
  onDragEnd({active, over}) {
    if (over) {
      return `Draggable item ${active.id} was dropped over droppable area ${over.id}`;
    }
    return `Draggable item ${active.id} was dropped.`;
  },
  onDragCancel({active}) {
    return `Dragging was cancelled. Draggable item ${active.id} was dropped.`;
  },
};
```

#### Example: Custom Screen‑Reader Instructions

```tsx
const screenReaderInstructions = `
  To pick up a draggable item, press space or enter.
  While dragging, use the arrow keys to move the item in any direction.
  Press space or enter again to drop the item, or press escape to cancel.
`;
```

---

### Droppable

> Wrap any area that can accept a drop.

```tsx
function DroppableArea() {
  const {setNodeRef} = useDroppable({
    id: 'droppable-1',
    data: {type: 'list'},
  });

  return <div ref={setNodeRef}>Drop here</div>;
}
```

* **`id`** – Unique identifier for the droppable.  
* **`data`** – Optional payload that is available in events.

---

### Draggable

> Attach to an element that can be dragged.

```tsx
function DraggableItem({id}) {
  const {attributes, listeners, setNodeRef, transform} = useDraggable({
    id,
    data: {type: 'item'},
  });

  const style = {
    transform: `translate3d(${transform?.x ?? 0}px,${transform?.y ?? 0}px,0)`,
  };

  return (
    <div ref={setNodeRef} style={style} {...listeners} {...attributes}>
      {id}
    </div>
  );
}
```

* **`attributes`** – ARIA attributes (`role`, `aria-roledescription`, etc.).  
* **`listeners`** – Mouse / keyboard event handlers.  
* **`setNodeRef`** – Hook to bind the DOM node.

> **Keyboard Requirements** – If you’re using a non‑native element (e.g. a `div`), `useDraggable`
> automatically sets `tabIndex="0"` so it can receive focus.

---

### Sensors

| Sensor | Purpose | Default |
|--------|---------|---------|
| `KeyboardSensor` | Enables keyboard interaction. | ✅ |
| `PointerSensor` | Mouse & touch support. | ✅ |
| `TouchSensor` | Dedicated touch handling (optional). | ❌ |
| `CustomSensor` | Your own sensor implementation. | ❌ |

#### Example: Enabling the Keyboard Sensor

```tsx
import {DndContext, KeyboardSensor} from '@dnd-kit/core';

const sensors = [new KeyboardSensor()];
<DndContext sensors={sensors}>…</DndContext>;
```

---

### Modifiers

> Functions that alter the drag position before it is applied.

```tsx
const snapToGrid = (args) => ({
  ...args,
  transform: {
    x: Math.round(args.transform.x / 20) * 20,
    y: Math.round(args.transform.y / 20) * 20,
  },
});

<DndContext
  modifiers={[snapToGrid]}
  …
>
  …
</DndContext>
```

---

### PRESETS

| Preset | What it does | Usage |
|--------|--------------|-------|
| `useSortable` | Sortable list items. | `useSortable({id, index})` |
| `useDroppable` | Simple drop zone. | `useDroppable({id})` |
| `useDragOverlay` | Custom overlay during drag. | `useDragOverlay({items, modifiers})` |

---

## Guides

### Accessibility

1. **Keyboard support** – All drag‑drop actions are keyboard‑navigable.  
2. **Screen‑reader instructions** – Off‑screen text that tells the user how to operate the widget.  
3. **Live‑region announcements** – Real‑time updates without changing focus.

#### Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `Space` / `Enter` | Pick up / drop |
| Arrow keys | Move when dragging |
| `Esc` | Cancel drag |

> *Customize these keys via the `KeyboardSensor` options, but keep the defaults to satisfy ARIA.*  

#### Screen‑Reader Instructions Example

> Default instructions (English):

```
To pick up a draggable item, press space or enter.
While dragging, use the arrow keys to move the item in any direction.
Press space or enter again to drop the item, or press escape to cancel.
```

> Tailored example for a grocery list:

```
To pick up a grocery list item, press space or enter.
Use the up and down arrow keys to update the position of the item in the grocery list.
Press space or enter again to drop the item, or press escape to cancel.
```

> Always provide localized versions for multi‑language apps.

#### Live‑Region Announcements Example

```tsx
const announcements = {
  onDragStart({active}) {
    return `Picked up draggable item ${active.id}.`;
  },
  onDragOver({active, over}) {
    if (over) {
      return `Draggable item ${active.id} was moved over droppable area ${over.id}.`;
    }
    return `Draggable item ${active.id} is no longer over a droppable area.`;
  },
  onDragEnd({active, over}) {
    if (over) {
      return `Draggable item ${active.id} was dropped over droppable area ${over.id}`;
    }
    return `Draggable item ${active.id} was dropped.`;
  },
  onDragCancel({active}) {
    return `Dragging was cancelled. Draggable item ${active.id} was dropped.`;
  },
};
```

> For lists, prefer **positions** over **indices**:

```tsx
function App() {
  const [items, setItems] = useState(['Apple', 'Orange', 'Strawberries', 'Raspberries']);
  const getPosition = (id) => items.indexOf(id) + 1; // 1‑based
  const itemCount = items.length;

  const announcements = {
    onDragStart({active}) {
      return `Picked up sortable item ${active.id}. Sortable item ${active.id} is in position ${getPosition(active.id)} of ${itemCount}`;
    },
    onDragOver({active, over}) {
      if (over) {
        return `Sortable item ${active.id} was moved into position ${getPosition(over.id)} of ${itemCount}`;
      }
    },
    onDragEnd({active, over}) {
      if (over) {
        return `Sortable item ${active.id} was dropped at position ${getPosition(over.id)} of ${itemCount}`;
      }
    },
    onDragCancel({active}) {
      return `Dragging was cancelled. Sortable item ${active.id} was dropped.`;
    },
  };

  return (
    <DndContext
      accessibility={announcements}
    >
      …
    </DndContext>
  );
}
```

---

## Examples

### 1. Accessible Sortable List

```tsx
import {DndContext, useSortable, arrayMove, closestCenter} from '@dnd-kit/core';
import {useState} from 'react';

function SortableList() {
  const [items, setItems] = useState(['Apple', 'Orange', 'Strawberries', 'Raspberries']);

  const getPosition = (id) => items.indexOf(id) + 1;
  const itemCount = items.length;

  const announcements = {
    onDragStart({active}) {
      return `Picked up sortable item ${active.id}. Sortable item ${active.id} is in position ${getPosition(active.id)} of ${itemCount}`;
    },
    onDragOver({active, over}) {
      if (over) {
        return `Sortable item ${active.id} was moved into position ${getPosition(over.id)} of ${itemCount}`;
      }
    },
    onDragEnd({active, over}) {
      if (over) {
        setItems((prev) => arrayMove(prev, active.index, over.index));
        return `Sortable item ${active.id} was dropped at position ${getPosition(over.id)} of ${itemCount}`;
      }
    },
    onDragCancel({active}) {
      return `Dragging was cancelled. Sortable item ${active.id} was dropped.`;
    },
  };

  return (
    <DndContext
      collisionDetection={closestCenter}
      accessibility={announcements}
    >
      {items.map((id, index) => (
        <SortableItem key={id} id={id} index={index} />
      ))}
    </DndContext>
  );
}

function SortableItem({id, index}) {
  const {
    attributes,
    listeners,
    setNodeRef,
    transform,
    transition,
    isDragging,
  } = useSortable({id, index});

  const style = {
    transform: `translate3d(${transform?.x ?? 0}px,${transform?.y ?? 0}px,0)`,
    transition,
    opacity: isDragging ? 0.5 : 1,
  };

  return (
    <li ref={setNodeRef} style={style} {...listeners} {...attributes}>
      {id}
    </li>
  );
}
```

### 2. Accessible Drag‑Drop Between Two Containers

```tsx
import {DndContext, useDraggable, useDroppable, closestCenter} from '@dnd-kit/core';

function DragDropExample() {
  const [left, setLeft] = useState(['Apple', 'Orange']);
  const [right, setRight] = useState(['Strawberries', 'Raspberries']);

  return (
    <DndContext collisionDetection={closestCenter} onDragEnd={handleDrop}>
      <div className="container">
        <Container items={left} setItems={setLeft} id="left" />
        <Container items={right} setItems={setRight} id="right" />
      </div>
    </DndContext>
  );
}

function Container({items, setItems, id}) {
  const {setNodeRef} = useDroppable({id});
  return (
    <ul ref={setNodeRef} className="list">
      {items.map((item, index) => (
        <DraggableItem key={item} id={item} />
      ))}
    </ul>
  );
}

function DraggableItem({id}) {
  const {attributes, listeners, setNodeRef} = useDraggable({id});
  return (
    <li ref={setNodeRef} {...attributes} {...listeners}>
      {id}
    </li>
  );
}

function handleDrop(event) {
  const {active, over} = event;
  if (!over) return;
  // Simple implementation: move item to the other container
  // You’ll likely need to track which container each item is in.
}
```

---

## FAQ

| Question | Answer |
|----------|--------|
| **Can I use a native `<button>` instead of `div` for draggable items?** | Yes! It will automatically satisfy the `role="button"` requirement and still work with keyboard. |
| **Is the keyboard sensor available on mobile devices?** | The keyboard sensor is primarily for desktop. Mobile devices use touch sensors. |
| **How do I make the announcements read in another language?** | Pass a custom `announcements` object in the `accessibility` prop that returns strings in your target language. |
| **What if I want to change the arrow‑key movement step?** | Use the `getNextCoordinates` option on `KeyboardSensor`. |

---

## Further Reading

* [Web Almanac – Accessibility](https://almanac.httparchive.org/en/2021/accessibility) – Primer on accessibility.  
* [WAI – Tools & Techniques](https://www.w3.org/WAI/ARIA/intro/index.html) – Understanding assistive tech.  

Happy coding! 🚀