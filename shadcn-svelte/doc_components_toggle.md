# shadcn‑svelte – Toggle Component

A simple two‑state button that can be either **on** or **off**.  
The component is fully accessible, supports icons, text, and can be styled with Tailwind.

---

## 📦 Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add toggle
```

> **Note**: The command works with `pnpm`, `npm`, `bun`, or `yarn`.

### Manual

```bash
# Using npm
npm install @shadcn/svelte

# Using pnpm
pnpm add @shadcn/svelte

# Using yarn
yarn add @shadcn/svelte
```

---

## 🚀 Usage

```svelte
<script lang="ts">
  import { Toggle } from "$lib/components/ui/toggle/index.js";
</script>

<Toggle>Toggle</Toggle>
```

---

## 🔧 API Reference

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `checked` | `boolean` | `false` | Whether the toggle is on. |
| `disabled` | `boolean` | `false` | Disables the toggle. |
| `on:change` | `Event` | – | Fired when the toggle state changes. |
| `aria-label` | `string` | – | Accessible label for screen readers. |

> **Tip**: Use `aria-label` when the toggle contains only an icon.

---

## 🎨 Examples

Below are a variety of common use‑cases. Each example includes a preview and the corresponding code.

### 1. Default

```svelte
<Toggle>Toggle</Toggle>
```

### 2. Outline

```svelte
<Toggle class="border border-gray-300 rounded-md">Outline</Toggle>
```

### 3. With Text

```svelte
<Toggle>With Text</Toggle>
```

### 4. Italic

```svelte
<Toggle class="italic">Italic</Toggle>
```

### 5. Small

```svelte
<Toggle class="text-sm">Small</Toggle>
```

### 6. Large

```svelte
<Toggle class="text-lg">Large</Toggle>
```

### 7. Disabled

```svelte
<Toggle disabled>Disabled</Toggle>
```

### 8. Icon‑Only (Bold)

```svelte
<script lang="ts">
  import BoldIcon from "@lucide/svelte/icons/bold";
  import { Toggle } from "$lib/components/ui/toggle/index.js";
</script>

<Toggle aria-label="toggle bold">
  <BoldIcon class="size-4" />
</Toggle>
```

### 9. Toggle Group

```svelte
<script lang="ts">
  import { ToggleGroup, Toggle } from "$lib/components/ui/toggle-group/index.js";
</script>

<ToggleGroup type="single" aria-label="text formatting">
  <Toggle value="bold" aria-label="bold">
    <BoldIcon class="size-4" />
  </Toggle>
  <Toggle value="italic" aria-label="italic">
    <ItalicIcon class="size-4" />
  </Toggle>
  <Toggle value="underline" aria-label="underline">
    <UnderlineIcon class="size-4" />
  </Toggle>
</ToggleGroup>
```

### 10. Tooltip

```svelte
<script lang="ts">
  import { Toggle } from "$lib/components/ui/toggle/index.js";
  import { Tooltip } from "$lib/components/ui/tooltip/index.js";
</script>

<Tooltip content="Toggle">
  <Toggle>Toggle</Toggle>
</Tooltip>
```

---

## 📚 Further Reading

- [Component Source](https://github.com/shadcn/svelte/tree/main/src/lib/components/ui/toggle)
- [API Reference](https://github.com/shadcn/svelte/blob/main/src/lib/components/ui/toggle/README.md)
- [Examples Repository](https://github.com/shadcn/svelte/tree/main/examples)

---

> **Built by shadcn. Ported to Svelte by Huntabyte & CokaKoala.**