# shadcn‑svelte – `Label` Component

> An accessible label component that can be associated with form controls.

---

## 📦 Installation

```bash
# Using the shadcn‑svelte CLI
pnpm dlx shadcn-svelte@latest add label
```

> **Note**: The CLI will add the component to your `$lib/components/ui/label` folder and update your `components.json`.

---

## 🔧 Usage

```svelte
<script lang="ts">
  import { Label } from "$lib/components/ui/label/index.js";
</script>

<Label for="email">Your email address</Label>
```

> The `for` prop should match the `id` of the associated form control.

---

## 📦 Example: Checkbox with Label

```svelte
<script lang="ts">
  import { Checkbox } from "$lib/components/ui/checkbox/index.js";
  import { Label } from "$lib/components/ui/label/index.js";
</script>

<div>
  <div class="flex items-center space-x-2">
    <Checkbox id="terms" />
    <Label for="terms">Accept terms and conditions</Label>
  </div>
</div>
```

---

## 📚 API Reference

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `for` | `string` | – | The `id` of the form control this label is associated with. |
| `class` | `string` | – | Additional Tailwind or custom classes. |
| `style` | `string` | – | Inline styles. |
| `...rest` | `any` | – | Any other props are forwarded to the underlying `<label>` element. |

---

## 📄 Component Source

```svelte
<script lang="ts" context="module">
  export const name = "Label";
</script>

<script lang="ts">
  export let for: string;
  export let class: string = "";
  export let style: string = "";
  export let ...rest: any;
</script>

<label
  for={for}
  class={`text-sm font-medium text-gray-700 dark:text-gray-300 ${class}`}
  style={style}
  {...rest}
>
  <slot />
</label>
```

---

## 🎨 Theming

The component uses Tailwind CSS classes that automatically adapt to light/dark mode. No additional configuration is required.

---

## 📦 Registry

The `Label` component is available in the shadcn‑svelte registry. You can import it directly from the registry if you prefer:

```svelte
<script lang="ts">
  import { Label } from "shadcn-svelte/ui/label";
</script>
```

---

## 📚 Further Reading

- [Shadcn‑Svelte Documentation](https://shadcn-svelte.com/docs)
- [Tailwind CSS Dark Mode](https://tailwindcss.com/docs/dark-mode)

---