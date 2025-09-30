# Mouse Sensor API

## Overview

The **Mouse** sensor is part of the drag‑and‑drop core.  
It listens to native mouse events and turns them into drag‑and‑drop
actions. A drag is triggered only when the user presses the left mouse
button and the sensor’s activation constraints are satisfied.

## Activator

| Event | Description |
|-------|-------------|
| `onMouseDown` | The sensor is initialized when the left mouse button
is pressed. |

> **Note**: Only the left mouse button (`event.button === 0`) will
> trigger the sensor.

## Activation Constraints

The Mouse sensor supports two mutually‑exclusive activation constraints
that control when a drag starts:

| Constraint | Description |
|------------|-------------|
| **Distance** | The mouse must move a minimum distance (in pixels) before the drag starts. |
| **Delay** | The mouse must hold the left button down for a minimum duration (in milliseconds). Motion tolerance is respected during this delay. |

> **Important**: Only one of these constraints may be active at a time.
> If both are provided, the sensor will ignore the second one.

### Distance Constraint

```ts
interface DistanceConstraint {
  /** Minimum distance in pixels the mouse must move before a drag starts. */
  distance: number;
}
```

### Delay Constraint

```ts
interface DelayConstraint {
  /** Time in milliseconds the mouse button must be held down before a drag starts. */
  delay: number;

  /** Maximum distance in pixels the mouse can move during the delay period
      before the drag operation is aborted. */
  tolerance: number;
}
```

When the `delay` constraint is used, a `tolerance` of `0` means any motion
immediately aborts the drag. A larger tolerance (e.g. `5`) allows the mouse to move up to that many pixels without aborting the drag.

## Example Usage

```ts
import { DndContext, MouseSensor } from '@dnd-kit/core';

function App() {
  return (
    <DndContext
      sensors={[new MouseSensor()]}
      activationConstraint={{ distance: 5 }}   // or { delay: 200, tolerance: 5 }
    >
      {/* draggable and droppable components */}
    </DndContext>
  );
}
```

The above example shows a drag start that requires the mouse to move
at least `5` pixels before a drag begins. Alternatively, the user could
hold the mouse button for `200 ms`, allowing up to `5` pixels of motion
before the drag is aborted.

## Changelog

- **Last updated**: 4 years ago

---