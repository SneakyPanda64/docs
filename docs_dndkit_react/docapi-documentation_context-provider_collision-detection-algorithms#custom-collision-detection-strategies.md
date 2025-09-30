# Collision‑Detection Algorithms – dnd‑kit Documentation

This section gives a quick‑reference guide to the collision‑detection algorithms that ship with **@dnd‑kit/core**.  
All examples shown here are taken from the source code or the official docs and are kept intact.

> **Note** – The original documentation contains a mixture of markdown, code, and prose.  
> In the following rewrite we keep the content as close as possible to the source while
> formatting it for readability.

---

## 1.  Overview

In the dnd‑kit world a **collision** happens when a draggable element overlaps with a droppable container.  
The library ships with a handful of ready‑to‑use algorithms that operate on the **Axis‑Aligned Bounding Box (AABB)** of every element.  
If you need something beyond rectangles (e.g. circles, rotated shapes, or custom hit‑boxes) you can implement your own algorithm or compose existing ones.

> **Why AABB?**  
> Every element in the DOM has a *bounding box* – the smallest axis‑aligned rectangle that encloses the element *and all of its descendants*.  
> Even if the visible shape is round or triangular, the underlying collision logic works on its bounding box.

---

## 2.  Built‑in Collision Detection Algorithms

| Algorithm | When to Use | How it Works |
|-----------|-------------|--------------|
| **rectIntersection** | Default | Two rectangles must overlap on **all four sides** (no gap). |
| **closestCenter** | Sortable lists, forgiving drag‑and‑drop | Chooses the droppable whose *center* is closest to the center of the draggable’s bounding rectangle. |
| **closestCorners** | Stacked droppable areas (Kanban, etc.) | Measures the distance between each of the draggable’s four corners and each droppable’s four corners; the pair with the smallest distance wins. |
| **pointerWithin** | High‑precision interfaces, pointer‑only | Registers a collision only when the pointer lies inside the droppable’s bounding rectangle.  Works only with pointer‑based sensors. |

> **Choosing an Algorithm**  
> - `rectIntersection` is strict – useful for precise “snap‑to‑grid” behaviours.  
> - `closestCenter` is forgiving – the default for sortable lists.  
> - `closestCorners` is ideal for nested, overlapping droppables (Kanban columns).  
> - `pointerWithin` is great for keyboard‑friendly or mouse‑only precision work.

---

## 3.  Custom Collision Detection

### 3.1  From Scratch

If you need collision logic that isn’t covered by the built‑ins, you can write your own function.  
It must accept the same arguments that the library passes to the built‑in algorithms and return an array of collision objects.

```ts
import {
  CollisionDetection,
  Collision,
  DroppableContainer,
  Rect,
} from '@dnd-kit/core';

const myCustomCollisionDetection: CollisionDetection = ({
  collisionRect,
  droppableContainers,
  droppableRects,
  ...rest
}) => {
  const collisions: Collision[] = [];

  for (const container of droppableContainers) {
    const rect = droppableRects.get(container.id);
    if (!rect) continue;

    // …your custom hit‑test logic here…

    collisions.push({
      id: container.id,
      data: { /* optional, e.g. distance */ },
    });
  }

  return collisions;
};
```

### 3.2  Composing Existing Algorithms

Sometimes you only need to *add* a layer of tolerance or fall‑back logic.  
Below are two typical compositions.

#### 3.2.1  `pointerWithin` with a Fallback

```ts
import { pointerWithin, rectIntersection } from '@dnd-kit/core';

function customCollisionDetectionAlgorithm(args) {
  // 1️⃣ Try pointer‑based collision first
  const pointerCollisions = pointerWithin(args);

  if (pointerCollisions.length > 0) {
    return pointerCollisions;          // pointer hit – accept it
  }

  // 2️⃣ If nothing was hit, fall back to rectangle intersection
  return rectIntersection(args);
}
```

#### 3.2.2  Different Algorithms for Different Droppables

```ts
import { closestCorners, rectIntersection } from '@dnd-kit/core';

function customCollisionDetectionAlgorithm({
  droppableContainers,
  ...args
}) {
  // 1️⃣ Handle the special “trash” droppable with rectIntersection
  const trashCollisions = rectIntersection({
    ...args,
    droppableContainers: droppableContainers.filter(({ id }) => id === 'trash')
  });

  if (trashCollisions.length > 0) {
    return trashCollisions;  // trash hit – return early
  }

  // 2️⃣ All other droppables use closestCorners
  return closestCorners({
    ...args,
    droppableContainers: droppableContainers.filter(({ id }) => id !== 'trash')
  });
}
```

### 3.3  Non‑Rectangular Collisions – Example for Circles

Below is a minimal example that detects circle collisions and returns the one with the greatest intersection area.

```ts
/**
 * Sort collisions in descending order (from greatest to smallest value)
 */
export function sortCollisionsDesc(
  { data: { value: a } }: Collision,
  { data: { value: b } }: Collision
) {
  return b - a;
}

/**
 * Return the distance between two circle centers, or 0 if no overlap
 */
function getCircleIntersection(entry: Rect, target: Rect): number {
  const circle1 = { radius: 20, x: entry.offsetLeft, y: entry.offsetTop };
  const circle2 = { radius: 12, x: target.offsetLeft, y: target.offsetTop };

  const dx = circle1.x - circle2.x;
  const dy = circle1.y - circle2.y;
  const distance = Math.hypot(dx, dy);

  return distance < circle1.radius + circle2.radius ? distance : 0;
}

/**
 * Returns the circle that has the greatest intersection area
 */
function circleIntersection({
  collisionRect,
  droppableRects,
  droppableContainers,
}: {
  collisionRect: Rect;
  droppableRects: Map<string, Rect>;
  droppableContainers: DroppableContainer[];
}): Collision[] {
  const collisions: Collision[] = [];

  for (const container of droppableContainers) {
    const rect = droppableRects.get(container.id);
    if (!rect) continue;

    const intersectionRatio = getCircleIntersection(rect, collisionRect);
    if (intersectionRatio > 0) {
      collisions.push({
        id: container.id,
        data: { droppableContainer: container, value: intersectionRatio },
      });
    }
  }

  return collisions.sort(sortCollisionsDesc);
}
```

> **Tip** – If you need to support keyboard sensors with `pointerWithin`, always compose with a tolerant algorithm such as `closestCenter` or `rectIntersection`.

---

## 4.  Summary

| Algorithm | Use‑Case | Notes |
|-----------|----------|-------|
| `rectIntersection` | Precise hit‑testing | Requires full overlap |
| `closestCenter` | Sortable lists, forgiving | Works with any sensor |
| `closestCorners` | Overlapping droppables (Kanban) | Matches human intuition in stacks |
| `pointerWithin` | Mouse‑only high precision | Only pointer sensors |
| **Custom** | Any special shape / rule | Compose or write from scratch |

All examples above are ready to drop into your project.  
Happy dragging!