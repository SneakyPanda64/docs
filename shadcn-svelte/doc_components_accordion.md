# shadcn‑svelte – Accordion Component

> A vertically stacked set of interactive headings that each reveal a section of content.

---

## Table of Contents

- [Installation](#installation)
  - [CLI](#cli)
  - [Manual](#manual)
- [Usage](#usage)
- [API Reference](#api-reference)
  - [`Accordion.Root`](#accordionroot)
  - [`Accordion.Item`](#accordionitem)
  - [`Accordion.Trigger`](#accordiontrigger)
  - [`Accordion.Content`](#accordioncontent)
- [Examples](#examples)
  - [Basic Accordion](#basic-accordion)
  - [Single‑Open Accordion](#single-open-accordion)
  - [Full‑Featured Accordion](#full‑featured-accordion)
- [Theming & Dark Mode](#theming--dark-mode)
- [Contributing](#contributing)
- [License](#license)

---

## Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add accordion
```

> The command will add the Accordion component to your `$lib/components/ui/accordion` folder and update your `components.json` registry.

### Manual

1. **Add the component files**  
   Copy the entire `accordion` folder from the repository into `$lib/components/ui/accordion`.

2. **Register the component**  
   Add the following entry to your `components.json`:

   ```json
   {
     "name": "accordion",
     "path": "$lib/components/ui/accordion/index.js"
   }
   ```

3. **Install dependencies**  
   The Accordion uses only Tailwind CSS and Svelte. Ensure you have Tailwind v3+ configured.

---

## Usage

```svelte
<script lang="ts">
  import * as Accordion from "$lib/components/ui/accordion/index.js";
</script>

<Accordion.Root type="single" class="w-full sm:max-w-[70%]" value="item-1">
  <Accordion.Item value="item-1">
    <Accordion.Trigger>Product Information</Accordion.Trigger>
    <Accordion.Content class="flex flex-col gap-4 text-balance">
      <p>
        Our flagship product combines cutting‑edge technology with sleek design.
        Built with premium materials, it offers unparalleled performance and
        reliability.
      </p>
      <p>
        Key features include advanced processing capabilities, and an intuitive
        user interface designed for both beginners and experts.
      </p>
    </Accordion.Content>
  </Accordion.Item>

  <Accordion.Item value="item-2">
    <Accordion.Trigger>Shipping Details</Accordion.Trigger>
    <Accordion.Content class="flex flex-col gap-4 text-balance">
      <p>
        We offer worldwide shipping through trusted courier partners. Standard
        delivery takes 3‑5 business days, while express shipping ensures
        delivery within 1‑2 business days.
      </p>
      <p>
        All orders are carefully packaged and fully insured. Track your shipment
        in real‑time through our dedicated tracking portal.
      </p>
    </Accordion.Content>
  </Accordion.Item>

  <Accordion.Item value="item-3">
    <Accordion.Trigger>Return Policy</Accordion.Trigger>
    <Accordion.Content class="flex flex-col gap-4 text-balance">
      <p>
        We stand behind our products with a comprehensive 30‑day return policy.
        If you’re not completely satisfied, simply return the item in its
        original condition.
      </p>
      <p>
        Our hassle‑free return process includes free return shipping and full
        refunds processed within 48 hours of receiving the returned item.
      </p>
    </Accordion.Content>
  </Accordion.Item>
</Accordion.Root>
```

---

## API Reference

### `Accordion.Root`

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `type` | `"single" \| "multiple"` | `"multiple"` | Determines whether only one panel can be open at a time. |
| `value` | `string` | `undefined` | The currently open panel value when `type="single"`. |
| `class` | `string` | `""` | Tailwind or custom classes applied to the root container. |

### `Accordion.Item`

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `value` | `string` | `undefined` | Unique identifier for the accordion item. |
| `class` | `string` | `""` | Custom classes for the item wrapper. |

### `Accordion.Trigger`

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `class` | `string` | `""` | Custom classes for the trigger button. |

### `Accordion.Content`

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `class` | `string` | `""` | Custom classes for the content panel. |

---

## Examples

### Basic Accordion

```svelte
<script lang="ts">
  import * as Accordion from "$lib/components/ui/accordion/index.js";
</script>

<Accordion.Root>
  <Accordion.Item value="item-1">
    <Accordion.Trigger>Is it accessible?</Accordion.Trigger>
    <Accordion.Content>
      Yes. It adheres to the WAI‑ARIA design pattern.
    </Accordion.Content>
  </Accordion.Item>
</Accordion.Root>
```

### Single‑Open Accordion

```svelte
<Accordion.Root type="single" value="item-1">
  <!-- Items as above -->
</Accordion.Root>
```

### Full‑Featured Accordion

> See the **Usage** section above for a complete example with three items, custom classes, and Tailwind styling.

---

## Theming & Dark Mode

The Accordion component is fully theme‑aware. It relies on Tailwind’s `dark:` variants. Example:

```html
<Accordion.Content class="bg-white dark:bg-gray-800 text-gray-900 dark:text-gray-100">
  ...
</Accordion.Content>
```

---

## Contributing

Feel free to open issues or pull requests on the [GitHub repository](https://github.com/shadcn-svelte/shadcn-svelte). Contributions that improve accessibility, add new props, or enhance documentation are welcome.

---

## License

MIT © shadcn-svelte

---