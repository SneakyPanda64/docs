# Pointer Sensor – DnD‑Kit Documentation

> The *Pointer* sensor is one of the three default sensors that the **DnD‑Kit** `DndContext` uses when none are supplied explicitly.  
> It is designed to work with any pointing device – mouse, pen, stylus or touch – by normalizing their events into a single DOM‑event model.

---

## 1.  Activator

| Event | What it does | When it’s the *primary* pointer |
|-------|--------------|---------------------------------|
| `onPointerDown` | Triggers the sensor. |  <br>• **Mouse** – always primary.  <br>• **Touch** – primary if no other touches are active.  <br>• **Pen/Stylus** – primary if no other pens/styluses are active. |

> The sensor is only initialized for a *primary* pointer. This keeps accidental multi‑touch drags from firing.

---

## 2.  Activation Constraints

A single activation constraint can be supplied to the sensor – **distance** or **delay** – *not both simultaneously*.

### 2.1 Distance Constraint

```ts
interface DistanceConstraint {
  /** Number of pixels the pointer must move before the drag starts. */
  distance: number;
}
```

*Use‑case*:  A “drag‑and‑drop” that should only start after the user has moved the cursor a bit, preventing accidental drags from tiny movements.

### 2.2 Delay Constraint

```ts
interface DelayConstraint {
  /** Milliseconds the pointer must remain pressed before the drag starts. */
  delay: number;

  /** Pixels of motion tolerated during the delay period. */
  tolerance: number;
}
```

*Explanation*  
- `delay` – e.g., `200`ms for a “long‑press” on touch devices.  
- `tolerance` – how much the pointer may drift before aborting the drag.  
  * `0` → any motion aborts.  
  * `5` → motion less than 5 px is ignored.  

Useful for touch input where a slight finger wobble should not cancel a drag.

---

## 3.  Recommendations

| Topic | Recommendation | Rationale |
|-------|----------------|-----------|
| **CSS – `touch-action`** | ```css .draggable { touch-action: none; } ``` | Prevents scrolling on mobile devices while dragging.  <br>Pointer events cannot cancel the browser’s default behaviour after `pointerdown`, so setting `touch-action: none` *before* the interaction is the only reliable way. |
| **Scrollable lists** | Use a separate drag handle and set `touch-action: none` **only** on the handle. | Keeps list scrolling usable while still preventing scroll when the drag is started via the handle. |
| **Mixed sensors** | If `touch-action` isn’t suitable, consider using the **Mouse** + **Touch** sensors instead of the Pointer sensor. | Touch events allow cancelling default scrolling via `touchmove`, unlike Pointer events. |
| **Dynamic `touch-action` changes** | Changing `touch-action` from `auto` to `none` after a `pointerdown` or `touchstart` has no effect. | The user agent locks the value for the duration of the active pointer. See [Pointer Events Level 2 Spec](https://www.w3.org/TR/pointerevents/). |

> All these recommendations help achieve a smooth, mobile‑friendly drag experience.

---

## 4.  Quick Code Example

```tsx
import { DndContext, useSensor, PointerSensor } from '@dnd-kit/core';

const App = () => {
  const pointerSensor = useSensor(PointerSensor, {
    // Start dragging only after the pointer has moved 10 px
    activationConstraint: {
      distance: 10,
    },
  });

  return (
    <DndContext sensors={[pointerSensor]} collisionDetection={closestCenter}>
      {/* ...draggable items... */}
    </DndContext>
  );
};
```

---

## 5.  Reference Links

- MDN – [Pointer Events](https://developer.mozilla.org/en-US/docs/Web/API/Pointer_events)  
- Pointer Events Level 2 Spec – [Section on `touch-action`](https://www.w3.org/TR/pointerevents/)

*Last updated: 4 years ago* (content derived from the official DnD‑Kit documentation)