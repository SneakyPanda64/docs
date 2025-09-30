## Keyboard Sensor – API Documentation

The **Keyboard** sensor is one of the default sensors that can be used with the `DndContext` provider.  
It listens for keyboard events on the activator element (the element that receives the
`useDraggable` listeners) and interprets them as drag‑start, drag‑move, and drag‑end
actions.

> **Note**: The activator element must be focusable. See the **Accessibility** guide for details.

---

### Activator

| Event | Description | Trigger |
|-------|-------------|---------|
| `onKeyDown` | The sensor is initialized when the key code matches one of the *start* keys. | `event.code` matches `keyboardCodes.start` |

---

### Options

The `Keyboard` sensor accepts a single `options` object.  
Its interface is:

```ts
type KeyboardCode = KeyboardEvent["code"];

interface KeyboardCodes {
  /** Keys that start dragging */
  start: KeyboardCode[];
  /** Keys that cancel dragging */
  cancel: KeyboardCode[];
  /** Keys that end dragging */
  end: KeyboardCode[];
}

/**
 * Options passed to the Keyboard sensor
 */
interface KeyboardSensorOptions {
  /** Custom keyboard key codes */
  keyboardCodes?: KeyboardCodes;

  /**
   * Custom coordinate getter function.
   * It receives the current keyboard event and the current coordinates.
   */
  getNextCoordinates?: (
    event: KeyboardEvent,
    args: { currentCoordinates: { x: number; y: number } }
  ) => { x: number; y: number } | undefined;

  /**
   * Scroll behavior when moving to new coordinates.
   * @default "smooth"
   */
  scrollBehavior?: "smooth" | "auto";
}
```

#### Default keyboard codes

```ts
const defaultKeyboardCodes: KeyboardCodes = {
  start: ["Space", "Enter"],
  cancel: ["Escape"],
  end: ["Space", "Enter"],
};
```

> **Accessibility reminder**  
> The third ARIA rule requires that a draggable widget be activatable using both *Enter* (Windows) or *Return* (macOS) and the *Space* key.  
> If you change the default keys, update the `screenReaderInstructions` prop on `<DndContext>` accordingly.

#### Custom coordinate getter

By default, each arrow key moves the dragged item by **25 px** in the corresponding direction.  
You can override this behaviour with `getNextCoordinates`.

**Example – move 50 px per arrow key press**

```ts
function customCoordinatesGetter(
  event: KeyboardEvent,
  { currentCoordinates }: { currentCoordinates: { x: number; y: number } }
) {
  const delta = 50;

  switch (event.code) {
    case "Right":
      return { ...currentCoordinates, x: currentCoordinates.x + delta };
    case "Left":
      return { ...currentCoordinates, x: currentCoordinates.x - delta };
    case "Down":
      return { ...currentCoordinates, y: currentCoordinates.y + delta };
    case "Up":
      return { ...currentCoordinates, y: currentCoordinates.y - delta };
  }

  return undefined; // no movement for other keys
}
```

> **Advanced use case**  
> The **Sortable** preset demonstrates how `getNextCoordinates` can be used to reorder items by moving the active item to a new index when arrow keys are pressed.

#### Scroll behavior

```ts
scrollBehavior: "smooth" | "auto"; // default is "smooth"
```

- `"smooth"`: the scroll container animates to the new coordinates.  
- `"auto"`: jumps directly to the new coordinates without animation.

---

### Usage Example

```tsx
import { DndContext } from "@dnd-kit/core";
import { useDraggable } from "@dnd-kit/core";

const MyDraggable = () => {
  const { attributes, listeners, setNodeRef } = useDraggable({
    id: "my-item",
  });

  return (
    <div
      ref={setNodeRef}
      {...attributes}
      {...listeners}          // includes onKeyDown for keyboard
    >
      Drag me
    </div>
  );
};

function App() {
  return (
    <DndContext
      sensors={[KeyboardSensor]}
      screenReaderInstructions="Drag with arrow keys; press space or enter to start"
      collisionDetection={closestCenter}
    >
      <MyDraggable />
    </DndContext>
  );
}
```

---

### Summary

| Feature | Default | How to customize |
|---------|---------|------------------|
| Start keys | `["Space", "Enter"]` | `keyboardCodes.start` |
| Cancel key | `["Escape"]` | `keyboardCodes.cancel` |
| End keys | `["Space", "Enter"]` | `keyboardCodes.end` |
| Move step | `25 px` | `getNextCoordinates` |
| Scroll | `"smooth"` | `scrollBehavior` |

Keep in mind the accessibility requirements when customizing the keyboard shortcuts.  
For more detailed guidance, refer to the library’s **Accessibility** guide.