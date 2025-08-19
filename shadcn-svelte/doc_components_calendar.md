# shadcn‑svelte Calendar Component

> A lightweight, fully‑featured calendar component for SvelteKit, Vite, Astro, and plain Svelte projects.  
> Built on top of the Bits UI Calendar and the `@internationalized/date` package.

---

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
  - [Basic Date Picker](#basic-date-picker)
  - [Range Calendar](#range-calendar)
  - [Month & Year Selector](#month--year-selector)
  - [Date of Birth Picker](#date-of-birth-picker)
  - [Date & Time Picker](#date--time-picker)
  - [Natural Language Picker](#natural-language-picker)
- [API Reference](#api-reference)
- [Upgrade Guide](#upgrade-guide)
- [Adding Calendar Blocks](#adding-calendar-blocks)
- [License](#license)

---

## Installation

### CLI

```bash
# Using pnpm
pnpm dlx shadcn-svelte@latest add calendar

# Using npm
npm dlx shadcn-svelte@latest add calendar

# Using bun
bun dlx shadcn-svelte@latest add calendar

# Using yarn
yarn dlx shadcn-svelte@latest add calendar
```

### Manual

```bash
# Install the component
npm i @shadcn/svelte

# Add the Calendar component to your project
```

---

## Usage

### Basic Date Picker

```svelte
<script lang="ts">
  import { getLocalTimeZone, today } from "@internationalized/date";
  import { Calendar } from "$lib/components/ui/calendar/index.js";

  let value = today(getLocalTimeZone());
</script>

<Calendar
  type="single"
  bind:value
  class="rounded-md border shadow-sm"
  captionLayout="dropdown"
/>
```

> **Props**
> - `type`: `"single"` | `"range"` – determines if the calendar allows single or range selection.
> - `value`: `Date | null` – the currently selected date(s). Use `bind:value` for two‑way binding.
> - `captionLayout`: `"dropdown"` | `"buttons"` – layout of the month/year selector.

---

### Range Calendar

```svelte
<script lang="ts">
  import { getLocalTimeZone, today } from "@internationalized/date";
  import { Calendar } from "$lib/components/ui/calendar/index.js";

  let value = { start: today(getLocalTimeZone()), end: null };
</script>

<Calendar
  type="range"
  bind:value
  class="rounded-md border shadow-sm"
/>
```

> **Props**
> - `value`: `{ start: Date | null; end: Date | null }` – the selected date range.

---

### Month & Year Selector

```svelte
<script lang="ts">
  import { getLocalTimeZone, today } from "@internationalized/date";
  import { Calendar } from "$lib/components/ui/calendar/index.js";

  let value = today(getLocalTimeZone());
</script>

<Calendar
  type="single"
  bind:value
  class="rounded-md border shadow-sm"
  captionLayout="dropdown"
/>
```

> The `captionLayout="dropdown"` prop turns the month/year selector into a dropdown menu.

---

### Date of Birth Picker

```svelte
<script lang="ts">
  import { getLocalTimeZone, today } from "@internationalized/date";
  import { Calendar } from "$lib/components/ui/calendar/index.js";

  let value = today(getLocalTimeZone());
</script>

<Calendar
  type="single"
  bind:value
  class="rounded-md border shadow-sm"
  captionLayout="dropdown"
/>
```

> Use the same component as the basic date picker; the UI is identical.  
> Add any additional validation logic (e.g., age restrictions) in your form.

---

### Date & Time Picker

```svelte
<script lang="ts">
  import { getLocalTimeZone, today } from "@internationalized/date";
  import { Calendar } from "$lib/components/ui/calendar/index.js";

  let value = today(getLocalTimeZone());
</script>

<Calendar
  type="single"
  bind:value
  class="rounded-md border shadow-sm"
  captionLayout="dropdown"
/>

<!-- Add a separate time picker if needed -->
```

> Combine the calendar with a time picker component (e.g., `<TimePicker />`) to create a full date‑time selector.

---

### Natural Language Picker

```svelte
<script lang="ts">
  import { getLocalTimeZone, today } from "@internationalized/date";
  import { Calendar } from "$lib/components/ui/calendar/index.js";
  import { parse } from "chrono-node";

  let value = today(getLocalTimeZone());
  let input = "";

  function handleInput() {
    const parsed = parse(input, new Date());
    if (parsed.length) {
      value = parsed[0].start.date();
    }
  }
</script>

<input
  type="text"
  bind:value={input}
  on:input={handleInput}
  placeholder="e.g., next Friday at 3pm"
/>

<Calendar
  type="single"
  bind:value
  class="rounded-md border shadow-sm"
/>
```

> The `chrono-node` library parses natural language dates and updates the calendar accordingly.

---

## API Reference

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `type` | `"single"` \| `"range"` | `"single"` | Determines if the calendar allows single or range selection. |
| `value` | `Date \| null` \| `{ start: Date \| null; end: Date \| null }` | `null` | The currently selected date(s). Use `bind:value` for two‑way binding. |
| `captionLayout` | `"dropdown"` \| `"buttons"` | `"buttons"` | Layout of the month/year selector. |
| `class` | `string` | `""` | Custom CSS classes. |
| `style` | `string` | `""` | Inline styles. |

> **Events**
> - `on:select` – emitted when a date is selected. Payload: `{ date: Date }` or `{ start: Date, end: Date }`.

---

## Upgrade Guide

To upgrade to the latest version of the `<Calendar />` component:

```bash
# Using pnpm
pnpm dlx shadcn-svelte@latest add calendar

# Using npm
npm dlx shadcn-svelte@latest add calendar

# Using bun
bun dlx shadcn-svelte@latest add calendar

# Using yarn
yarn dlx shadcn-svelte@latest add calendar
```

When prompted to overwrite existing files, select **Yes**.  
If you have customized the component, merge your changes with the new version manually.

---

## Adding Calendar Blocks

After upgrading the Calendar component, you can add the latest calendar blocks:

```bash
# Using pnpm
pnpm dlx shadcn-svelte@latest add calendar-02

# Using npm
npm dlx shadcn-svelte@latest add calendar-02

# Using bun
bun dlx shadcn-svelte@latest add calendar-02

# Using yarn
yarn dlx shadcn-svelte@latest add calendar-02
```

These blocks provide pre‑built UI patterns (e.g., date picker, range picker, month/year selector) that you can drop into your project.

---

## License

MIT © shadcn-svelte

---