# Hover Card – shadcn‑svelte

> A lightweight, accessible hover‑card component that shows a preview of content when the user hovers over a trigger element.

---

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Examples](#examples)
  - [Minimal Example](#minimal-example)
  - [Full‑Featured Example](#full‑featured-example)
- [API](#api)
- [Related Components](#related-components)

---

## Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add hover-card
```

> The CLI will add the component files to your `$lib/components/ui/hover-card` directory and update your `components.json`.

### Manual

If you prefer to add the component manually, copy the files from the repository into your project and import them as shown below.

```ts
<script lang="ts">
  import * as HoverCard from "$lib/components/ui/hover-card/index.js";
</script>
```

---

## Usage

```svelte
<script lang="ts">
  import * as HoverCard from "$lib/components/ui/hover-card/index.js";
</script>

<HoverCard.Root>
  <HoverCard.Trigger>Hover</HoverCard.Trigger>
  <HoverCard.Content>
    SvelteKit – Web development, streamlined
  </HoverCard.Content>
</HoverCard.Root>
```

> **Tip** – The `HoverCard.Trigger` can be any element (link, button, div, etc.). The `HoverCard.Content` will appear when the trigger is hovered or focused.

---

## Examples

### Minimal Example

```svelte
<script lang="ts">
  import * as HoverCard from "$lib/components/ui/hover-card/index.js";
</script>

<HoverCard.Root>
  <HoverCard.Trigger>Hover</HoverCard.Trigger>
  <HoverCard.Content>
    SvelteKit – Web development, streamlined
  </HoverCard.Content>
</HoverCard.Root>
```

### Full‑Featured Example

```svelte
<script lang="ts">
  import CalendarDaysIcon from "@lucide/svelte/icons/calendar-days";
  import * as Avatar from "$lib/components/ui/avatar/index.js";
  import * as HoverCard from "$lib/components/ui/hover-card/index.js";
</script>

<HoverCard.Root>
  <HoverCard.Trigger
    href="https://github.com/sveltejs"
    target="_blank"
    rel="noreferrer noopener"
    class="rounded-sm underline-offset-4 hover:underline focus-visible:outline-2 focus-visible:outline-offset-8 focus-visible:outline-black"
  >
    @sveltejs
  </HoverCard.Trigger>

  <HoverCard.Content class="w-80">
    <div class="flex justify-between space-x-4">
      <Avatar.Root>
        <Avatar.Image src="https://github.com/sveltejs.png" />
        <Avatar.Fallback>SK</Avatar.Fallback>
      </Avatar.Root>

      <div class="space-y-1">
        <h4 class="text-sm font-semibold">@sveltejs</h4>
        <p class="text-sm">Cybernetically enhanced web apps.</p>

        <div class="flex items-center pt-2">
          <CalendarDaysIcon class="mr-2 size-4 opacity-70" />
          <span class="text-muted-foreground text-xs">
            Joined September 2022
          </span>
        </div>
      </div>
    </div>
  </HoverCard.Content>
</HoverCard.Root>
```

> **Note** – The example uses the `Avatar` component and an icon from `lucide-svelte`. Feel free to replace these with your own UI elements.

---

## API

| Element | Description | Props |
|---------|-------------|-------|
| `HoverCard.Root` | Wrapper that manages the hover state. | `as?: string` – Render as a different element. |
| `HoverCard.Trigger` | The element that triggers the hover card. | `as?: string` – Render as a different element.<br>`href?: string` – If a link.<br>`target?: string` – Link target.<br>`rel?: string` – Link relation. |
| `HoverCard.Content` | The content that appears on hover. | `as?: string` – Render as a different element.<br>`class?: string` – Custom styling. |

> All elements accept the standard Svelte `class`, `style`, and `on:*` event props.

---

## Related Components

- **Accordion** – Collapsible panels.
- **Alert** – Notification messages.
- **Button** – Interactive buttons.
- **Dialog** – Modal dialogs.
- **Popover** – Pop‑up content.
- **Tooltip** – Hover‑only tooltips.

> For a full list of components, see the [shadcn‑svelte component registry](https://github.com/shadcn-svelte/shadcn-svelte/tree/main/src/lib/components/ui).

---

### Quick Reference

```ts
import * as HoverCard from "$lib/components/ui/hover-card/index.js";
```

```svelte
<HoverCard.Root>
  <HoverCard.Trigger>...</HoverCard.Trigger>
  <HoverCard.Content>...</HoverCard.Content>
</HoverCard.Root>
```

That’s all you need to get started with hover cards in your SvelteKit or Vite project!