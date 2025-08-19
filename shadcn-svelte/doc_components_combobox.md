# shadcn‑Svelte – Combobox Component

The **Combobox** is a reusable, accessible dropdown that combines the power of the `<Popover />` and `<Command />` components.  
It supports keyboard navigation, search, and can be used inside forms with the `Form.Control` helper.

> **Tip** – The component is built with Svelte 5 and Tailwind v4.  
> For a full list of available components, see the [shadcn‑Svelte registry](https://github.com/shadcn-svelte/shadcn-svelte).

---

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
  - [Basic Example](#basic-example)
  - [Custom Trigger](#custom-trigger)
  - [Form Integration](#form-integration)
- [Examples](#examples)
  - [Combobox](#combobox)
  - [Popover](#popover)
  - [Dropdown Menu](#dropdown-menu)
  - [Form](#form)
- [API](#api)
- [Notes & Gotchas](#notes--gotchas)

---

## Installation

```bash
# Using npm
npm i @shadcn-svelte/ui @lucide/svelte

# Using pnpm
pnpm add @shadcn-svelte/ui @lucide/svelte
```

> **Prerequisites**  
> * SvelteKit or Vite (Svelte 5)  
> * Tailwind CSS v4  
> * `@shadcn-svelte/ui` must be installed for the underlying `Popover` and `Command` components.

---

## Usage

The Combobox is a composition of:

| Component | Purpose |
|-----------|---------|
| `<Popover />` | Handles the dropdown panel positioning and open/close state. |
| `<Command />` | Provides the searchable list and item selection logic. |

Below is a minimal, fully‑functional example.

### Basic Example

```svelte
<script lang="ts">
  import CheckIcon from "@lucide/svelte/icons/check";
  import ChevronsUpDownIcon from "@lucide/svelte/icons/chevrons-up-down";
  import { tick } from "svelte";
  import * as Command from "$lib/components/ui/command/index.js";
  import * as Popover from "$lib/components/ui/popover/index.js";
  import { Button } from "$lib/components/ui/button/index.js";
  import { cn } from "$lib/utils.js";

  const frameworks = [
    { value: "sveltekit", label: "SvelteKit" },
    { value: "next.js", label: "Next.js" },
    { value: "nuxt.js", label: "Nuxt.js" },
    { value: "remix", label: "Remix" },
    { value: "astro", label: "Astro" }
  ];

  let open = $state(false);
  let value = $state("");
  let triggerRef = $state<HTMLButtonElement>(null!);

  const selectedValue = $derived(
    frameworks.find((f) => f.value === value)?.label
  );

  // Refocus the trigger after selection
  function closeAndFocusTrigger() {
    open = false;
    tick().then(() => triggerRef.focus());
  }
</script>

<Popover.Root bind:open>
  <Popover.Trigger bind:ref={triggerRef}>
    <Button
      variant="outline"
      class="w-[200px] justify-between"
      role="combobox"
      aria-expanded={open}
    >
      {selectedValue || "Select a framework…"}
      <ChevronsUpDownIcon class="ml-2 size-4 shrink-0 opacity-50" />
    </Button>
  </Popover.Trigger>

  <Popover.Content class="w-[200px] p-0">
    <Command.Root>
      <Command.Input placeholder="Search framework…" />
      <Command.List>
        <Command.Empty>No framework found.</Command.Empty>
        <Command.Group>
          {#each frameworks as framework}
            <Command.Item
              value={framework.value}
              onSelect={() => {
                value = framework.value;
                closeAndFocusTrigger();
              }}
            >
              <CheckIcon
                class={cn(
                  "mr-2 size-4",
                  value !== framework.value && "text-transparent"
                )}
              />
              {framework.label}
            </Command.Item>
          {/each}
        </Command.Group>
      </Command.List>
    </Command.Root>
  </Popover.Content>
</Popover.Root>
```

### Custom Trigger

If you need a custom trigger (e.g., a different button style or an icon), wrap the trigger content in a `{#snippet}` block:

```svelte
<Popover.Trigger bind:ref={triggerRef}>
  {#snippet child({ props })}
    <Button
      {...props}
      variant="outline"
      class="w-[200px] justify-between"
      role="combobox"
      aria-expanded={open}
    >
      {selectedValue || "Select a framework…"}
      <ChevronsUpDownIcon class="opacity-50" />
    </Button>
  {/snippet}
</Popover.Trigger>
```

### Form Integration

When the Combobox is part of a form, use `Form.Control` to expose a hidden `<input>` that carries the selected value:

```svelte
<script lang="ts">
  import { Form } from "$lib/components/ui/form/index.js";
  // ...rest of the imports
</script>

<Form.Control name="framework">
  <!-- Combobox markup from the basic example -->
</Form.Control>
```

> **Note** – Requires `formsnap` v0.5.0 or newer.

---

## Examples

Below are the full code snippets for the examples shown in the original documentation.

### Combobox

```svelte
<!-- Combobox.svelte -->
<script lang="ts">
  // ...same script as the basic example
</script>

<Popover.Root bind:open>
  <!-- Trigger -->
  <Popover.Trigger bind:ref={triggerRef}>
    <Button
      variant="outline"
      class="w-[200px] justify-between"
      role="combobox"
      aria-expanded={open}
    >
      {selectedValue || "Select a framework…"}
      <ChevronsUpDownIcon class="ml-2 size-4 shrink-0 opacity-50" />
    </Button>
  </Popover.Trigger>

  <!-- Content -->
  <Popover.Content class="w-[200px] p-0">
    <Command.Root>
      <Command.Input placeholder="Search framework…" />
      <Command.List>
        <Command.Empty>No framework found.</Command.Empty>
        <Command.Group>
          {#each frameworks as framework}
            <Command.Item
              value={framework.value}
              onSelect={() => {
                value = framework.value;
                closeAndFocusTrigger();
              }}
            >
              <CheckIcon
                class={cn(
                  "mr-2 size-4",
                  value !== framework.value && "text-transparent"
                )}
              />
              {framework.label}
            </Command.Item>
          {/each}
        </Command.Group>
      </Command.List>
    </Command.Root>
  </Popover.Content>
</Popover.Root>
```

### Popover

```svelte
<!-- Popover.svelte -->
<script lang="ts">
  import { Popover } from "$lib/components/ui/popover/index.js";
  import { Button } from "$lib/components/ui/button/index.js";
</script>

<Popover.Root>
  <Popover.Trigger>
    <Button>Open Popover</Button>
  </Popover.Trigger>
  <Popover.Content>
    <p>This is the popover content.</p>
  </Popover.Content>
</Popover.Root>
```

### Dropdown Menu

```svelte
<!-- DropdownMenu.svelte -->
<script lang="ts">
  import * as DropdownMenu from "$lib/components/ui/dropdown-menu/index.js";
  import { Button } from "$lib/components/ui/button/index.js";
</script>

<DropdownMenu.Root>
  <DropdownMenu.Trigger>
    <Button>Open Menu</Button>
  </DropdownMenu.Trigger>
  <DropdownMenu.Content>
    <DropdownMenu.Item onSelect={() => console.log('Item 1')}>Item 1</DropdownMenu.Item>
    <DropdownMenu.Item onSelect={() => console.log('Item 2')}>Item 2</DropdownMenu.Item>
  </DropdownMenu.Content>
</DropdownMenu.Root>
```

### Form

```svelte
<!-- Form.svelte -->
<script lang="ts">
  import { Form } from "$lib/components/ui/form/index.js";
  // ...Combobox imports
</script>

<Form onSubmit={handleSubmit}>
  <Form.Control name="framework">
    <!-- Combobox markup -->
  </Form.Control>
  <Button type="submit">Submit</Button>
</Form>
```

---

## API

| Prop | Type | Description |
|------|------|-------------|
| `value` | `string` | The current selected value. |
| `onSelect` | `(value: string) => void` | Callback when an item is selected. |
| `placeholder` | `string` | Placeholder text for the search input. |
| `open` | `boolean` | Controlled open state for the popover. |
| `triggerRef` | `HTMLButtonElement` | Reference to the trigger button (used for focus management). |

> All props are forwarded to the underlying `<Command />` and `<Popover />` components.

---

## Notes & Gotchas

- **Keyboard Accessibility** – The Combobox fully supports arrow navigation, `Enter`, and `Esc`.  
- **Focus Management** – After selecting an item, the trigger button receives focus automatically.  
- **Form Integration** – Use `Form.Control` to expose a hidden input; otherwise the value will not be submitted.  
- **Styling** – Tailwind classes are used for sizing and spacing; adjust as needed.  
- **Dependencies** – Ensure `@shadcn-svelte/ui` and `@lucide/svelte` are installed.

---

Happy coding! 🚀