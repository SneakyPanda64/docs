# shadcn‑svelte – Alert Dialog

> A modal dialog that interrupts the user with important content and expects a response.

---

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Examples](#examples)
- [API Reference](#api-reference)
- [Component Source](#component-source)

---

## Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add alert-dialog
```

> **Supported package managers**  
> `pnpm`, `npm`, `bun`, `yarn`

### Manual

```bash
# Using npm
npm install @shadcn/svelte

# Or using yarn
yarn add @shadcn/svelte
```

> After installation, import the component from the generated `$lib/components/ui/alert-dialog` path.

---

## Usage

```svelte
<script lang="ts">
  import * as AlertDialog from "$lib/components/ui/alert-dialog/index.js";
</script>

<AlertDialog.Root>
  <AlertDialog.Trigger>Open</AlertDialog.Trigger>

  <AlertDialog.Content>
    <AlertDialog.Header>
      <AlertDialog.Title>Are you absolutely sure?</AlertDialog.Title>
      <AlertDialog.Description>
        This action cannot be undone. This will permanently delete your account
        and remove your data from our servers.
      </AlertDialog.Description>
    </AlertDialog.Header>

    <AlertDialog.Footer>
      <AlertDialog.Cancel>Cancel</AlertDialog.Cancel>
      <AlertDialog.Action>Continue</AlertDialog.Action>
    </AlertDialog.Footer>
  </AlertDialog.Content>
</AlertDialog.Root>
```

> **Tip** – If you’re using Tailwind’s `buttonVariants` helper, you can style the trigger like this:

```svelte
<script lang="ts">
  import * as AlertDialog from "$lib/components/ui/alert-dialog/index.js";
  import { buttonVariants } from "$lib/components/ui/button/index.js";
</script>

<AlertDialog.Root>
  <AlertDialog.Trigger class={buttonVariants({ variant: "outline" })}>
    Show Dialog
  </AlertDialog.Trigger>
  <!-- …rest of the dialog… -->
</AlertDialog.Root>
```

---

## Examples

### 1. Basic Alert Dialog

```svelte
<script lang="ts">
  import * as AlertDialog from "$lib/components/ui/alert-dialog/index.js";
</script>

<AlertDialog.Root>
  <AlertDialog.Trigger>Open</AlertDialog.Trigger>

  <AlertDialog.Content>
    <AlertDialog.Header>
      <AlertDialog.Title>Are you absolutely sure?</AlertDialog.Title>
      <AlertDialog.Description>
        This action cannot be undone. This will permanently delete your account
        and remove your data from our servers.
      </AlertDialog.Description>
    </AlertDialog.Header>

    <AlertDialog.Footer>
      <AlertDialog.Cancel>Cancel</AlertDialog.Cancel>
      <AlertDialog.Action>Continue</AlertDialog.Action>
    </AlertDialog.Footer>
  </AlertDialog.Content>
</AlertDialog.Root>
```

### 2. Styled Trigger with Tailwind

```svelte
<script lang="ts">
  import * as AlertDialog from "$lib/components/ui/alert-dialog/index.js";
  import { buttonVariants } from "$lib/components/ui/button/index.js";
</script>

<AlertDialog.Root>
  <AlertDialog.Trigger class={buttonVariants({ variant: "outline" })}>
    Show Dialog
  </AlertDialog.Trigger>

  <AlertDialog.Content>
    <AlertDialog.Header>
      <AlertDialog.Title>Are you absolutely sure?</AlertDialog.Title>
      <AlertDialog.Description>
        This action cannot be undone. This will permanently delete your account
        and remove your data from our servers.
      </AlertDialog.Description>
    </AlertDialog.Header>

    <AlertDialog.Footer>
      <AlertDialog.Cancel>Cancel</AlertDialog.Cancel>
      <AlertDialog.Action>Continue</AlertDialog.Action>
    </AlertDialog.Footer>
  </AlertDialog.Content>
</AlertDialog.Root>
```

---

## API Reference

| Element | Description | Props |
|---------|-------------|-------|
| `<AlertDialog.Root>` | Wrapper that manages dialog state. | `open?: boolean`<br>`onOpenChange?: (open: boolean) => void` |
| `<AlertDialog.Trigger>` | Element that opens the dialog. | `as?: string | typeof SvelteComponent`<br>`class?: string` |
| `<AlertDialog.Content>` | The modal container. | `class?: string` |
| `<AlertDialog.Header>` | Header wrapper. | `class?: string` |
| `<AlertDialog.Title>` | Dialog title. | `class?: string` |
| `<AlertDialog.Description>` | Dialog description. | `class?: string` |
| `<AlertDialog.Footer>` | Footer wrapper. | `class?: string` |
| `<AlertDialog.Cancel>` | Cancel button. | `class?: string` |
| `<AlertDialog.Action>` | Primary action button. | `class?: string` |

> All elements accept standard Svelte component props and can be styled with Tailwind or custom CSS.

---

## Component Source

The component source can be found in the generated library:

```
$lib/components/ui/alert-dialog/
```

Feel free to inspect or extend the implementation to fit your project’s needs.

---

**Built by shadcn. Ported to Svelte by Huntabyte & CokaKoala.**