# ScrollArea – shadcn‑svelte

`ScrollArea` augments native scrolling with a fully‑styled, cross‑browser scroll container.  
It can be used for vertical, horizontal, or both‑direction scrolling and is fully
accessible.

> **Tip** – The component is part of the official shadcn‑svelte UI registry.  
> Install it with the CLI or manually via npm/pnpm/yarn.

---

## 📦 Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add scroll-area
```

### Manual

```bash
# npm
npm i @shadcn/svelte

# pnpm
pnpm add @shadcn/svelte

# yarn
yarn add @shadcn/svelte
```

---

## 🚀 Usage

```svelte
<script lang="ts">
  import { ScrollArea } from "$lib/components/ui/scroll-area/index.js";
</script>

<ScrollArea class="h-[200px] w-[350px] rounded-md border p-4">
  Jokester began sneaking into the castle in the middle of the night and
  leaving jokes all over the place: under the king's pillow, in his soup,
  even in the royal toilet. The king was furious, but he couldn't seem to
  stop Jokester. And then, one day, the people of the kingdom discovered
  that the jokes left by Jokester were so funny that they couldn't help
  but laugh. And once they started laughing, they couldn't stop.
</ScrollArea>
```

---

## 📚 API Reference

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `orientation` | `"vertical" \| "horizontal" \| "both"` | `"vertical"` | Determines the scroll direction. |
| `class` | `string` | – | Tailwind or custom classes applied to the root element. |
| `style` | `string` | – | Inline styles for the root element. |
| `...rest` | `any` | – | Any other props are forwarded to the root `<div>`. |

> **Note** – The component automatically adds a custom scrollbar track and thumb that
> match the current theme (light/dark).

---

## 📦 Example – Horizontal Scrolling

Set `orientation="horizontal"` to enable horizontal scrolling.

```svelte
<script lang="ts">
  import { ScrollArea } from "$lib/components/ui/scroll-area/index.js";
</script>

<ScrollArea orientation="horizontal" class="h-24 w-full rounded-md border p-4">
  <div class="flex space-x-4">
    <img src="photo1.jpg" alt="Photo 1" class="h-20 w-20 object-cover rounded" />
    <img src="photo2.jpg" alt="Photo 2" class="h-20 w-20 object-cover rounded" />
    <img src="photo3.jpg" alt="Photo 3" class="h-20 w-20 object-cover rounded" />
  </div>
</ScrollArea>
```

---

## 📦 Example – Both Directions

Set `orientation="both"` to enable scrolling in both axes.

```svelte
<script lang="ts">
  import { ScrollArea } from "$lib/components/ui/scroll-area/index.js";
</script>

<ScrollArea orientation="both" class="h-48 w-48 rounded-md border p-4">
  <div class="grid grid-cols-3 gap-2">
    {#each Array(9) as _, i}
      <div class="h-12 w-12 bg-gray-200 rounded flex items-center justify-center">
        {i + 1}
      </div>
    {/each}
  </div>
</ScrollArea>
```

---

## 📦 Example – Vertical List with Separators

```svelte
<script lang="ts">
  import { ScrollArea } from "$lib/components/ui/scroll-area/index.js";
  import { Separator } from "$lib/components/ui/separator/index.js";

  const tags = Array.from({ length: 50 }).map(
    (_, i, a) => `v1.2.0-beta.${a.length - i}`
  );
</script>

<ScrollArea class="h-72 w-48 rounded-md border">
  <div class="p-4">
    <h4 class="mb-4 text-sm font-medium leading-none">Tags</h4>
    {#each tags as tag (tag)}
      <div class="text-sm">{tag}</div>
      <Separator class="my-2" />
    {/each}
  </div>
</ScrollArea>
```

---

## 🔧 Component Source

```svelte
<!-- $lib/components/ui/scroll-area/index.svelte -->
<script lang="ts">
  export let orientation: 'vertical' | 'horizontal' | 'both' = 'vertical';
  export let class: string = '';
  export let style: string = '';
  export let ...rest;
</script>

<div
  class={`relative overflow-hidden ${class}`}
  style={style}
  {...rest}
>
  <div
    class="absolute inset-0 overflow-auto"
    data-orientation={orientation}
  >
    <slot />
  </div>
  <!-- Custom scrollbar styles are injected via Tailwind CSS utilities -->
</div>
```

> The component relies on Tailwind CSS for styling. Ensure your project is
> configured with the required utilities (`overflow-auto`, `relative`,
> `absolute`, etc.).

---

## 📖 Further Reading

- [shadcn‑svelte Documentation](https://shadcn-svelte.com/docs)
- [Tailwind CSS Docs – Scrollbar](https://tailwindcss.com/docs/scrollbar)
- [Accessibility Guide – Scrollable Regions](https://www.w3.org/WAI/ARIA/apg/patterns/scrollable-region/)

---

Happy coding! 🎉