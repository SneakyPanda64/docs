# shadcn‑Svelte – UI Component Library

> **shadcn‑Svelte** is a collection of fully‑styled, accessible UI components for SvelteKit, Vite, Astro and plain Svelte projects.  
> The components are built with Tailwind CSS and follow the design system of shadcn‑UI.

---

## Table of Contents

1. [Installation](#installation)
2. [Usage](#usage)
3. [Examples](#examples)
   * [Select Component](#select-component)
   * [Form Example – Email Selector](#form-example‑email-selector)
4. [Component API](#component-api)
5. [Registry](#registry)
6. [Dark Mode](#dark-mode)
7. [CLI](#cli)
8. [Manual Installation](#manual-installation)
9. [License](#license)

---

## Installation

### Using the CLI

```bash
pnpm dlx shadcn-svelte@latest add <component>
# Example: add the Select component
pnpm dlx shadcn-svelte@latest add select
```

> The CLI will scaffold the component files into your `$lib/components/ui` directory.

### Manual Installation

```bash
# npm
npm i shadcn-svelte

# yarn
yarn add shadcn-svelte

# pnpm
pnpm add shadcn-svelte

# bun
bun add shadcn-svelte
```

After installing, import the component you need:

```ts
<script lang="ts">
  import * as Select from "$lib/components/ui/select/index.js";
</script>
```

---

## Usage

Below is a minimal example of a single‑select dropdown.

```svelte
<script lang="ts">
  import * as Select from "$lib/components/ui/select/index.js";
</script>

<Select.Root type="single">
  <Select.Trigger class="w-[180px]"></Select.Trigger>
  <Select.Content>
    <Select.Item value="light">Light</Select.Item>
    <Select.Item value="dark">Dark</Select.Item>
    <Select.Item value="system">System</Select.Item>
  </Select.Content>
</Select.Root>
```

> The `Select.Root` component manages the state.  
> `Select.Trigger` is the button that opens the dropdown.  
> `Select.Content` contains the list of `Select.Item` elements.

---

## Examples

### 1. Select Component – Fruit Picker

```svelte
<script lang="ts">
  import * as Select from "$lib/components/ui/select/index.js";

  const fruits = [
    { value: "apple", label: "Apple" },
    { value: "banana", label: "Banana" },
    { value: "blueberry", label: "Blueberry" },
    { value: "grapes", label: "Grapes" },
    { value: "pineapple", label: "Pineapple" }
  ];

  let value = $state("");

  const triggerContent = $derived(
    fruits.find((f) => f.value === value)?.label ?? "Select a fruit"
  );
</script>

<Select.Root type="single" name="favoriteFruit" bind:value>
  <Select.Trigger class="w-[180px]">
    {triggerContent}
  </Select.Trigger>
  <Select.Content>
    <Select.Group>
      <Select.Label>Fruits</Select.Label>
      {#each fruits as fruit (fruit.value)}
        <Select.Item
          value={fruit.value}
          label={fruit.label}
          disabled={fruit.value === "grapes"}
        >
          {fruit.label}
        </Select.Item>
      {/each}
    </Select.Group>
  </Select.Content>
</Select.Root>
```

**What it does**

* Renders a dropdown with a list of fruits.
* Disables the “Grapes” option.
* Shows the selected fruit label in the trigger button.

---

### 2. Form Example – Email Selector

> This example demonstrates how to use the `Select` component inside a form.

```svelte
<script lang="ts">
  import * as Select from "$lib/components/ui/select/index.js";

  const emails = [
    { value: "alice@example.com", label: "Alice" },
    { value: "bob@example.com", label: "Bob" },
    { value: "carol@example.com", label: "Carol" }
  ];

  let selectedEmail = $state("");
</script>

<form>
  <label for="email-select">Select a verified email to display</label>
  <Select.Root