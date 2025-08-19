# Skeleton Component – shadcn‑svelte

The **Skeleton** component is a lightweight placeholder used while content is loading.  
It can be styled with Tailwind classes to match the shape of the content it represents.

---

## 📦 Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add skeleton
```

> **Note**: The command above will add the component to your project and update the registry.

### Manual

If you prefer to add it manually, copy the component files from the repository into your `src/lib/components/ui/skeleton` folder and import it as shown below.

---

## 📦 Usage

```svelte
<script lang="ts">
  import { Skeleton } from "$lib/components/ui/skeleton/index.js";
</script>

<!-- Example 1: Simple skeleton -->
<Skeleton class="h-[20px] w-[100px] rounded-full" />

<!-- Example 2: Skeleton with avatar and text placeholders -->
<div class="flex items-center space-x-4">
  <Skeleton class="size-12 rounded-full" />
  <div class="space-y-2">
    <Skeleton class="h-4 w-[250px]" />
    <Skeleton class="h-4 w-[200px]" />
  </div>
</div>
```

> The `class` prop accepts any Tailwind utility classes to control size, shape, and spacing.

---

## 📚 Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `class` | `string` | `""` | Tailwind classes applied to the root element. |
| `style` | `string` | `""` | Inline styles applied to the root element. |
| `as` | `string` | `"div"` | The HTML element to render. |

---

## 🎨 Theming

The Skeleton component inherits the current Tailwind theme.  
To change its color, adjust the `bg-gray-200` (or similar) class in your Tailwind config or override it directly:

```svelte
<Skeleton class="h-4 w-32 bg-gray-300 dark:bg-gray-700" />
```

---

## 📦 Registry

The component is registered in `registry.json` under the `ui` namespace.  
If you use the shadcn‑svelte registry, you can import it directly:

```svelte
import { Skeleton } from "$lib/components/ui/skeleton";
```

---

## 📖 Example Project

```svelte
<script lang="ts">
  import { Skeleton } from "$lib/components/ui/skeleton/index.js";
</script>

<div class="flex items-center space-x-4">
  <Skeleton class="size-12 rounded-full" />
  <div class="space-y-2">
    <Skeleton class="h-4 w-[250px]" />
    <Skeleton class="h-4 w-[200px]" />
  </div>
</div>
```

This snippet demonstrates a typical use case: an avatar placeholder next to two lines of text.

---

## 🔗 Related Components

- **Button** – `Button`
- **Card** – `Card`
- **Input** – `Input`
- **Progress** – `Progress`

---

## 📄 License

This component is part of the **shadcn‑svelte** library, licensed under the MIT license.  
See the repository for full licensing details.