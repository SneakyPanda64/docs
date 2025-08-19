# Tooltip – shadcn‑svelte

A lightweight tooltip component that displays contextual information when an element receives keyboard focus or the mouse hovers over it.

> **Note**  
> This component is part of the shadcn‑svelte UI library.  
> It is fully typed and works with SvelteKit, Vite, Astro, or any Svelte project.

---

## Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add tooltip
```

> Replace `pnpm` with `npm`, `bun`, or `yarn` if you prefer.

### Manual

1. Install the package:

   ```bash
   pnpm add shadcn-svelte
   ```

2. Import the component in your Svelte file:

   ```svelte
   <script lang="ts">
     import * as Tooltip from "$lib/components/ui/tooltip/index.js";
   </script>
   ```

---

## Usage

```svelte
<script lang="ts">
  import * as Tooltip from "$lib/components/ui/tooltip/index.js";
</script>

<Tooltip.Provider>
  <Tooltip.Root>
    <Tooltip.Trigger>Hover</Tooltip.Trigger>
    <Tooltip.Content>
      <p>Add to library</p>
    </Tooltip.Content>
  </Tooltip.Root>
</Tooltip.Provider>
```

### With Button Variants

```svelte
<script lang="ts">
  import { buttonVariants } from "../ui/button/index.js";
  import * as Tooltip from "$lib/components/ui/tooltip/index.js";
</script>

<Tooltip.Provider>
  <Tooltip.Root>
    <Tooltip.Trigger class={buttonVariants({ variant: "outline" })}>
      Hover
    </Tooltip.Trigger>
    <Tooltip.Content>
      <p>Add to library</p>
    </Tooltip.Content>
  </Tooltip.Root>
</Tooltip.Provider>
```

---

## API Reference

| Element | Description | Props |
|---------|-------------|-------|
| `<Tooltip.Provider>` | Wraps the tooltip tree and provides context. | `as?: string` – element type (default: `div`). |
| `<Tooltip.Root>` | Root of a single tooltip instance. | `as?: string` – element type (default: `div`). |
| `<Tooltip.Trigger>` | Element that triggers the tooltip. | `as?: string` – element type (default: `button`). |
| `<Tooltip.Content>` | The tooltip panel. | `as?: string` – element type (default: `div`).<br>`side?: "top" | "right" | "bottom" | "left"` – placement (default: `"top"`).<br>`align?: "start" | "center" | "end"` – alignment (default: `"center"`). |

> All components accept the standard Svelte `class`, `style`, and `...$$restProps` attributes.

---

## Customization

You can style the tooltip using Tailwind CSS or any other CSS framework. The component exposes the following CSS classes:

- `tooltip-root`
- `tooltip-trigger`
- `tooltip-content`

Example with Tailwind:

```svelte
<Tooltip.Content class="bg-gray-800 text-white p-2 rounded shadow">
  <p>Custom styled tooltip</p>
</Tooltip.Content>
```

---

## FAQ

- **Can I use the tooltip without a provider?**  
  No, the `<Tooltip.Provider>` is required to establish context for all tooltip instances.

- **Does the tooltip support keyboard navigation?**  
  Yes, it is fully accessible and works with focus and hover events.

- **How do I change the default placement?**  
  Pass the `side` prop to `<Tooltip.Content>`.

---

## Contributing

Feel free to open issues or pull requests on the [GitHub repository](https://github.com/shadcn-svelte/shadcn-svelte). Contributions are welcome!

---