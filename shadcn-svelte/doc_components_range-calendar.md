# RangeCalendar – shadcn‑svelte

The **`<RangeCalendar />`** component lets users pick a start and end date in a single, accessible calendar UI.  
It is a thin wrapper around the Bits Range Calendar component and uses the `@internationalized/date` package for date handling.

> **Tip** – The component is available in the shadcn‑svelte registry.  
> You can add it to your project with the CLI or install it manually.

---

## 📦 Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add range-calendar
# or
npm i -g shadcn-svelte
shadcn-svelte add range-calendar
```

### Manual

```bash
# Add the component to your project
pnpm add @shadcn/svelte
```

> After installation, the component will be located at  
> `src/lib/components/ui/range-calendar/index.js`.

---

## 🚀 Usage

```svelte
<script lang="ts">
  import { getLocalTimeZone, today } from "@internationalized/date";
  import { RangeCalendar } from "$lib/components/ui/range-calendar/index.js";

  // Define the initial range (today → 7 days later)
  const start = today(getLocalTimeZone());
  const end = start.add({ days: 7 });

  // Reactive state for the selected range
  let value = $state({
    start,
    end
  });
</script>

<RangeCalendar bind:value class="rounded-md border" />
```

### What the example does

| Step | Description |
|------|-------------|
| `import { getLocalTimeZone, today }` | Pulls the current time zone and today's date. |
| `import { RangeCalendar }` | Imports the component from the registry. |
| `const start = today(getLocalTimeZone());` | Sets the start of the range to today. |
| `const end = start.add({ days: 7 });` | Sets the end of the range to 7 days after the start. |
| `let value = $state({ start, end });` | Creates a reactive state object that the component will bind to. |
| `<RangeCalendar bind:value ... />` | Renders the calendar and keeps `value` in sync. |

---

## 📚 API Reference

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `value` | `{ start: Date, end: Date }` | `undefined` | Two‑way bound value representing the selected date range. |
| `class` | `string` | `""` | Optional Tailwind classes for styling. |
| `disabled` | `boolean` | `false` | Disables the calendar. |
| `minDate` | `Date` | `undefined` | Minimum selectable date. |
| `maxDate` | `Date` | `undefined` | Maximum selectable date. |
| `locale` | `string` | `"en-US"` | Locale for date formatting. |

> **Note** – The component internally uses the Bits Range Calendar, so any additional props supported by that component are also available.

---

## 🎨 Theming & Dark Mode

The component respects Tailwind’s dark mode utilities.  
Add `dark:` variants to your classes to style it for dark mode.

```html
<RangeCalendar
  bind:value
  class="rounded-md border dark:border-gray-700"
/>
```

---

## 📦 Registry

The component is part of the shadcn‑svelte registry.  
You can view the full registry entry in `registry.json` or `registry-item.json`.

---

## 📦 Example Blocks

The RangeCalendar is showcased in over 30 calendar‑related blocks in the shadcn‑svelte ecosystem.  
Explore them to see the component in various contexts.

---

## 📖 Further Reading

- **Bits Range Calendar** – The underlying component that powers this UI.  
- **@internationalized/date** – The date library used for handling locales and date math.  
- **shadcn‑svelte** – The component library that brings Tailwind‑styled UI components to Svelte.

---

## 📦 Manual Installation (Alternative)

If you prefer not to use the CLI, you can manually copy the component files into your project:

```bash
# Copy the component files
cp -r node_modules/@shadcn/svelte/src/lib/components/ui/range-calendar src/lib/components/ui/
```

Then import it as shown in the usage example.

---

## 📦 License

shadcn‑svelte components are released under the MIT license.  
See the repository’s `LICENSE` file for details.

---

Happy coding! 🎉