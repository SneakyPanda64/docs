# dnd‑kit – Sensor API Documentation

> This documentation covers the **Sensors** abstraction used by **dnd‑kit**.  
> Sensors detect user input (mouse, touch, keyboard, etc.) and trigger drag
> operations. All examples below are kept intact.

---

## 1. What are Sensors?

Sensors are classes that

1. **Detect an activation event** (e.g., a mouse click or a key press).  
2. **Attach listeners** to the chosen input method.  
3. **Dispatch drag events** (`dragstart`, `dragmove`, `dragend`, `dragcancel`).  
4. **Clean up** once the drag finishes or is cancelled.

Sensors are instantiated **once** the activation event is detected, so they can
respond immediately to user actions.

---

## 2. Built‑in Sensors

| Sensor | Activator events | Default usage |
|--------|------------------|---------------|
| `PointerSensor` | Pointer events | Enabled by default in `DndContext` |
| `MouseSensor` | `mousedown` | Optional |
| `TouchSensor` | `touchstart` | Optional |
| `KeyboardSensor` | `keydown` | Enabled by default in `DndContext` |

---

## 3. Custom Sensors

If a built‑in sensor does not meet your needs, you can create your own.  
Export your sensor class and register it with `useSensors`.  
Feel free to open an RFC pull request if you think it could help others.

---

## 4. Sensor Lifecycle

```
1. Activator event detected → sensor is instantiated
2. Sensor attaches listeners for its input method
3. Sensor dispatches `dragstart` once constraints are met
4. Sensor dispatches `dragmove` as input changes
5. Sensor dispatches `dragend` or `dragcancel` on finish/cancel
6. Sensor detaches all listeners and is destroyed
```

---

## 5. Using Sensors with Hooks

### 5.1 `useSensor`

`useSensor` creates a sensor instance with optional configuration.

```tsx
import { MouseSensor, TouchSensor, useSensor } from '@dnd-kit/core';

function App() {
  const mouseSensor = useSensor(MouseSensor, {
    // Require the mouse to move 10 px before activation
    activationConstraint: { distance: 10 },
  });

  const touchSensor = useSensor(TouchSensor, {
    // 250 ms press delay, 5 px tolerance
    activationConstraint: { delay: 250, tolerance: 5 },
  });

  // ...later use them with useSensors()
}
```

### 5.2 `useSensors`

`useSensors` combines one or more sensors into a single value that
`DndContext` consumes.

```tsx
import {
  DndContext,
  KeyboardSensor,
  MouseSensor,
  TouchSensor,
  useSensor,
  useSensors,
} from '@dnd-kit/core';

function App() {
  const mouseSensor   = useSensor(MouseSensor);
  const touchSensor   = useSensor(TouchSensor);
  const keyboardSensor = useSensor(KeyboardSensor);

  const sensors = useSensors(
    mouseSensor,
    touchSensor,
    keyboardSensor,
  );

  return (
    <DndContext sensors={sensors}>
      {/* Your drag‑and‑drop components go here */}
    </DndContext>
  );
}
```

> **Shortcut** – you can create the sensors inline:

```tsx
const sensors = useSensors(
  useSensor(MouseSensor),
  useSensor(TouchSensor),
  useSensor(KeyboardSensor),
);
```

---

## 6. Quick Start

```tsx
import { DndContext, useSensors, useSensor } from '@dnd-kit/core';
import { MouseSensor, TouchSensor, KeyboardSensor } from '@dnd-kit/core';

function App() {
  const sensors = useSensors(
    useSensor(MouseSensor, { activationConstraint: { distance: 10 } }),
    useSensor(TouchSensor, { activationConstraint: { delay: 250 } }),
    useSensor(KeyboardSensor),
  );

  return (
    <DndContext sensors={sensors}>
      {/* Your Draggable/Droppable components */}
    </DndContext>
  );
}
```

---

### 7. Installation

```bash
npm install @dnd-kit/core
# or
yarn add @dnd-kit/core
```

---

## 8. Additional Resources

- **Concepts** – [Concepts](https://dndkit.com/docs/concepts)  
- **API Reference** – [API Docs](https://dndkit.com/docs/api)  
- **Guides** – [Accessibility Guide](https://dndkit.com/docs/guides/accessibility)  

---

**Last updated**: 4 years ago

---