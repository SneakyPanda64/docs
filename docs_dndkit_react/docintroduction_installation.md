# @dnd‑kit  
A modern, lightweight drag‑and‑drop toolkit for React.

---

## 📦 Installation

### Core Library

```bash
npm install @dnd-kit/core
```

`@dnd-kit/core` contains the minimal set of building blocks needed for almost all drag‑and‑drop use‑cases:

* **Context provider** – `DndContext`
* **Hooks** – `useDraggable`, `useDroppable`, `useDragOverlay`
* **Sensors** – pointer, mouse, touch, keyboard
* **Accessibility helpers**
* **Modifiers** – for dynamic motion adjustments

#### Peer dependencies

`@dnd-kit/core` requires React. If your project does not yet have React, install it as a peer dependency:

```bash
npm install react react-dom
```

---

## 📦 Packages

`@dnd-kit` is a monorepo. Depending on your needs, you may also want to add one or more of the following sub‑packages.

| Package | What it adds | Install command |
|---------|--------------|-----------------|
| `@dnd-kit/core` | Core drag‑and‑drop building blocks | `npm install @dnd-kit/core` |
| `@dnd-kit/modifiers` | Ready‑to‑use modifiers for motion control | `npm install @dnd-kit/modifiers` |
| `@dnd-kit/sortable` | Optimized API for sortable lists | `npm install @dnd-kit/sortable` |

### Modifiers

Modifiers let you dynamically adjust the coordinates reported by sensors. Use them for:

* Constraining movement to a single axis  
* Keeping the draggable within its container’s bounds  
* Adding resistance or clamping behavior  

> **Tip:** Apply modifiers either on `DndContext` (global) or on a `DraggableClone` for per‑item logic.

```bash
npm install @dnd-kit/modifiers
```

---

### Sortable

While the core library can build a sortable list from scratch, `@dnd-kit/sortable` offers a convenient, pre‑built API that handles the heavy lifting:

```bash
npm install @dnd-kit/sortable
```

* Smooth, flexible drag‑and‑drop sorting  
* Accessible out‑of‑the‑box  
* Tiny runtime footprint

---

## 🚀 Development releases

Every merge into the `main` branch triggers a new development build that is published to npm under the `next` tag.  
To try the latest development versions:

```bash
npm install @dnd-kit/core@next @dnd-kit/sortable@next
```

> **Note:** Development releases are unstable. If you plan to use them in production, lock to a specific tag instead of `next`.

---

## 📚 API Documentation (quick overview)

| Component | Purpose | Typical usage |
|-----------|---------|---------------|
| `<DndContext>` | Provides the drag‑and‑drop environment | Wrap your app or the draggable area |
| `useDraggable` | Makes an element draggable | Attach to any component |
| `useDroppable` | Makes an element a drop target | Attach to any component |
| `useDragOverlay` | Renders an overlay while dragging | Optional, for visual feedback |
| Sensors (`pointer`, `mouse`, `touch`, `keyboard`) | Capture input events | Passed to `DndContext` |
| Modifiers | Alter motion coordinates | Passed as an array to `DndContext` |

For full API details, refer to the generated documentation inside each package or visit the official docs site.

---

*Last updated: 2023*