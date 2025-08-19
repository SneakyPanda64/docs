# Slider – shadcn‑svelte

A lightweight, accessible slider component for SvelteKit, Vite, Astro, or any Svelte project.  
It supports single‑thumb and multi‑thumb (range) sliders, custom step values, and a fully‑controlled API.

---

## 📦 Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add slider
```

> **Tip** – The CLI will automatically add the component to your project and update the registry.

### Manual

```bash
# npm
npm i @shadcn/svelte

# pnpm
pnpm add @shadcn/svelte

# yarn
yarn add @shadcn/svelte

# bun
bun add @shadcn/svelte
```

Then import the component:

```ts
<script lang="ts">
  import { Slider } from "$lib/components/ui/slider/index.js";
</script>
```

---

## ⚙️ Usage

```ts
<script lang="ts">
  import { Slider } from "$lib/components/ui/slider/index.js";
  let value = $state(33); // Svelte reactive state
</script>

<Slider type="single" bind:value max={100} step={1} />
```

### Props

| Prop      | Type          | Default | Description |
|-----------|---------------|---------|-------------|
| `type`    | `"single"` \| `"multiple"` | `"single"` | Determines if the slider has one thumb or two. |
| `value`   | `number` \| `number[]` | – | Two‑way bound value. |
| `min`     | `number` | `0` | Minimum value. |
| `max`     | `number` | `100` | Maximum value. |
| `step`    | `number` | `1` | Increment step. |
| `class`   | `string` | – | Tailwind or custom classes. |

---

## 📚 Examples

### 1️⃣ Single Thumb

```ts
<script lang="ts">
  import { Slider } from "$lib/components/ui/slider/index.js";
  let value = $state(50);
</script>

<Slider type="single" bind:value max={100} step={1} class="max-w-[70%]" />
```

### 2️⃣ Multiple Thumbs (Range)

```ts
<script lang="ts">
  import { Slider } from "$lib/components/ui/slider/index.js";
  let value = $state([25, 75]);
</script>

<Slider type="multiple" bind:value max={100} step={1} />
```

---

## 🔧 API Reference

```ts
export interface SliderProps {
  type?: "single" | "multiple";
  value: number | number[];
  min?: number;
  max?: number;
  step?: number;
  class?: string;
}
```

- **`bind:value`** – Two‑way binding.  
- **`type="multiple"`** – Enables a second thumb; `value` must be an array `[min, max]`.  
- **`class`** – Pass Tailwind or custom CSS classes for styling.

---

## 🎨 Theming

The component uses Tailwind CSS utilities.  
To change colors or sizes, override the default classes in your global stylesheet or use Tailwind’s `theme` extensions.

---

## 📦 Registry

The component is registered in `registry.json` and can be imported via:

```ts
import { Slider } from "$lib/components/ui/slider/index.js";
```

---

## 📖 Further Reading

- [Shadcn‑Svelte Docs](https://shadcn-svelte.com/docs)  
- [Tailwind CSS Docs](https://tailwindcss.com/docs)  
- [SvelteKit Docs](https://kit.svelte.dev/docs)

---