# Touch Sensor – API Documentation  

The **Touch** sensor is part of the Drag‑and‑Drop (DnD) library.  
It listens for native touch events (`touchstart`, `touchmove`, `touchend`, …) and
converts them into the library’s drag‑start / drag‑move / drag‑end events.  
The sensor is useful for mobile devices, tablets, or any device that supports
finger or stylus input.

---

## 1.  Activation

| Property | Description | Example |
|----------|-------------|---------|
| **`onTouchStart`** | The event handler that activates the sensor. | `element.addEventListener('touchstart', onTouchStart);` |

The sensor is initialized **only** when there is **one** touch point:

```js
if (event.touches.length === 1) {
  // initialise drag logic
}
```

---

## 2.  Activation Constraints

The Touch sensor supports **two** mutually exclusive constraints that determine when a drag actually starts:

| Constraint | How it works | Use‑case |
|------------|--------------|----------|
| **Distance** | The drag starts after the touch point has moved a minimum distance. | Avoid accidental drags when the user taps. |
| **Delay** | The drag starts after a timeout, but can be aborted if the touch moves beyond a tolerance. | Make sure a deliberate long‑press turns into a drag. |

They are *not* combinable.  Pick one that matches your UX.

### 2.1  Distance Constraint

```ts
interface DistanceConstraint {
  /** Distance in pixels before a drag starts. */
  distance: number;
}
```

```js
const distanceConstraint: DistanceConstraint = { distance: 10 };
```

If the user moves the finger more than 10 px, the `dragstart` event fires.

---

### 2.2  Delay Constraint

```ts
interface DelayConstraint {
  /** Milliseconds to wait before starting a drag. */
  delay: number;
  /** Pixels of movement tolerated during the delay. */
  tolerance: number;
}
```

```js
const delayConstraint: DelayConstraint = { delay: 300, tolerance: 5 };
```

* **`delay`** – Wait 300 ms before a drag can start.  
* **`tolerance`** – If the finger moves more than 5 px during that time, abort the drag.

**Typical values**

| Context | Suggested values |
|---------|-------------------|
| Mobile drag | `delay: 200`, `tolerance: 5` |
| Tablet stylus | `delay: 0` (distance constraint only) |

---

## 3.  CSS Recommendation – `touch-action`

To avoid unintended page scrolling or zooming while dragging, set the `touch-action` CSS property on draggable elements:

```css
.draggable {
  /* Allows manipulation (drag, pinch‑zoom, etc.) */
  touch-action: manipulation;
}
```

* `manipulation` is the safest default for draggable items using the Touch sensor.  
* If you need to disable all gestures, use `touch-action: none;`.

> **Why?**  
> The Touch sensor can react to all touch gestures.  
> Setting `touch-action` tells the browser what to do with the gesture before JavaScript runs, improving performance and preventing double‑handling.

---

## 4.  Summary

| Feature | Description | Typical Usage |
|---------|-------------|---------------|
| `onTouchStart` | Activation handler | `addEventListener('touchstart', onTouchStart)` |
| Distance constraint | Minimum movement | Use when you want a quick drag after a tap. |
| Delay constraint | Hold‑and‑move | Use when a long‑press should become a drag. |
| `touch-action: manipulation` | Prevent scroll/zoom | Apply to all draggable elements. |

---

### Example: Drag‑and‑Drop with a Delay Constraint

```tsx
// React component example
import { DndContext, TouchSensor, useSensor, useSensors } from '@dnd-kit/core';

const sensors = useSensors(
  useSensor(TouchSensor, { activationConstraint: { delay: 200, tolerance: 5 } })
);

function App() {
  return (
    <DndContext sensors={sensors} onDragStart={handleDragStart}>
      {/* Draggable items */}
    </DndContext>
  );
}

function handleDragStart(event) {
  console.log('Drag started', event.active.id);
}
```

> **Tip** – If you observe unintended scrolling, add `touch-action: manipulation;` to the CSS for the draggable items.

---