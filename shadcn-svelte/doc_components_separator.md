# Separator – shadcn‑svelte

A lightweight component that visually or semantically separates content.  
It supports both horizontal and vertical orientations and can be styled with Tailwind classes.

> **Built by** shadcn  
> **Ported to Svelte** by Huntabyte & CokaKoala

---

## Installation

```bash
# Using pnpm
pnpm dlx shadcn-svelte@latest add separator

# Or with npm / bun / yarn
npm i shadcn-svelte
# then add the component manually
```

---

## Usage

```svelte
<script lang="ts">
  import { Separator } from "$lib/components/ui/separator/index.js";
</script>

<Separator />
```

---

## Props

| Prop          | Type   | Default | Description                                 |
|---------------|--------|---------|---------------------------------------------|
| `orientation` | `string` | `"horizontal"` | `"horizontal"` or `"vertical"` – determines the separator’s direction. |
| `class`       | `string` | – | Tailwind or custom classes to style the separator. |

---

## Examples

### Basic horizontal separator

```svelte
<script lang="ts">
  import { Separator } from "$lib/components/ui/separator/index.js";
</script>

<div>
  <div class="space-y-1">
    <h4 class="text-sm font-medium leading-none">Bits UI Primitives</h4>
    <p class="text-muted-foreground text-sm">
      An open-source UI component library.
    </p>
  </div>

  <!-- Horizontal separator -->
  <Separator class="my-4" />

  <div class="flex h-5 items-center space-x-4 text-sm">
    <div>Blog</div>

    <!-- Vertical separator -->
    <Separator orientation="vertical" />

    <div>Docs</div>

    <!-- Another vertical separator -->
    <Separator orientation="vertical" />

    <div>Source</div>
  </div>
</div>
```

### Vertical separator in a flex container

```svelte
<script lang="ts">
  import { Separator } from "$lib/components/ui/separator/index.js";
</script>

<div class="flex items-center space-x-4">
  <span>Home</span>
  <Separator orientation="vertical" />
  <span>About</span>
  <Separator orientation="vertical" />
  <span>Contact</span>
</div>
```

---

## Notes

- The component is fully responsive and works with any Tailwind utility classes.
- It can be used in any layout where a visual or semantic break is needed.

---