# Sheet – Side‑panel dialog component

The **Sheet** component is a lightweight, accessible side‑panel dialog that can be anchored to any edge of the viewport. It is a drop‑in replacement for a modal when you need a panel that slides in from the side.

> **Tip** – The component is built on top of the same primitives used by the rest of the shadcn‑svelte UI library, so it follows the same API conventions.

---

## Table of contents

- [Installation](#installation)
- [Usage](#usage)
- [API Reference](#api-reference)
  - [`Sheet.Root`](#sheetroot)
  - [`Sheet.Trigger`](#sheettrigger)
  - [`Sheet.Content`](#sheetcontent)
  - [`Sheet.Header`](#sheetheader)
  - [`Sheet.Title`](#sheettitle)
  - [`Sheet.Description`](#sheetdescription)
  - [`Sheet.Footer`](#sheetfooter)
  - [`Sheet.Close`](#sheetclose)
- [Examples](#examples)
  - [Basic usage](#basic-usage)
  - [Side](#side)
  - [Size](#size)
  - [Custom content](#custom-content)
- [Theming & Dark‑mode](#theming--dark-mode)
- [Contributing](#contributing)

---

## Installation

```bash
# Using pnpm
pnpm dlx shadcn-svelte@latest add sheet

# Or with npm
npm i shadcn-svelte@latest
npx shadcn-svelte add sheet

# Yarn
yarn dlx shadcn-svelte@latest add sheet
```

The command will add the component files to `src/lib/components/ui/sheet` and update your `registry.json`.

---

## Usage

```svelte
<script lang="ts">
  import * as Sheet from "$lib/components/ui/sheet/index.js";
</script>

<Sheet.Root>
  <Sheet.Trigger>Open</Sheet.Trigger>
  <Sheet.Content>
    <Sheet.Header>
      <Sheet.Title>Are you sure absolutely sure?</Sheet.Title>
      <Sheet.Description>
        This action cannot be undone. This will permanently delete your account
        and remove your data from our servers.
      </Sheet.Description>
    </Sheet.Header>
  </Sheet.Content>
</Sheet.Root>
```

---

## API Reference

### `Sheet.Root`

The top‑level component that manages the open/close state.

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `open` | `boolean` | `undefined` | Controlled open state. |
| `on:openChange` | `Event` | – | Fires when the open state changes. |

### `Sheet.Trigger`

The element that opens the sheet. It can be any element; the component will forward the click event.

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `asChild` | `boolean` | `false` | Render the trigger as a child element instead of a `<button>`. |

### `Sheet.Content`

The sliding panel itself.

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `side` | `"top" | "right" | "bottom" | "left"` | `"right"` | Which edge the sheet slides from. |
| `class` | `string` | – | Custom CSS classes (e.g., width). |
| `on:close` | `Event` | – | Fires when the sheet is requested to close. |

### `Sheet.Header`

Container for the title and description.

### `Sheet.Title`

The title of the sheet.

### `Sheet.Description`

A short description or subtitle.

### `Sheet.Footer`

Container for footer actions (e.g., buttons).

### `Sheet.Close`

A button that closes the sheet. It forwards the click event to the root.

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `asChild` | `boolean` | `false` | Render as a child element. |

---

## Examples

### Basic usage

```svelte
<script lang="ts">
  import * as Sheet from "$lib/components/ui/sheet/index.js";
</script>

<Sheet.Root>
  <Sheet.Trigger>Open</Sheet.Trigger>
  <Sheet.Content>
    <Sheet.Header>
      <Sheet.Title>Are you sure absolutely sure?</Sheet.Title>
      <Sheet.Description>
        This action cannot be undone. This will permanently delete your account
        and remove your data from our servers.
      </Sheet.Description>
    </Sheet.Header>
  </Sheet.Content>
</Sheet.Root>
```

### Side

Pass the `side` prop to `<Sheet.Content />` to indicate the edge of the screen where the component will appear. The values can be `top`, `right`, `bottom`, or `left`.

```svelte
<Sheet.Root>
  <Sheet.Trigger>Open</Sheet.Trigger>
  <Sheet.Content side="right">
    <Sheet.Header>
      <Sheet.Title>Are you absolutely sure?</Sheet.Title>
      <Sheet.Description>
        This action cannot be undone. This will permanently delete your account
        and remove your data from our servers.
      </Sheet.Description>
    </Sheet.Header>
  </Sheet.Content>
</Sheet.Root>
```

### Size

You can adjust the size of the sheet using CSS classes:

```svelte
<Sheet.Root>
  <Sheet.Trigger>Open</Sheet.Trigger>
  <Sheet.Content class="w-[400px] sm:w-[540px]">
    <Sheet.Header>
      <Sheet.Title>Are you absolutely sure?</Sheet.Title>
      <Sheet.Description>
        This action cannot be undone. This will permanently delete your account
        and remove your data from our servers.
      </Sheet.Description>
    </Sheet.Header>
  </Sheet.Content>
</Sheet.Root>
```

### Custom content

A more complete example that includes form fields and a footer button.

```svelte
<script lang="ts">
  import * as Sheet from "$lib/components/ui/sheet/index.js";
  import { buttonVariants } from "$lib/components/ui/button/index.js";
  import { Input } from "$lib/components/ui/input/index.js";
  import { Label } from "$lib/components/ui/label/index.js";
</script>

<Sheet.Root>
  <Sheet.Trigger class={buttonVariants({ variant: "outline" })}>Open</Sheet.Trigger>
  <Sheet.Content side="right">
    <Sheet.Header>
      <Sheet.Title>Edit profile</Sheet.Title>
      <Sheet.Description>
        Make changes to your profile here. Click save when you're done.
      </Sheet.Description>
    </Sheet.Header>

    <div class="grid flex-1 auto-rows-min gap-6 px-4">
      <div class="grid gap-3">
        <Label for="name" class="text-right">Name</Label>
        <Input id="name" value="Pedro Duarte" />
      </div>
      <div class="grid gap-3">
        <Label for="username" class="text-right">Username</Label>
        <Input id="username" value="@peduarte" />
      </div>
    </div>

    <Sheet.Footer>
      <Sheet.Close class={buttonVariants({ variant: "outline" })}>Save changes</Sheet.Close>
    </Sheet.Footer>
  </Sheet.Content>
</Sheet.Root>
```

---

## Theming & Dark‑mode

The Sheet component inherits the global Tailwind theme. To support dark mode, simply add the `dark:` variants to your custom classes or use the built‑in `sheet` variant classes that automatically adapt to the current color scheme.

```svelte
<Sheet.Content class="bg-white dark:bg-gray-800 text-gray-900 dark:text-gray-100">
  ...
</Sheet.Content>
```

---

## Contributing

Feel free to open issues or pull requests on the [shadcn‑svelte GitHub repository](https://github.com/shadcn-svelte/shadcn-svelte). All contributions that improve the component, add tests, or enhance documentation are welcome.

---