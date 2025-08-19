# Progress Component – shadcn‑svelte

The **Progress** component renders a horizontal progress bar that can be used to indicate the completion state of a task.  
It is a thin wrapper around a native `<progress>` element styled with Tailwind CSS.

> **Built by** shadcn – **ported to Svelte** by Huntabyte & CokaKoala.

---

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [API Reference](#api-reference)
- [Examples](#examples)
- [Related Components](#related-components)

---

## Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add progress
```

> The command will add the component files to your `$lib/components/ui/progress` folder and update your `components.json` automatically.

### Manual

1. **Add the component files**  
   Copy the following files into your project:

   - `src/lib/components/ui/progress/index.js`
   - `src/lib/components/ui/progress/progress.svelte`

2. **Add the Tailwind CSS utilities**  
   Ensure your Tailwind config includes the default shadcn styles (or import the component’s CSS directly).

---

## Usage

```svelte
<script lang="ts">
  import { Progress } from "$lib/components/ui/progress/index.js";
</script>

<Progress value={33} />
```

> The component accepts a `value` prop that represents the current progress.  
> By default the maximum value is `100`.

---

## API Reference

| Prop   | Type   | Default | Description |
|--------|--------|---------|-------------|
| `value` | `number` | `0` | Current progress value. |
| `max`   | `number` | `100` | Maximum progress value. |
| `class` | `string` | `""` | Additional Tailwind classes for styling. |
| `style` | `string` | `""` | Inline styles. |
| `ariaLabel` | `string` | `"progress"` | Accessible label for screen readers. |

> All other native `<progress>` attributes are forwarded automatically.

---

## Examples

### 1. Dynamic Progress Update

```svelte
<script lang="ts">
  import { onMount } from "svelte";
  import { Progress } from "$lib/components/ui/progress/index.js";

  let value = $state(13);

  onMount(() => {
    const timer = setTimeout(() => (value = 66), 500);
    return () => clearTimeout(timer);
  });
</script>

<Progress {value} max={100} class="w-[60%]" />
```

*The progress bar starts at 13 % and jumps to 66 % after 500 ms.*

### 2. Simple Static Usage

```svelte
<script lang="ts">
  import { Progress } from "$lib/components/ui/progress/index.js";
</script>

<Progress value={33} />
```

*Shows a progress bar at 33 %.*

---

## Related Components

- **Alert** – Show status messages.
- **Button** – Trigger actions.
- **Skeleton** – Loading placeholders.
- **Spinner** – Indeterminate loading indicator.

---

### License

This component is part of the **shadcn‑svelte** library, licensed under the MIT license.  
Feel free to customize the styles or extend the component to fit your project’s needs.