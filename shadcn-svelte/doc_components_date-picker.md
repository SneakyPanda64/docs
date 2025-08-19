# shadcn‑Svelte – Date Picker

The **Date Picker** is a reusable component that lets users select a single date or a date range.  
It is built by composing the `Popover`, `Calendar`, and `RangeCalendar` components.

> **Tip** – All examples below are written in **TypeScript** and use the Svelte 5 syntax.

---

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Examples](#examples)
  - [Single Date Picker](#single-date-picker)
  - [Date Range Picker](#date-range-picker)
  - [With Presets](#with-presets)
  - [Form Integration](#form-integration)
- [Component API](#component-api)
- [Dependencies](#dependencies)

---

## Installation

```bash
# Using npm
npm i @shadcn-svelte/ui @internationalized/date @lucide/svelte

# Using pnpm
pnpm add @shadcn-svelte/ui @internationalized/date @lucide/svelte
```

> **Note**  
> The Date Picker relies on the following components:
> - `Popover` (`@shadcn-svelte/ui/popover`)
> - `Calendar` (`@shadcn-svelte/ui/calendar`)
> - `RangeCalendar` (`@shadcn-svelte/ui/range-calendar`)

Make sure those packages are installed as well.

---

## Usage

Below is a minimal example that shows how to wire a single‑date picker into a Svelte component.

```svelte
<script lang="ts">
  import CalendarIcon from "@lucide/svelte/icons/calendar";
  import {
    type DateValue,
    DateFormatter,
    getLocalTimeZone,
  } from "@internationalized/date";
  import { cn } from "$lib/utils.js";
  import { Button } from "$lib/components/ui/button/index.js";
  import { Calendar } from "$lib/components/ui/calendar/index.js";
  import * as Popover from "$lib/components/ui/popover/index.js";

  const df = new DateFormatter("en-US", { dateStyle: "long" });

  // Reactive state for the selected date
  let value = $state<DateValue>();
</script>

<Popover.Root>
  <Popover.Trigger>
    <Button
      variant="outline"
      class={cn(
        "w-[280px] justify-start text-left font-normal",
        !value && "text-muted-foreground"
      )}
    >
      <CalendarIcon class="mr-2 size-4" />
      {value ? df.format(value.toDate(getLocalTimeZone())) : "Select a date"}
    </Button>
  </Popover.Trigger>

  <Popover.Content class="w-auto p-0">
    <Calendar bind:value type="single" initialFocus />
  </Popover.Content>
</Popover.Root>
```

### Key Points

| Feature | Description |
|---------|-------------|
| `Popover.Root` | Wraps the trigger and content. |
| `Popover.Trigger` | The element that opens the popover. |
| `Popover.Content` | The popover panel that contains the calendar. |
| `Calendar` | The calendar UI. |
| `type="single"` | Configures the calendar for a single date selection. |
| `bind:value` | Two‑way binding to the selected date. |
| `initialFocus` | Automatically focuses the calendar when the popover opens. |

---

## Examples

### 1. Single Date Picker

```svelte
<script lang="ts">
  import CalendarIcon from "@lucide/svelte/icons/calendar";
  import {
    type DateValue,
    DateFormatter,
    getLocalTimeZone,
  } from "@internationalized/date";
  import { cn } from "$lib/utils.js";
  import { Button } from "$lib/components/ui/button/index.js";
  import { Calendar } from "$lib/components/ui/calendar/index.js";
  import * as Popover from "$lib/components/ui/popover/index.js";

  const df = new DateFormatter("en-US", { dateStyle: "long" });

  let value = $state<DateValue>();
</script>

<Popover.Root>
  <Popover.Trigger>
    <Button
      variant="outline"
      class={cn(
        "w-[280px] justify-start text-left font-normal",
        !value && "text-muted-foreground"
      )}
    >
      <CalendarIcon class="mr-2 size-4" />
      {value ? df.format(value.toDate(getLocalTimeZone())) : "Select a date"}
    </Button>
  </Popover.Trigger>

  <Popover.Content class="w-auto p-0">
    <Calendar bind:value type="single" initialFocus />
  </Popover.Content>
</Popover.Root>
```

---

### 2. Date Range Picker

```svelte
<script lang="ts">
  import CalendarIcon from "@lucide/svelte/icons/calendar";
  import {
    type DateValue,
    DateFormatter,
    getLocalTimeZone,
  } from "@internationalized/date";
  import { cn } from "$lib/utils.js";
  import { Button } from "$lib/components/ui/button/index.js";
  import { RangeCalendar } from "$lib/components/ui/range-calendar/index.js";
  import * as Popover from "$lib/components/ui/popover/index.js";

  const df = new DateFormatter("en-US", { dateStyle: "long" });

  // `value` is a tuple: [start, end]
  let value = $state<[DateValue, DateValue]>();
</script>

<Popover.Root>
  <Popover.Trigger>
    <Button
      variant="outline"
      class={cn(
        "w-[280px] justify-start text-left font-normal",
        !value && "text-muted-foreground"
      )}
    >
      <CalendarIcon class="mr-2 size-4" />
      {value
        ? `${df.format(value[0].toDate(getLocalTimeZone()))} – ${df.format(value[1].toDate(getLocalTimeZone()))}`
        : "Select a range"}
    </Button>
  </Popover.Trigger>

  <Popover.Content class="w-auto p-0">
    <RangeCalendar bind:value initialFocus />
  </Popover.Content>
</Popover.Root>
```

---

### 3. With Presets

```svelte
<script lang="ts">
  import CalendarIcon from "@lucide/svelte/icons/calendar";
  import {
    type DateValue,
    DateFormatter,
    getLocalTimeZone,
  } from "@internationalized/date";
  import { cn } from "$lib/utils.js";
  import { Button } from "$lib/components/ui/button/index.js";
  import { Calendar } from "$lib/components/ui/calendar/index.js";
  import * as Popover from "$lib/components/ui/popover/index.js";

  const df = new DateFormatter("en-US", { dateStyle: "long" });

  let value = $state<DateValue>();
</script>

<Popover.Root>
  <Popover.Trigger>
    <Button
      variant="outline"
      class={cn(
        "w-[280px] justify-start text-left font-normal",
        !value && "text-muted-foreground"
      )}
    >
      <CalendarIcon class="mr-2 size-4" />
      {value ? df.format(value.toDate(getLocalTimeZone())) : "Pick a date"}
    </Button>
  </Popover.Trigger>

  <Popover.Content class="w-auto p-0">
    <Calendar bind:value type="single" initialFocus />
  </Popover.Content>
</Popover.Root>
```

> **Note** – The preset logic (e.g., “Today”, “Last 7 days”) is typically added via a custom UI or a separate component. The example above shows the base picker; you can extend it with preset buttons that set `value` accordingly.

---

### 4. Form Integration

```svelte
<script lang="ts">
  import CalendarIcon from "@lucide/svelte/icons/calendar";
  import {
    type DateValue,
    DateFormatter,
    getLocalTimeZone,
  } from "@internationalized/date";
  import { cn } from "$lib/utils.js";
  import { Button } from "$lib/components/ui/button/index.js";
  import { Calendar } from "$lib/components/ui/calendar/index.js";
  import * as Popover from "$lib/components/ui/popover/index.js";

  const df = new DateFormatter("en-US", { dateStyle: "long" });

  let value = $state<DateValue>();
</script>

<form>
  <label class="block text-sm font-medium text-gray-700 mb-1">
    Date of birth
  </label>

  <Popover.Root>
    <Popover.Trigger>
      <Button
        variant="outline"
        class={cn(
          "w-[280px] justify-start text-left font-normal",
          !value && "text-muted-foreground"
        )}
      >
        <CalendarIcon class="mr-2 size-4" />
        {value ? df.format(value.toDate(getLocalTimeZone())) : "Pick a date"}
      </Button>
    </Popover.Trigger>

    <Popover.Content class="w-auto p-0">
      <Calendar bind:value type="single" initialFocus />
    </Popover.Content>
  </Popover.Root>

  <p class="text-sm text-gray-500 mt-1">
    Your date of birth is used to calculate your age.
  </p>

  <Button type="submit" class="mt-4">
    Submit
  </Button>
</form>
```

---

## Component API

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `type` | `"single" | "range"` | `"single"` | Determines whether the calendar shows a single date or a range. |
| `value` | `DateValue | [DateValue, DateValue]` | `undefined` | Two‑way bound value. |
| `initialFocus` | `boolean` | `false` | If `true`, the calendar receives focus when the popover opens. |
| `class` | `string` | `""` | Custom CSS classes. |
| `style` | `string` | `""` | Inline styles. |

> **Tip** – The `Calendar` component internally uses the `@internationalized/date` library for date handling.

---

## Dependencies

| Package | Purpose |
|---------|---------|
| `@shadcn-svelte/ui` | Core UI components (`Popover`, `Calendar`, `RangeCalendar`, `Button`, etc.) |
| `@internationalized/date` | Date manipulation and formatting |
| `@lucide/svelte` | Icons (e.g., `CalendarIcon`) |
| `svelte` | Framework |
| `tailwindcss` | Styling (optional but recommended) |

---

### Final Thoughts

The Date Picker is a lightweight, composable component that can be dropped into any Svelte project. By leveraging the `Popover` and `Calendar` components, you get a fully accessible, theme‑aware picker with minimal boilerplate. Happy coding!