# shadcn‑svelte – Aspect Ratio Component

> **Aspect Ratio** – Displays content within a desired ratio.

---

## 📦 Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add aspect-ratio
```

> *You can also use `npm`, `yarn`, or `bun` – the command is the same.*

### Manual

```bash
# Add the component to your project
pnpm add @shadcn/svelte
```

---

## 🚀 Usage

```svelte
<script lang="ts">
  import { AspectRatio } from "$lib/components/ui/aspect-ratio/index.js";
</script>

<div class="w-[450px]">
  <AspectRatio ratio={16 / 9} class="bg-muted">
    <img
      src="https://images.unsplash.com/photo-1588345921523-c2dcdb7f1dcd?w=800&dpr=2&q=80"
      alt="Gray by Drew Beamer"
      class="rounded-md object-cover"
    />
  </AspectRatio>
</div>
```

> The `ratio` prop accepts a numeric value (e.g., `16 / 9`).  
> The component will automatically maintain that aspect ratio for its children.

---

## 📚 API Reference

| Prop   | Type   | Default | Description                                 |
|--------|--------|---------|---------------------------------------------|
| `ratio` | `number` | `1`     | Desired width / height ratio.               |
| `class` | `string` | –       | Optional Tailwind classes for styling.      |

---

## 🎨 Example

```svelte
<script lang="ts">
  import { AspectRatio } from "$lib/components/ui/aspect-ratio/index.js";
</script>

<AspectRatio ratio={4 / 3} class="bg-muted">
  <img
    src="https://images.unsplash.com/photo-1588345921523-c2dcdb7f1dcd?w=800&dpr=2&q=80"
    alt="Gray by Drew Beamer"
    class="h-full w-full rounded-md object-cover"
  />
</AspectRatio>
```

---

## 📖 Further Reading

- [Installation Guide](#installation)
- [Component Source](https://github.com/shadcn/svelte/blob/main/src/lib/components/ui/aspect-ratio/index.svelte)
- [Related Components](#components)

---

*Built by shadcn. Ported to Svelte by Huntabyte & CokaKoala.*