# Checkbox – shadcn‑svelte

A simple, accessible checkbox component that can be used in any form or UI.  
It supports the standard `checked`, `disabled`, and `id` props, and exposes a
`data-[state=checked]` attribute that can be styled with Tailwind.

---

## Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add checkbox
```

> The command will add the component files to your `$lib/components/ui/checkbox`
> directory and update your `components.json` registry.

### Manual

If you prefer to add the component manually, copy the files from the
`checkbox` package into your project and import them as shown below.

---

## Usage

```svelte
<script lang="ts">
  import { Checkbox } from "$lib/components/ui/checkbox/index.js";
</script>

<Checkbox />
```

---

## API Reference

| Prop      | Type     | Default | Description |
|-----------|----------|---------|-------------|
| `id`      | `string` | –       | The `id` attribute for the underlying `<input>` element. |
| `checked` | `boolean` | `false` | Whether the checkbox is checked. |
| `disabled`| `boolean` | `false` | Whether the checkbox is disabled. |
| `class`   | `string` | –       | Additional Tailwind classes to apply to the root element. |

> The component also emits the standard `change` event from the `<input>`.

---

## Examples

### Basic usage with a label

```svelte
<script lang="ts">
  import { Checkbox } from "$lib/components/ui/checkbox/index.js";
  import { Label } from "$lib/components/ui/label/index.js";
</script>

<div class="flex flex-col gap-6">
  <!-- Simple checkbox -->
  <div class="flex items-center gap-3">
    <Checkbox id="terms" />
    <Label for="terms">Accept terms and conditions</Label>
  </div>

  <!-- Checked by default with helper text -->
  <div class="flex items-start gap-3">
    <Checkbox id="terms-2" checked />
    <div class="grid gap-2">
      <Label for="terms-2">Accept terms and conditions</Label>
      <p class="text-muted-foreground text-sm">
        By clicking this checkbox, you agree to the terms and conditions.
      </p>
    </div>
  </div>

  <!-- Disabled state -->
  <div class="flex items-start gap-3">
    <Checkbox id="toggle" disabled />
    <Label for="toggle">Enable notifications</Label>
  </div>

  <!-- Checkbox inside a styled label -->
  <Label
    class="hover:bg-accent/50 flex items-start gap-3 rounded-lg border p-3
           has-[[aria-checked=true]]:border-blue-600
           has-[[aria-checked=true]]:bg-blue-50
           dark:has-[[aria-checked=true]]:border-blue-900
           dark:has-[[aria-checked=true]]:bg-blue-950"
  >
    <Checkbox
      id="toggle-2"
      checked
      class="data-[state=checked]:border-blue-600
             data-[state=checked]:bg-blue-600
             data-[state=checked]:text-white
             dark:data-[state=checked]:border-blue-700
             dark:data-[state=checked]:bg-blue-700"
    />
    <div class="grid gap-1.5 font-normal">
      <p class="text-sm font-medium leading-none">Enable notifications</p>
      <p class="text-muted-foreground text-sm">
        You can enable or disable notifications at any time.
      </p>
    </div>
  </Label>
</div>
```

> **Tip:** The `has-[[aria-checked=true]]` and `data-[state=checked]` classes
> allow you to style the checkbox when it is checked without writing custom
> CSS.

---

## Component Source

```ts
// $lib/components/ui/checkbox/index.js
export { default as Checkbox } from "./checkbox.svelte";
```

The component is a thin wrapper around a native `<input type="checkbox">`
with Tailwind utility classes for styling. Feel free to extend or override
the styles as needed.

---

## Further Reading

- [Tailwind CSS Docs](https://tailwindcss.com/docs)
- [SvelteKit Docs](https://kit.svelte.dev/docs)
- [shadcn‑svelte Registry](https://github.com/shadcn-svelte/registry)

---