# shadcn‑svelte – Dropdown Menu

A lightweight, accessible dropdown menu component for SvelteKit, Vite, Astro, or any Svelte project.  
It follows the same design principles as the original shadcn‑ui library and is fully Tailwind‑styled.

---

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [API Reference](#api-reference)
- [Examples](#examples)
- [Changelog](#changelog)

---

## Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add dropdown-menu
```

> **Tip** – The CLI will scaffold the component files into your `$lib/components/ui/dropdown-menu` folder.

### Manual

```bash
# npm
npm i shadcn-svelte

# yarn
yarn add shadcn-svelte

# bun
bun add shadcn-svelte
```

Then copy the component files from the repository into your project.

---

## Usage

```svelte
<script lang="ts">
  import * as DropdownMenu from "$lib/components/ui/dropdown-menu/index.js";
</script>

<DropdownMenu.Root>
  <DropdownMenu.Trigger>Open</DropdownMenu.Trigger>

  <DropdownMenu.Content>
    <DropdownMenu.Group>
      <DropdownMenu.Label>My Account</DropdownMenu.Label>
      <DropdownMenu.Separator />

      <DropdownMenu.Item>Profile</DropdownMenu.Item>
      <DropdownMenu.Item>Billing</DropdownMenu.Item>
      <DropdownMenu.Item>Team</DropdownMenu.Item>
      <DropdownMenu.Item>Subscription</DropdownMenu.Item>
    </DropdownMenu.Group>
  </DropdownMenu.Content>
</DropdownMenu.Root>
```

> **Note** – All sub‑components (`Root`, `Trigger`, `Content`, `Item`, etc.) are exported from the same module.

---

## API Reference

| Component | Props | Description |
|-----------|-------|-------------|
| `DropdownMenu.Root` | `as?: string` | Root wrapper. |
| `DropdownMenu.Trigger` | `as?: string` | Button or element that opens the menu. |
| `DropdownMenu.Content` | `class?: string`, `align?: "start" | "end"` | The dropdown panel. |
| `DropdownMenu.Item` | `disabled?: boolean`, `as?: string` | Clickable menu item. |
| `DropdownMenu.Label` | `as?: string` | Non‑interactive label. |
| `DropdownMenu.Separator` | `as?: string` | Divider line. |
| `DropdownMenu.Sub` | `as?: string` | Container for nested sub‑menus. |
| `DropdownMenu.SubTrigger` | `as?: string` | Trigger for a sub‑menu. |
| `DropdownMenu.SubContent` | `class?: string` | Panel for a sub‑menu. |
| `DropdownMenu.Shortcut` | `as?: string` | Optional keyboard shortcut label. |

> All components accept a `class` prop for custom styling and an optional `as` prop to render a different HTML element.

---

## Examples

### Full‑Featured Menu

```svelte
<script lang="ts">
  import * as DropdownMenu from "$lib/components/ui/dropdown-menu/index.js";
  import { Button } from "$lib/components/ui/button/index.js";
</script>

<DropdownMenu.Root>
  <DropdownMenu.Trigger>
    <Button variant="outline">Open</Button>
  </DropdownMenu.Trigger>

  <DropdownMenu.Content class="w-56" align="start">
    <DropdownMenu.Label>My Account</DropdownMenu.Label>

    <DropdownMenu.Group>
      <DropdownMenu.Item>
        Profile
        <DropdownMenu.Shortcut>⇧⌘P</DropdownMenu.Shortcut>
      </DropdownMenu.Item>

      <DropdownMenu.Item>
        Billing
        <DropdownMenu.Shortcut>⌘B</DropdownMenu.Shortcut>
      </DropdownMenu.Item>

      <DropdownMenu.Item>
        Settings
        <DropdownMenu.Shortcut>⌘S</DropdownMenu.Shortcut>
      </DropdownMenu.Item>

      <DropdownMenu.Item>
        Keyboard shortcuts
        <DropdownMenu.Shortcut>⌘K</DropdownMenu.Shortcut>
      </DropdownMenu.Item>
    </DropdownMenu.Group>

    <DropdownMenu.Separator />

    <DropdownMenu.Group>
      <DropdownMenu.Item>Team</DropdownMenu.Item>

      <DropdownMenu.Sub>
        <DropdownMenu.SubTrigger>Invite users</DropdownMenu.SubTrigger>
        <DropdownMenu.SubContent>
          <DropdownMenu.Item>Email</DropdownMenu.Item>
          <DropdownMenu.Item>Message</DropdownMenu.Item>
          <DropdownMenu.Separator />
          <DropdownMenu.Item>More...</DropdownMenu.Item>
        </DropdownMenu.SubContent>
      </DropdownMenu.Sub>

      <DropdownMenu.Item>
        New Team
        <DropdownMenu.Shortcut>⌘+T</DropdownMenu.Shortcut>
      </DropdownMenu.Item>
    </DropdownMenu.Group>

    <DropdownMenu.Separator />

    <DropdownMenu.Item>GitHub</DropdownMenu.Item>
    <DropdownMenu.Item>Support</DropdownMenu.Item>
    <DropdownMenu.Item disabled>API</DropdownMenu.Item>

    <DropdownMenu.Separator />

    <DropdownMenu.Item>
      Log out
      <DropdownMenu.Shortcut>⇧⌘Q</DropdownMenu.Shortcut>
    </DropdownMenu.Item>
  </DropdownMenu.Content>
</DropdownMenu.Root>
```

### Simple Menu

```svelte
<script lang="ts">
  import * as DropdownMenu from "$lib/components/ui/dropdown-menu/index.js";
</script>

<DropdownMenu.Root>
  <DropdownMenu.Trigger>Open</DropdownMenu.Trigger>

  <DropdownMenu.Content>
    <DropdownMenu.Group>
      <DropdownMenu.Label>My Account</DropdownMenu.Label>
      <DropdownMenu.Separator />

      <DropdownMenu.Item>Profile</DropdownMenu.Item>
      <DropdownMenu.Item>Billing</DropdownMenu.Item>
      <DropdownMenu.Item>Team</DropdownMenu.Item>
      <DropdownMenu.Item>Subscription</DropdownMenu.Item>
    </DropdownMenu.Group>
  </DropdownMenu.Content>
</DropdownMenu.Root>
```

---

## Changelog

- **2024‑10‑30** – Added Tailwind classes to `<DropdownMenu.SubTrigger>`:  
  `gap-2 [&_svg]:pointer-events-none [&_svg]:size-4 [&_svg]:shrink-0`.  
  The icon inside the sub‑trigger no longer needs `size-4`; it is now handled by the parent.

---

### Further Reading

- [Components](https://shadcn-svelte.com/components) – Full list of UI components.  
- [Theming](https://shadcn-svelte.com/theming) – Dark mode, custom themes.  
- [CLI](https://shadcn-svelte.com/cli) – Generate and scaffold components.  

---