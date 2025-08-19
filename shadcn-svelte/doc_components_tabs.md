# shadcn‑svelte – Tabs Component

The **Tabs** component lets you create a set of layered sections of content (tab panels) that are displayed one at a time.  
It is fully accessible, keyboard‑friendly, and styled with Tailwind CSS.

> **Note**  
> This component is part of the shadcn‑svelte UI library, a port of the original shadcn‑ui components to Svelte.

---

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
  - [Basic Example](#basic-example)
  - [Tabs with Cards](#tabs-with-cards)
- [API Reference](#api-reference)
  - [`Tabs.Root`](#tabsroot)
  - [`Tabs.List`](#tabslist)
  - [`Tabs.Trigger`](#tabstrigger)
  - [`Tabs.Content`](#tabscontent)
- [Theming & Dark Mode](#theming--dark-mode)
- [Contributing](#contributing)
- [License](#license)

---

## Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add tabs
```

> Replace `pnpm` with `npm`, `yarn`, or `bun` if you prefer.

### Manual

```bash
# Using npm
npm install shadcn-svelte

# Import the component
<script lang="ts">
  import * as Tabs from "$lib/components/ui/tabs/index.js";
</script>
```

---

## Usage

### Basic Example

```svelte
<script lang="ts">
  import * as Tabs from "$lib/components/ui/tabs/index.js";
</script>

<Tabs.Root value="account" class="w-[400px]">
  <Tabs.List>
    <Tabs.Trigger value="account">Account</Tabs.Trigger>
    <Tabs.Trigger value="password">Password</Tabs.Trigger>
  </Tabs.List>

  <Tabs.Content value="account">
    Make changes to your account here.
  </Tabs.Content>

  <Tabs.Content value="password">
    Change your password here.
  </Tabs.Content>
</Tabs.Root>
```

### Tabs with Cards

```svelte
<script lang="ts">
  import * as Tabs from "$lib/components/ui/tabs/index.js";
  import * as Card from "$lib/components/ui/card/index.js";
  import { Button } from "$lib/components/ui/button/index.js";
  import { Input } from "$lib/components/ui/input/index.js";
  import { Label } from "$lib/components/ui/label/index.js";
</script>

<div class="flex w-full max-w-sm flex-col gap-6">
  <Tabs.Root value="account">
    <Tabs.List>
      <Tabs.Trigger value="account">Account</Tabs.Trigger>
      <Tabs.Trigger value="password">Password</Tabs.Trigger>
    </Tabs.List>

    <Tabs.Content value="account">
      <Card.Root>
        <Card.Header>
          <Card.Title>Account</Card.Title>
          <Card.Description>
            Make changes to your account here. Click save when you&apos;re done.
          </Card.Description>
        </Card.Header>

        <Card.Content class="grid gap-6">
          <div class="grid gap-3">
            <Label for="tabs-demo-name">Name</Label>
            <Input id="tabs-demo-name" value="Pedro Duarte" />
          </div>

          <div class="grid gap-3">
            <Label for="tabs-demo-username">Username</Label>
            <Input id="tabs-demo-username" value="@peduarte" />
          </div>
        </Card.Content>

        <Card.Footer>
          <Button>Save changes</Button>
        </Card.Footer>
      </Card.Root>
    </Tabs.Content>

    <Tabs.Content value="password">
      <Card.Root>
        <Card.Header>
          <Card.Title>Password</Card.Title>
          <Card.Description>
            Change your password here. After saving, you&apos;ll be logged out.
          </Card.Description>
        </Card.Header>

        <Card.Content class="grid gap-6">
          <div class="grid gap-3">
            <Label for="tabs-demo-current">Current password</Label>
            <Input id="tabs-demo-current" type="password" />
          </div>

          <div class="grid gap-3">
            <Label for="tabs-demo-new">New password</Label>
            <Input id="tabs-demo-new" type="password" />
          </div>
        </Card.Content>

        <Card.Footer>
          <Button>Save password</Button>
        </Card.Footer>
      </Card.Root>
    </Tabs.Content>
  </Tabs.Root>
</div>
```

---

## API Reference

### `Tabs.Root`

The container that holds the entire tab system.

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `value` | `string` | `""` | The currently selected tab value. |
| `class` | `string` | – | Tailwind classes for styling. |

### `Tabs.List`

Container for the tab triggers.

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `class` | `string` | – | Tailwind classes for styling. |

### `Tabs.Trigger`

A button that activates a tab panel.

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `value` | `string` | – | The value that identifies the tab. |
| `class` | `string` | – | Tailwind classes for styling. |

### `Tabs.Content`

The panel that displays content for a selected tab.

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `value` | `string` | – | The value that matches the corresponding trigger. |
| `class` | `string` | – | Tailwind classes for styling. |

---

## Theming & Dark Mode

The component uses Tailwind CSS for styling.  
To enable dark mode, add the `dark:` variants to your Tailwind configuration or use the `class="dark"` on a parent element.

```svelte
<Tabs.Root value="account" class="w-[400px] dark:bg-gray-800">
  <!-- ... -->
</Tabs.Root>
```

---

## Contributing

Contributions are welcome!  
Please read the [contributing guide](https://github.com/shadcn-svelte/shadcn-svelte/blob/main/CONTRIBUTING.md) before submitting a pull request.

---

## License

MIT © shadcn-svelte

---