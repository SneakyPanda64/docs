# @dnd‑kit – A Lightweight, Modular Drag & Drop Toolkit for React

> **Why @dnd‑kit?**  
> A zero‑dependency, performance‑first library that gives you full control over drag‑and‑drop behaviour, animations and accessibility.  
> Built around React hooks (`useDraggable`, `useDroppable`) and context (`<DndContext>`), it lets you augment any component without adding wrapper nodes.

---

## Table of Contents

| Section | Description |
|---------|-------------|
| [Installation](#installation) | Getting the package into your project |
| [Quick Start](#quick-start) | Minimal example using hooks |
| [API Documentation](#api-documentation) | Core concepts & public API |
| [Presets](#presets) | Built‑in presets (sortable, grid, etc.) |
| [Guides](#guides) | Accessibility, Architecture, Performance |
| [Extensibility](#extensibility) | Sensors, Modifiers, Collision detection |
| [Contributing](#contributing) | How to help improve @dnd‑kit |

---

## Installation

```bash
# npm
npm install @dnd-kit/core @dnd-kit/sortable

# yarn
yarn add @dnd-kit/core @dnd-kit/sortable
```

> `@dnd-kit/sortable` is a thin layer that builds on top of `@dnd-kit/core`.  It provides ready‑to‑use sortable behaviour but can be omitted if you only need drag‑and‑drop.

---

## Quick Start

Below is a minimal example that turns a list into a sortable list.  
All code is written in **TypeScript** (you can drop the types for plain JS).

```tsx
import { DndContext, useDraggable, useDroppable } from '@dnd-kit/core'
import { SortableContext, sortableKeyboardCoordinates, arrayMove } from '@dnd-kit/sortable'
import { CSS } from '@dnd-kit/utilities'

const items = ['Apple', 'Banana', 'Cherry']

function Item({ id, index }: { id: string; index: number }) {
  const { attributes, listeners, setNodeRef, transform, transition } = useDraggable({
    id,
  })

  const style = {
    transform: CSS.Transform.toString(transform),
    transition,
    padding: 8,
    margin: 4,
    background: '#eee',
    cursor: 'grab',
  }

  return (
    <div ref={setNodeRef} style={style} {...attributes} {...listeners}>
      {items[index]}
    </div>
  )
}

export default function SortableDemo() {
  const [order, setOrder] = React.useState(items)

  return (
    <DndContext
      sensors={[] /* default sensors – mouse, touch, keyboard */}
      collisionDetection={pointerWithin}
      onDragEnd={({ active, over }) => {
        if (over && active.id !== over.id) {
          setOrder((prev) => arrayMove(prev, prev.indexOf(active.id), prev.indexOf(over.id)))
        }
      }}
    >
      <SortableContext items={order} strategy={verticalListSortingStrategy}>
        {order.map((id, index) => (
          <Item key={id} id={id} index={index} />
        ))}
      </SortableContext>
    </DndContext>
  )
}
```

### Explanation

| Hook / Component | Purpose |
|------------------|---------|
| `DndContext` | Provides the drag‑drop environment, manages sensors & events |
| `useDraggable` | Makes an element draggable; gives refs, listeners, attributes and drag state |
| `useDroppable` | Makes an element a drop target (useful for multi‑container drag‑drop) |
| `SortableContext` | Sets up the context for sorting items; uses the `strategy` you choose |
| `arrayMove` | Utility that moves an item in an array – handy for updating state after a drag |

---

## API Documentation

### `<DndContext>`

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `sensors` | `Sensor[]` | `[]` (mouse, touch, keyboard) | List of input sensors to use |
| `collisionDetection` | `CollisionDetection` | `pointerWithin` | Function that returns a collision detection algorithm |
| `onDragStart` | `(event: DragStartEvent) => void` | — | Called when a drag starts |
| `onDragMove` | `(event: DragMoveEvent) => void` | — | Called on every movement |
| `onDragEnd` | `(event: DragEndEvent) => void` | — | Called when drag ends |
| `onDragCancel` | `(event: DragCancelEvent) => void` | — | Called if drag is canceled (e.g. `Esc`) |
| `onDragOver` | `(event: DragOverEvent) => void` | — | Called when a draggable hovers over a droppable |
| `autoScroll` | `boolean | AutoScrollOptions` | `false` | Enables auto‑scroll during drag |
| `overlayContainer` | `HTMLElement | null` | `null` | If supplied, the overlay is rendered in this container instead of the default body |

### `useDraggable(options)`

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `id` | `string` | **Required** | Unique identifier for the draggable |
| `data` | `any` | `undefined` | Payload that will be available in drag events |
| `disabled` | `boolean` | `false` | Disables drag if `true` |
| `attributes` | `Record<string, string>` | `{}` | Extra attributes added to the draggable element |
| `listeners` | `React.HTMLAttributes<HTMLElement>` | `{}` | Extra event listeners |
| `autoScroll` | `boolean | AutoScrollOptions` | `true` | Enables auto‑scroll while dragging this item |

#### Return value

```ts
{
  attributes: HTMLAttributes<HTMLElement>
  listeners: HTMLAttributes<HTMLElement>
  setNodeRef: (node: HTMLElement | null) => void
  transform: Transform | null
  transition: string | null
  isDragging: boolean
  over: DroppableId | null
}
```

### `useDroppable(options)`

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `id` | `string` | **Required** | Unique identifier for the droppable area |
| `data` | `any` | `undefined` | Payload that will be available in drag events |
| `disabled` | `boolean` | `false` | Disables dropping if `true` |

#### Return value

```ts
{
  setNodeRef: (node: HTMLElement | null) => void
  isOver: boolean
  over: boolean
  attributes: HTMLAttributes<HTMLElement>
  listeners: HTMLAttributes<HTMLElement>
}
```

### `DragOverlay`

```tsx
<DragOverlay>
  {/* render the node you want to show as the overlay */}
</DragOverlay>
```

> The overlay is rendered *outside* of the normal document flow.  
> Use it to show a ghost preview or a custom drag image.

### Sensors

| Built‑in | Description |
|----------|-------------|
| `PointerSensor` | Handles mouse, touch, and pointer events |
| `KeyboardSensor` | Handles keyboard navigation (`Enter`, `Arrow` keys) |
| `TouchSensor` | Handles touch only, optional for specific use‑cases |

> **Custom Sensor** – Implement `Sensor` interface: `{ id, activate, deactivate, getSensorEventData, getActivationEvent }`.

### Modifiers

Modifiers are functions that can adjust the drag state before it is applied.

```ts
const restrictToVerticalAxis: Modifier = ({ transform }) => ({
  transform: { ...transform, x: 0 }
})
```

Add them to `DndContext`:

```tsx
<DndContext modifiers={[restrictToVerticalAxis]} … />
```

### Collision Detection Algorithms

| Algorithm | Use‑case |
|-----------|----------|
| `pointerWithin` | Default – checks if pointer is inside a droppable |
| `closestCenter` | Picks the droppable closest to the pointer |
| `rectIntersection` | Uses rectangle intersection to detect overlaps |
| `custom` | Pass a custom function that returns a list of `Collision` objects |

---

## Presets

| Preset | Description |
|--------|-------------|
| `@dnd-kit/sortable` | Provides sortable behaviour for lists, grids, nested containers. Uses `@dnd-kit/sortable` strategies: `verticalListSortingStrategy`, `horizontalListSortingStrategy`, `rectSortingStrategy`, etc. |
| `@dnd-kit/virtual` | (planned) – Handles virtualized lists |
| `@dnd-kit/game` | (planned) – 2‑D game‑style drag‑drop |

**Example of sortable list using the preset**

```tsx
<SortableContext items={items} strategy={verticalListSortingStrategy}>
  {items.map(id => <SortableItem key={id} id={id} />)}
</SortableContext>
```

---

## Guides

### Accessibility

* Keyboard support is built‑in – `Enter` to pick up, `Space` to drop.  
* Default `aria` attributes are applied: `role="list"`, `aria-grabbed`, `aria-dropeffect`.  
* Custom screen‑reader announcements can be provided via `DndContext` props `aria` or via your own event listeners.  
* Live regions: `DndContext` automatically updates a hidden `<div aria-live="polite">` with drag state.  

**Tip** – When using `useDraggable`, spread `attributes` and `listeners` onto the root element to ensure the `role`, `tabIndex`, and keyboard events are present.

### Architecture

* **No HTML5 Drag & Drop API** – Avoids the API’s limitations (touch, custom previews, cross‑window dragging).  
* **React‑first** – Uses context, state and hooks; no extra wrapper DOM nodes.  
* **Performance** – Pre‑calculates bounding boxes on drag start, uses `transform` to reposition items, no layout thrashing.

> **When to use an HTML5‑based library** – If you need cross‑browser window dragging or rely heavily on the native API, consider `react-dnd` instead.

### Performance

| Technique | What it does |
|-----------|--------------|
| **Lazy measurement** – Captures all element sizes on drag start. |
| **`transform: translate3d`** – Moves elements without repainting the layout. |
| **Synthetic event listeners** – Sensors use React’s event system, reducing DOM event overhead. |
| **No DOM mutation during drag** – Prefer CSS transforms over direct DOM updates. |

---

## Extensibility

| Extension Point | What you can change |
|------------------|---------------------|
| **Sensors** | Add support for a new input method (e.g., gamepad). |
| **Modifiers** | Constrain movement, add inertia, snap to grid. |
| **Collision detection** | Write custom logic (e.g., fuzzy matching, 3‑D space). |
| **Accessibility** | Override default `aria` attributes, inject custom live‑region announcements. |

---

## Contributing

> @dnd‑kit is an open source project.  
> All contributors should read the [contributing guidelines](https://github.com/clauderic/dnd-kit/blob/main/CONTRIBUTING.md).

### Running the repo locally

```bash
git clone https://github.com/clauderic/dnd-kit.git
cd dnd-kit
npm install
npm run dev   # start the dev server
```

### Tests

```bash
npm test
```

---

## FAQ

**Q: Why is the library 10 KB minified?**  
A: It has no runtime dependencies and relies purely on React’s own context & state.

**Q: Does it support server‑side rendering?**  
A: Yes – the hooks return empty states on the server and update on hydrate.

**Q: Can I use it in a Next.js app?**  
A: Absolutely. Just import `@dnd-kit/core` in a client‑side component.

---

## Further Reading

* [Accessibility guide](https://github.com/clauderic/dnd-kit/blob/main/docs/Accessibility.md)  
* [Performance guide](https://github.com/clauderic/dnd-kit/blob/main/docs/Performance.md)  
* [Sensors & Modifiers](https://github.com/clauderic/dnd-kit/blob/main/docs/Extensibility.md)  

Happy dragging!