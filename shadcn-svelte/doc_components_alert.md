# shadcn‑svelte – Alert Component

> A lightweight, accessible alert component that can be used to display important messages to users.  
> Built by **shadcn** and ported to Svelte by **Huntabyte & CokaKoala**.

---

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Props & Variants](#props--variants)
- [Examples](#examples)
  - [Default Alert](#default-alert)
  - [Destructive Alert](#destructive-alert)
- [API Reference](#api-reference)
- [Related Components](#related-components)

---

## Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add alert
```

> The CLI will add the component files to your `$lib/components/ui/alert` directory and update your `components.json`.

### Manual

```bash
# Using npm
npm i shadcn-svelte

# Using pnpm
pnpm add shadcn-svelte

# Using yarn
yarn add shadcn-svelte
```

Then copy the `alert` component files from the package into your project’s `$lib/components/ui/alert` folder.

---

## Usage

```svelte
<script lang="ts">
  import * as Alert from "$lib/components/ui/alert/index.js";
</script>

<Alert.Root>
  <Alert.Title>Heads up!</Alert.Title>
  <Alert.Description>
    You can add components to your app using the CLI.
  </Alert.Description>
</Alert.Root>
```

> **Tip:** The component is split into sub‑components (`Root`, `Title`, `Description`) for better composability.

---

## Props & Variants

| Component | Prop | Type | Default | Description |
|-----------|------|------|---------|-------------|
| `Alert.Root` | `variant` | `"default" \| "destructive"` | `"default"` | Determines the visual style. |
| `Alert.Root` | `class` | `string` | – | Custom CSS classes. |
| `Alert.Title` | `class` | `string` | – | Custom CSS classes. |
| `Alert.Description` | `class` | `string` | – | Custom CSS classes. |

> The component uses Tailwind CSS classes internally. Feel free to override or extend them via the `class` prop.

---

## Examples

### Default Alert

```svelte
<script lang="ts">
  import * as Alert from "$lib/components/ui/alert/index.js";
  import CheckCircle2Icon from "@lucide/svelte/icons/check-circle-2";
  import PopcornIcon from "@lucide/svelte/icons/popcorn";
</script>

<div class="grid w-full max-w-xl items-start gap-4">
  <!-- Alert with icon, title, and description -->
  <Alert.Root>
    <CheckCircle2Icon />
    <Alert.Title>Success! Your changes have been saved</Alert.Title>
    <Alert.Description>
      This is an alert with icon, title and description.
    </Alert.Description>
  </Alert.Root>

  <!-- Alert with icon and title only -->
  <Alert.Root>
    <PopcornIcon />
    <Alert.Title>This Alert has a title and an icon. No description.</Alert.Title>
  </Alert.Root>
</div>
```

### Destructive Alert

```svelte
<script lang="ts">
  import * as Alert from "$lib/components/ui/alert/index.js";
  import AlertCircleIcon from "@lucide/svelte/icons/alert-circle";
</script>

<Alert.Root variant="destructive">
  <AlertCircleIcon />
  <Alert.Title>Unable to process your payment.</Alert.Title>
  <Alert.Description>
    <p>Please verify your billing information and try again.</p>
    <ul class="list-inside list-disc text-sm">
      <li>Check your card details</li>
      <li>Ensure sufficient funds</li>
      <li>Verify billing address</li>
    </ul>
  </Alert.Description>
</Alert.Root>
```

---

## API Reference

### `Alert.Root`

```svelte
<Alert.Root
  variant="default | destructive"
  class="string"
>
  <!-- Child components -->
</Alert.Root>
```

- **variant** – `"default"` (blue) or `"destructive"` (red).  
- **class** – Optional Tailwind or custom classes.

### `Alert.Title`

```svelte
<Alert.Title class="string">
  <!-- Title text -->
</Alert.Title>
```

- **class** – Optional Tailwind or custom classes.

### `Alert.Description`

```svelte
<Alert.Description class="string">
  <!-- Description content -->
</Alert.Description>
```

- **class** – Optional Tailwind or custom classes.

---

## Related Components

- **Alert Dialog** – Modal dialog for critical actions.  
- **Button** – For triggering alerts.  
- **Badge** – For status indicators.  
- **Tooltip** – For contextual help.

---

### Quick Start

```svelte
<script lang="ts">
  import * as Alert from "$lib/components/ui/alert/index.js";
  import CheckCircle2Icon from "@lucide/svelte/icons/check-circle-2";
</script>

<Alert.Root>
  <CheckCircle2Icon />
  <Alert.Title>Success!</Alert.Title>
  <Alert.Description>Your changes have been saved.</Alert.Description>
</Alert.Root>
```

> This minimal example demonstrates the core usage of the Alert component. Feel free to customize the icon, text, and variant to fit your design system.