# Drag‑and‑Drop Documentation (dnd‑kit)

This guide covers the core concepts of drag‑and‑drop in **dnd‑kit**, the built‑in collision‑detection algorithms, and how to create custom algorithms that suit your UI.  
All examples are kept intact so you can copy‑paste them into your project.

---

## Table of Contents
1. [Overview](#overview)
2. [Installation](#installation)
3. [Quick Start](#quick-start)
4. [API Documentation](#api-documentation)
   - [DndContext](#dndcontext)
   - [Collision‑Detection Algorithms](#collision-detection-algorithms)
     - [Rectangle Intersection](#rectangle-intersection)
     - [Closest Center](#closest-center)
     - [Closest Corners](#closest-corners)
     - [Pointer Within](#pointer-within)
   - [Custom Collision‑Detection Algorithms](#custom-collision-detection-algorithms)
     - [Composition of Existing Algorithms](#composition-of-existing-algorithms)
     - [Building Algorithms from Scratch](#building-algorithms-from-scratch)
5. [Sensors & Modifiers](#sensors--modifiers)
6. [Presets](#presets)
7. [Guides](#guides)

---

## Overview

Drag‑and‑drop is a ubiquitous interaction pattern in modern web apps.  
**dnd‑kit** is a lightweight, composable library that gives you full control over:

- Drag sources (`<Draggable />`)
- Drop targets (`<Droppable />`)
- Sensors (mouse, touch, keyboard, pointer, etc.)
- Collision‑detection strategies

Below you’ll find a deep dive into the default collision‑detection algorithms and how to extend them.

---

## Installation

```bash
# npm
npm install @dnd-kit/core @dnd-kit/sortable

# yarn
yarn add @dnd-kit/core @dnd-kit/sortable
```

---

## Quick Start

```tsx
import { DndContext, useDraggable, useDroppable } from '@dnd-kit/core';

function MyComponent() {
  return (
    <DndContext>
      <Draggable />
      <Droppable />
    </DndContext>
  );
}
```

See the [examples](https://github.com/clauderic/dnd-kit) for more elaborate setups.

---

## API Documentation

### DndContext

`<DndContext>` is the top‑level provider that manages the drag state.  
Key props:

| Prop | Type | Description |
|------|------|-------------|
| `collisionDetection` | function | Custom collision‑detection algorithm. Default: `rectIntersection`. |
| `sensors` | Sensor[] | Array of active sensors (mouse, touch, etc.). |
| `onDragStart` | function | Callback when a drag starts. |
| `onDragMove` | function | Callback for drag movement. |
| `onDragEnd` | function | Callback when a drag ends. |
| `onDragCancel` | function | Callback when a drag is cancelled. |

> **Tip** – Use the `collisionDetection` prop to swap between algorithms.

---

### Collision‑Detection Algorithms

All algorithms share the same signature:

```ts
type CollisionDetection = (args: CollisionDetectionArgs) => Collision[];
```

The library ships with four ready‑to‑use algorithms:

| Algorithm | Use‑Case | Description |
|-----------|----------|-------------|
| `rectIntersection` | Default | Requires rectangle overlap. |
| `closestCenter` | Sortable lists | Picks the droppable whose *center* is nearest the drag center. |
| `closestCorners` | Overlapping droppables | Considers all four corners; more forgiving when droppables are stacked. |
| `pointerWithin` | High‑precision | Only registers collision if the pointer is inside a droppable. |

---

#### Rectangle Intersection

The default algorithm. It checks that **every side** of two rectangles overlaps.

```ts
import { rectIntersection } from '@dnd-kit/core';
```

---

#### Closest Center

Best for sortable lists. Finds the droppable whose center is closest to the center of the active draggable.

```ts
import { closestCenter } from '@dnd-kit/core';
```

> *When to use?*  
> Most sortable lists – it gives a forgiving drop zone.

---

#### Closest Corners

Meant for UI patterns where droppables may be stacked (e.g., Kanban boards).  
It measures distance between **each corner of the draggable** and **each corner of the droppable**.

```ts
import { closestCorners } from '@dnd-kit/core';
```

> **Example**  
> In a Kanban column where droppables overlap, `closestCorners` will usually pick the droppable that the user actually expects.

---

#### Pointer Within

Only registers a collision if the pointer is inside the droppable’s bounding rectangle.  
Great for precision controls but **does not work with keyboard sensors**.

```ts
import { pointerWithin } from '@dnd-kit/core';
```

---

## Custom Collision‑Detection Algorithms

If none of the built‑in strategies fit your UI, you can create your own.  
You can:

1. **Compose** existing algorithms.
2. **Implement** from scratch (e.g., circles, polygons).

---

### Composition of Existing Algorithms

#### Pointer + Rectangle

```tsx
import { pointerWithin, rectIntersection } from '@dnd-kit/core';

function customCollisionDetection(args) {
  // First, try the pointer algorithm
  const pointerCollisions = pointerWithin(args);

  if (pointerCollisions.length > 0) return pointerCollisions;

  // Fallback to rectangle intersection
  return rectIntersection(args);
}
```

> This pattern is useful when you need high precision with the pointer but still want a fallback for keyboard navigation.

#### Mixed Algorithms for Different Droppables

```tsx
import { closestCorners, rectIntersection } from '@dnd-kit/core';

function customCollisionDetection({
  droppableContainers,
  ...args
}) {
  // 1️⃣ Handle the 'trash' droppable with rectangle intersection
  const trashCollisions = rectIntersection({
    ...args,
    droppableContainers: droppableContainers.filter(({ id }) => id === 'trash')
  });

  if (trashCollisions.length > 0) return trashCollisions;

  // 2️⃣ All other droppables use closestCorners
  return closestCorners({
    ...args,
    droppableContainers: droppableContainers.filter(({ id }) => id !== 'trash')
  });
}
```

> *Scenario* – A sortable list plus a trash bin that accepts drops only when the rectangle overlaps.

---

### Building Algorithms from Scratch

#### Circle Collision Example

Below is a simplified algorithm that detects collisions between circular bounding boxes.  
It’s intentionally minimal – adapt it to your exact needs.

```ts
/**
 * Sort collisions in descending order (largest first).
 */
export function sortCollisionsDesc(
  { data: { value: a } },
  { data: { value: b } }
) {
  return b - a;
}

/**
 * Calculate intersection ratio between two circles.
 * Returns 0 if no overlap.
 */
function getCircleIntersection(entry, target) {
  // Example: assume static radii for brevity
  const circle1 = { radius: 20, x: entry.offsetLeft, y: entry.offsetTop };
  const circle2 = { radius: 12, x: target.offsetLeft, y: target.offsetTop };

  const dx = circle1.x - circle2.x;
  const dy = circle1.y - circle2.y;
  const distance = Math.sqrt(dx * dx + dy * dy);

  return distance < circle1.radius + circle2.radius ? distance : 0;
}

/**
 * Return the droppable with the greatest circle overlap.
 */
export function circleIntersection({
  collisionRect,
  droppableRects,
  droppableContainers,
}) {
  const collisions = [];

  for (const droppableContainer of droppableContainers) {
    const { id } = droppableContainer;
    const rect = droppableRects.get(id);

    if (rect) {
      const intersectionRatio = getCircleIntersection(rect, collisionRect);
      if (intersectionRatio > 0) {
        collisions.push({
          id,
          data: { droppableContainer, value: intersectionRatio },
        });
      }
    }
  }

  return collisions.sort(sortCollisionsDesc);
}
```

> **How to use** – Pass `circleIntersection` to `<DndContext collisionDetection={circleIntersection} />`.

---

## Sensors & Modifiers

Sensors translate input events into drag events.  
Common sensors:

- `mouseSensor` – Desktop mouse.
- `touchSensor` – Touch devices.
- `keyboardSensor` – Keyboard interactions.
- `pointerSensor` – Pointer events.

Modifiers can modify drag behavior (e.g., snapping, inertia).  
See the official docs for a full list.

---

## Presets

*dnd‑kit* ships with high‑level presets that combine a set of components and configuration:

- `Sortable` – A fully‑featured sortable list.
- `Tree` – Drag‑and‑drop within nested trees.
- `DragOverlay` – A floating preview of the dragged item.

> Use presets for quick setups; drop‑in with minimal code.

---

## Guides

- **Accessibility** – Making drag‑and‑drop keyboard‑friendly.
- **Performance** – Optimizing for large lists.
- **Extending** – Writing custom sensors and modifiers.

For deeper dives, visit the [GitBook guide](https://github.com/clauderic/dnd-kit/blob/master/README.md).

---

*Last updated: 3 years ago (adapted for clarity).*