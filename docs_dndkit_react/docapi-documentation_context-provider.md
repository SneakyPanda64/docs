# dnd‑kit – `DndContext` Documentation

`DndContext` is the root provider that wires together the drag‑and‑drop system.  
All components that use the hooks `useDraggable`, `useDroppable`, or `DragOverlay` **must** live inside a `<DndContext>`.

> **Tip** – If you need independent drag zones, you can nest multiple `<DndContext>`s.  
> The hooks will only see the nodes inside the nearest ancestor context.

---

## 1. Quick start

```tsx
import React from 'react';
import { DndContext } from '@dnd-kit/core';

export default function App() {
  return (
    <DndContext>
      {/* All draggable / droppable components go here */}
    </DndContext>
  );
}
```

---

## 2. Nesting contexts

```tsx
import React from 'react';
import { DndContext } from '@dnd-kit/core';

function App() {
  return (
    <DndContext>
      {/* Outer drag zone */}
      <DndContext>
        {/* Middle drag zone */}
        <DndContext>
          {/* Inner drag zone */}
        </DndContext>
      </DndContext>
    </DndContext>
  );
}
```

> *Hooks inside the middle context only see nodes from that context, not from the outer or inner ones.*

---

## 3. Props

```ts
interface Props {
  announcements?: Announcements;
  autoScroll?: boolean;
  cancelDrop?: CancelDrop;
  children?: React.ReactNode;
  collisionDetection?: CollisionDetection;
  layoutMeasuring?: Partial<LayoutMeasuring>;
  modifiers?: Modifiers;
  screenReaderInstructions?: ScreenReaderInstructions;
  sensors?: SensorDescriptor<any>[];
  onDragStart?(event: DragStartEvent): void;
  onDragMove?(event: DragMoveEvent): void;
  onDragOver?(event: DragOverEvent): void;
  onDragEnd?(event: DragEndEvent): void;
  onDragCancel?(): void;
}
```

| Prop | Type | Description |
|------|------|-------------|
| `announcements` | `Announcements` | Custom screen‑reader announcement callbacks. |
| `autoScroll` | `boolean` | Globally enable/disable auto‑scrolling for all sensors. |
| `cancelDrop` | `CancelDrop` | Custom drop‑cancellation logic. |
| `children` | `React.ReactNode` | The content to render inside the context. |
| `collisionDetection` | `CollisionDetection` | Algorithm for detecting draggable‑droppable collisions. |
| `layoutMeasuring` | `Partial<LayoutMeasuring>` | Control when droppables are measured. |
| `modifiers` | `Modifiers` | Array of functions that adjust drag coordinates. |
| `screenReaderInstructions` | `ScreenReaderInstructions` | Text that reads to screen‑readers when the focus moves. |
| `sensors` | `SensorDescriptor<any>[]` | Custom sensors (pointer, keyboard, etc.). |
| `onDragStart` | `(event: DragStartEvent) => void` | Fired when a drag starts. |
| `onDragMove` | `(event: DragMoveEvent) => void` | Fired during dragging. |
| `onDragOver` | `(event: DragOverEvent) => void` | Fired when a draggable moves over a droppable. |
| `onDragEnd` | `(event: DragEndEvent) => void` | Fired when the drag ends (drop or cancel). |
| `onDragCancel` | `() => void` | Fired when a drag is cancelled (e.g., escape key). |

---

## 4. Event handlers

### `onDragStart`

```tsx
<DndContext
  onDragStart={event => {
    console.log('Drag started:', event.active.id);
  }}
/>
```

### `onDragMove`

```tsx
<DndContext
  onDragMove={event => {
    console.log('Dragging at', event.delta);
  }}
/>
```

### `onDragOver`

```tsx
<DndContext
  onDragOver={event => {
    console.log('Over droppable', event.over?.id);
  }}
/>
```

### `onDragEnd`

```tsx
<DndContext
  onDragEnd={event => {
    const { active, over } = event;
    console.log('Drag finished', active.id, 'over', over?.id ?? 'none');
  }}
/>
```

> **Note** – `onDragEnd` does **not** move the item. It merely reports the outcome. Your app must update state to reflect the new position.

### `onDragCancel`

```tsx
<DndContext
  onDragCancel={() => {
    console.log('Drag cancelled');
  }}
/>
```

---

## 5. Accessibility

### Announcements

```ts
const customAnnouncements: Announcements = {
  onDragStart(id) {
    return `Picked up item ${id}.`;
  },
  onDragOver(id, overId) {
    if (overId) {
      return `Moving item ${id} over drop zone ${overId}.`;
    }
    return `Item ${id} is not over a drop zone.`;
  },
  onDragEnd(id, overId) {
    if (overId) {
      return `Dropped item ${id} onto ${overId}.`;
    }
    return `Item ${id} was dropped.`;
  },
  onDragCancel(id) {
    return `Drag cancelled. Item ${id} was not dropped.`;
  },
};

<DndContext announcements={customAnnouncements} />
```

The default announcements are:

```ts
const defaultAnnouncements = {
  onDragStart(id) {
    return `Picked up draggable item ${id}.`;
  },
  onDragOver(id, overId) {
    return overId
      ? `Draggable item ${id} was moved over droppable area ${overId}.`
      : `Draggable item ${id} is no longer over a droppable area.`;
  },
  onDragEnd(id, overId) {
    return overId
      ? `Draggable item was dropped over droppable area ${overId}`
      : `Draggable item ${id} was dropped.`;
  },
  onDragCancel(id) {
    return `Dragging was cancelled. Draggable item ${id} was dropped.`;
  },
};
```

### Screen reader instructions

```tsx
<DndContext
  screenReaderInstructions="Use arrow keys to move the item."
/>
```

---

## 6. Auto‑scroll

```tsx
<DndContext autoScroll={false} />
```

- `autoScroll={true}` (default) enables auto‑scroll for all sensors.
- Sensors can opt‑out by setting `static autoScrollEnabled = false` (e.g., the `KeyboardSensor`).

---

## 7. Collision detection

| Algorithm | Description |
|-----------|-------------|
| `RectIntersection` | Default – intersects bounding boxes. |
| `ClosestCenter` | Uses the closest center point. |
| `ClosestCorners` | Uses the nearest corner. |

```tsx
import { DndContext, closestCenter } from '@dnd-kit/core';

<DndContext collisionDetection={closestCenter} />
```

You can also compose or implement custom algorithms.

---

## 8. Sensors

The default sensors are `PointerSensor` and `KeyboardSensor`.

```tsx
import { DndContext, PointerSensor, KeyboardSensor } from '@dnd-kit/core';

<DndContext sensors={[new PointerSensor(), new KeyboardSensor()]} />
```

See the [Sensors guide](https://dndkit.com/guides/sensors) for customizing activation constraints and more.

---

## 9. Modifiers

Modifiers alter the coordinates reported by sensors.

```tsx
import { DndContext, restrictToVerticalAxis } from '@dnd-kit/core';

<DndContext modifiers={[restrictToVerticalAxis]} />
```

Common modifiers:

- `restrictToVerticalAxis`
- `restrictToHorizontalAxis`
- `restrictToContainer`
- `restrictToScrollContainer`
- `applyResistance`
- `clamp`

Read the [Modifiers guide](https://dndkit.com/guides/modifiers) for details.

---

## 10. Layout measuring

Control when droppables are measured:

```tsx
import { DndContext, LayoutMeasuringStrategy } from '@dnd-kit/core';

<DndContext
  layoutMeasuring={{
    strategy: LayoutMeasuringStrategy.Always,
  }}
/>
```

| Strategy | When measurements occur |
|----------|------------------------|
| `WhileDragging` | Right after dragging starts (default). |
| `BeforeDragging` | Before dragging starts & after it ends. |
| `Always` | Before, during, and after dragging. |

---

## 11. Summary

* Wrap all drag‑and‑drop logic inside `<DndContext>`.  
* Use event callbacks to drive your state.  
* Customize accessibility, collision detection, sensors, modifiers, and layout measurement to fit your UI.  
* Nested contexts give you independent zones.  

Happy dragging!