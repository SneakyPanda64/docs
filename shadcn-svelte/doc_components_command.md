# shadcn‑svelte – Command Menu

A fast, composable, unstyled command menu for Svelte.  
It is a port of the original `shadcn/ui` command component and works out‑of‑the‑box with Tailwind CSS.

> **Tip** – The component is fully accessible (ARIA) and supports keyboard navigation, shortcuts, and grouping.

---

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Examples](#examples)
  - [Basic Command Menu](#basic-command-menu)
  - [Command Menu with Icons](#command-menu-with-icons)
  - [Command Menu inside a Dialog](#command-menu-inside-a-dialog)
- [Changelog](#changelog)

---

## Installation

### CLI

```bash
# Using pnpm
pnpm dlx shadcn-svelte@latest add command

# Using npm
npm i shadcn-svelte@latest
npx shadcn-svelte add command

# Using bun
bun x shadcn-svelte@latest add command

# Using yarn
yarn dlx shadcn-svelte@latest add command
```

### Manual

```bash
# Install the package
pnpm add shadcn-svelte

# Or
npm i shadcn-svelte
```

After installing, import the component in your Svelte file:

```svelte
<script lang="ts">
  import * as Command from "$lib/components/ui/command/index.js";
</script>
```

---

## Usage

```svelte
<script lang="ts">
  import * as Command from "$lib/components/ui/command/index.js";
</script>

<Command.Root>
  <Command.Input placeholder="Type a command or search..." />
  <Command.List>
    <Command.Empty>No results found.</Command.Empty>

    <Command.Group heading="Suggestions">
      <Command.Item>Calendar</Command.Item>
      <Command.Item>Search Emoji</Command.Item>
      <Command.Item>Calculator</Command.Item>
    </Command.Group>

    <Command.Separator />

    <Command.Group heading="Settings">
      <Command.Item>Profile</Command.Item>
      <Command.Item>Billing</Command.Item>
      <Command.Item>Settings</Command.Item>
    </Command.Group>
  </Command.List>
</Command.Root>
```

> **Note** – The component accepts all props of `<Command.Root>` and `<Command.Input>` (e.g., `class`, `placeholder`, `value`, `on:input`, etc.).

---

## Examples

### Basic Command Menu

```svelte
<script lang="ts">
  import * as Command from "$lib/components/ui/command/index.js";
</script>

<Command.Root>
  <Command.Input placeholder="Type a command or search..." />
  <Command.List>
    <Command.Empty>No results found.</Command.Empty>

    <Command.Group heading="Suggestions">
      <Command.Item>Calendar</Command.Item>
      <Command.Item>Search Emoji</Command.Item>
      <Command.Item>Calculator</Command.Item>
    </Command.Group>

    <Command.Separator />

    <Command.Group heading="Settings">
      <Command.Item>Profile</Command.Item>
      <Command.Item>Billing</Command.Item>
      <Command.Item>Settings</Command.Item>
    </Command.Group>
  </Command.List>
</Command.Root>
```

### Command Menu with Icons

```svelte
<script lang="ts">
  import CalculatorIcon from "@lucide/svelte/icons/calculator";
  import CalendarIcon from "@lucide/svelte/icons/calendar";
  import CreditCardIcon from "@lucide/svelte/icons/credit-card";
  import SettingsIcon from "@lucide/svelte/icons/settings";
  import SmileIcon from "@lucide/svelte/icons/smile";
  import UserIcon from "@lucide/svelte/icons/user";
  import * as Command from "$lib/components/ui/command/index.js";
</script>

<Command.Root class="rounded-lg border shadow-md md:min-w-[450px]">
  <Command.Input placeholder="Type a command or search..." />

  <Command.List>
    <Command.Empty>No results found.</Command.Empty>

    <Command.Group heading="Suggestions">
      <Command.Item>
        <CalendarIcon />
        <span>Calendar</span>
      </Command.Item>

      <Command.Item>
        <SmileIcon />
        <span>Search Emoji</span>
      </Command.Item>

      <Command.Item disabled>
        <CalculatorIcon />
        <span>Calculator</span>
      </Command.Item>
    </Command.Group>

    <Command.Separator />

    <Command.Group heading="Settings">
      <Command.Item>
        <UserIcon />
        <span>Profile</span>
        <Command.Shortcut>⌘P</Command.Shortcut>
      </Command.Item>

      <Command.Item>
        <CreditCardIcon />
        <span>Billing</span>
        <Command.Shortcut>⌘B</Command.Shortcut>
      </Command.Item>

      <Command.Item>
        <SettingsIcon />
        <span>Settings</span>
        <Command.Shortcut>⌘S</Command.Shortcut>
      </Command.Item>
    </Command.Group>
  </Command.List>
</Command.Root>
```

> **Tip** – The `<Command.Item>` component automatically applies `gap-2 [&_svg]:pointer-events-none [&_svg]:size-4 [&_svg]:shrink-0` to style any SVG icons inside.

### Command Menu inside a Dialog

```svelte
<script lang="ts">
  import * as Command from "$lib/components/ui/command/index.js";
  import { onMount } from "svelte";

  let open = $state(false);

  function handleKeydown(e: KeyboardEvent) {
    if (e.key === "k" && (e.metaKey || e.ctrlKey)) {
      e.preventDefault();
      open = !open;
    }
  }
</script>

<svelte:document onkeydown={handleKeydown} />

<Command.Dialog bind:open>
  <Command.Input placeholder="Type a command or search..." />

  <Command.List>
    <Command.Empty>No results found.</Command.Empty>

    <Command.Group heading="Suggestions">
      <Command.Item>Calendar</Command.Item>
      <Command.Item>Search Emoji</Command.Item>
      <Command.Item>Calculator</Command.Item>
    </Command.Group>
  </Command.List>
</Command.Dialog>
```

> **Note** – `<Command.Dialog>` is a wrapper that combines `<Dialog.Root>` and `<Command.Root>`. It accepts props for both components.

---

## Changelog

- **2024‑10‑30** – Added `gap-2 [&_svg]:pointer-events-none [&_svg]:size-4 [&_svg]:shrink-0` to `<Command.Item>` for automatic icon styling.

---

## Further Reading

- [shadcn‑svelte Documentation](https://shadcn-svelte.com/docs)
- [Tailwind CSS](https://tailwindcss.com/)
- [Lucide Icons](https://lucide.dev/)

---