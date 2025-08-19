# Popover – shadcn‑svelte

A lightweight, accessible popover component that renders its content in a portal.  
It can be used for tooltips, dropdowns, modal‑like overlays, or any UI that needs a floating panel.

> **Note** – The component is part of the official shadcn‑svelte UI library.  
> It is fully Tailwind‑styled and works out‑of‑the‑box with SvelteKit, Vite, Astro, or any Svelte project.

---

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Examples](#examples)
  - [Basic Popover](#basic-popover)
  - [Popover with Form Controls](#popover-with-form-controls)
- [API Reference](#api-reference)
- [Component Source](#component-source)
- [Related Components](#related-components)

---

## Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add popover
```

> The command will add the component files to your `$lib/components/ui/popover` folder and update your `components.json`.

### Manual

1. Copy the files from the repository into your project under `$lib/components/ui/popover`.
2. Import the component in your Svelte files:

```ts
<script lang="ts">
  import * as Popover from "$lib/components/ui/popover/index.js";
</script>
```

---

## Usage

```svelte
<script lang="ts">
  import * as Popover from "$lib/components/ui/popover/index.js";
</script>

<Popover.Root>
  <Popover.Trigger>Open</Popover.Trigger>
  <Popover.Content>
    Place content for the popover here.
  </Popover.Content>
</Popover.Root>
```

The component exposes the following slots:

| Slot          | Description                                 |
|---------------|---------------------------------------------|
| `Trigger`     | The element that toggles the popover.       |
| `Content`     | The floating panel that appears.           |
| `Arrow` (optional) | An optional arrow element. |

---

## Examples

### Basic Popover

```svelte
<script lang="ts">
  import * as Popover from "$lib/components/ui/popover/index.js";
</script>

<Popover.Root>
  <Popover.Trigger class="btn btn-outline">Open</Popover.Trigger>
  <Popover.Content class="w-80">
    <p>This is a simple popover.</p>
  </Popover.Content>
</Popover.Root>
```

### Popover with Form Controls

```svelte
<script lang="ts">
  import { buttonVariants } from "$lib/components/ui/button/index.js";
  import { Input } from "$lib/components/ui/input/index.js";
  import { Label } from "$lib/components/ui/label/index.js";
  import * as Popover from "$lib/components/ui/popover/index.js";
</script>

<Popover.Root>
  <Popover.Trigger class={buttonVariants({ variant: "outline" })}>
    Open
  </Popover.Trigger>

  <Popover.Content class="w-80">
    <div class="grid gap-4">
      <div class="space-y-2">
        <h4 class="font-medium leading-none">Dimensions</h4>
        <p class="text-muted-foreground text-sm">
          Set the dimensions for the layer.
        </p>
      </div>

      <div class="grid gap-2">
        <div class="grid grid-cols-3 items-center gap-4">
          <Label for="width">Width</Label>
          <Input id="width" value="100%" class="col-span-2 h-8" />
        </div>

        <div class="grid grid-cols-3 items-center gap-4">
          <Label for="maxWidth">Max. width</Label>
          <Input id="maxWidth" value="300px" class="col-span-2 h-8" />
        </div>

        <div class="grid grid-cols-3 items-center gap-4">
          <Label for="height">Height</Label>
          <Input id="height" value="25px" class="col-span-2 h-8" />
        </div>

        <div class="grid grid-cols-3 items-center gap-4">
          <Label for="maxHeight">Max. height</Label>
          <Input id="maxHeight" value="none" class="col-span-2 h-8" />
        </div>
      </div>
    </div>
  </Popover.Content>
</Popover.Root>
```

---

## API Reference

| Prop / Attribute | Type   | Default | Description |
|------------------|--------|---------|-------------|
| `open` (optional) | `boolean` | `undefined` | Controls the open state when used as a controlled component. |
| `on:openChange` | `Event` | – | Fires when the popover opens or closes. |
| `side` | `"top" | "right" | "bottom" | "left"` | `"bottom"` | Preferred side of the popover relative to the trigger. |
| `align` | `"start" | "center" | "end"` | `"center"` | Alignment of the popover on the chosen side. |
| `offset` | `number` | `0` | Distance in pixels between the trigger and the popover. |
| `collisionPadding` | `number` | `8` | Padding to keep the popover inside the viewport. |
| `class` | `string` | – | Custom Tailwind classes for the content. |
| `style` | `string` | – | Inline styles for the content. |

> The component internally uses `@floating-ui/dom` for positioning and `svelte:portal` for rendering the content in a portal.

---

## Component Source

The source code is available in the repository under:

```
$lib/components/ui/popover/
```

Feel free to inspect or modify the implementation to fit your needs.

---

## Related Components

- **Dialog** – Modal dialog with focus trapping.
- **Dropdown Menu** – Similar to a popover but with built‑in menu items.
- **Tooltip** – A lightweight popover variant for hover interactions.

---

### Quick Start

```bash
# Add the component
pnpm dlx shadcn-svelte@latest add popover

# Import and use
<script lang="ts">
  import * as Popover from "$lib/components/ui/popover/index.js";
</script>

<Popover.Root>
  <Popover.Trigger>Open</Popover.Trigger>
  <Popover.Content>Content goes here.</Popover.Content>
</Popover.Root>
```

Enjoy building accessible, styled popovers with shadcn‑svelte!