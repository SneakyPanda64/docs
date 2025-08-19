# shadcn‑svelte – Dialog Component

The **Dialog** component is a modal dialog that can be opened from a trigger element and closed by clicking outside, pressing `Esc`, or using the close button.  
It is built on top of the `@radix-ui/react-dialog` primitives and styled with Tailwind CSS.

> **Note** – All examples below use **Svelte 5** with **TypeScript**.

---

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Examples](#examples)
  - [Edit‑Profile Form](#edit-profile-form)
  - [Confirmation Dialog](#confirmation-dialog)
- [API Reference](#api-reference)
  - [`Dialog.Root`](#dialogroot)
  - [`Dialog.Trigger`](#dialogtrigger)
  - [`Dialog.Content`](#dialogcontent)
  - [`Dialog.Header`](#dialogheader)
  - [`Dialog.Title`](#dialogtitle)
  - [`Dialog.Description`](#dialogdescription)
  - [`Dialog.Footer`](#dialogfooter)
- [Related Components](#related-components)

---

## Installation

### CLI

```bash
# Using pnpm
pnpm dlx shadcn-svelte@latest add dialog

# Using npm
npm i shadcn-svelte@latest
npx shadcn-svelte add dialog

# Using bun
bun x shadcn-svelte@latest add dialog

# Using yarn
yarn dlx shadcn-svelte@latest add dialog
```

### Manual

1. Install the package:

   ```bash
   pnpm add shadcn-svelte
   ```

2. Import the component in your Svelte file:

   ```svelte
   <script lang="ts">
     import * as Dialog from "$lib/components/ui/dialog/index.js";
   </script>
   ```

---

## Usage

```svelte
<script lang="ts">
  import * as Dialog from "$lib/components/ui/dialog/index.js";
</script>

<Dialog.Root>
  <Dialog.Trigger>Open</Dialog.Trigger>

  <Dialog.Content>
    <Dialog.Header>
      <Dialog.Title>Are you sure absolutely sure?</Dialog.Title>
      <Dialog.Description>
        This action cannot be undone. This will permanently delete your account
        and remove your data from our servers.
      </Dialog.Description>
    </Dialog.Header>
  </Dialog.Content>
</Dialog.Root>
```

> The `Dialog.Trigger` can be any element (button, link, etc.).  
> The `Dialog.Content` is the modal container; you can style it with Tailwind classes.

---

## Examples

### Edit‑Profile Form

```svelte
<script lang="ts">
  import { Button, buttonVariants } from "$lib/components/ui/button/index.js";
  import * as Dialog from "$lib/components/ui/dialog/index.js";
  import { Input } from "$lib/components/ui/input/index.js";
  import { Label } from "$lib/components/ui/label/index.js";
</script>

<Dialog.Root>
  <Dialog.Trigger class={buttonVariants({ variant: "outline" })}>
    Edit Profile
  </Dialog.Trigger>

  <Dialog.Content class="sm:max-w-[425px]">
    <Dialog.Header>
      <Dialog.Title>Edit profile</Dialog.Title>
      <Dialog.Description>
        Make changes to your profile here. Click save when you're done.
      </Dialog.Description>
    </Dialog.Header>

    <div class="grid gap-4 py-4">
      <div class="grid grid-cols-4 items-center gap-4">
        <Label for="name" class="text-right">Name</Label>
        <Input id="name" value="Pedro Duarte" class="col-span-3" />
      </div>

      <div class="grid grid-cols-4 items-center gap-4">
        <Label for="username" class="text-right">Username</Label>
        <Input id="username" value="@peduarte" class="col-span-3" />
      </div>
    </div>

    <Dialog.Footer>
      <Button type="submit">Save changes</Button>
    </Dialog.Footer>
  </Dialog.Content>
</Dialog.Root>
```

### Confirmation Dialog

```svelte
<script lang="ts">
  import * as Dialog from "$lib/components/ui/dialog/index.js";
</script>

<Dialog.Root>
  <Dialog.Trigger>Delete Account</Dialog.Trigger>

  <Dialog.Content>
    <Dialog.Header>
      <Dialog.Title>Delete account</Dialog.Title>
      <Dialog.Description>
        Are you sure you want to delete your account? This action cannot be undone.
      </Dialog.Description>
    </Dialog.Header>

    <Dialog.Footer>
      <Button variant="destructive">Delete</Button>
      <Dialog.Close>Cancel</Dialog.Close>
    </Dialog.Footer>
  </Dialog.Content>
</Dialog.Root>
```

---

## API Reference

All components are exported from `$lib/components/ui/dialog/index.js`.

| Component | Description | Props |
|-----------|-------------|-------|
| `Dialog.Root` | The root provider that manages dialog state. | `open?: boolean` – controlled open state. <br> `onOpenChange?: (open: boolean) => void` – callback when open state changes. |
| `Dialog.Trigger` | Element that opens the dialog. | `asChild?: boolean` – render as child element. |
| `Dialog.Content` | The modal container. | `class?: string` – Tailwind classes for styling. |
| `Dialog.Header` | Wrapper for title and description. | — |
| `Dialog.Title` | Title of the dialog. | — |
| `Dialog.Description` | Description text. | — |
| `Dialog.Footer` | Footer area (usually for actions). | — |
| `Dialog.Close` | Button that closes the dialog. | `asChild?: boolean` – render as child element. |

> **Tip** – Use `Dialog.Close` inside the footer or header to provide a close button that automatically calls `onOpenChange(false)`.

---

## Related Components

- **Button** – `$lib/components/ui/button/index.js`
- **Input** – `$lib/components/ui/input/index.js`
- **Label** – `$lib/components/ui/label/index.js`
- **Alert** – `$lib/components/ui/alert/index.js`
- **Drawer** – `$lib/components/ui/drawer/index.js`

---

### Quick Start

```svelte
<script lang="ts">
  import * as Dialog from "$lib/components/ui/dialog/index.js";
  import { Button } from "$lib/components/ui/button/index.js";
</script>

<Dialog.Root>
  <Dialog.Trigger asChild>
    <Button>Open Dialog</Button>
  </Dialog.Trigger>

  <Dialog.Content class="p-6 bg-white rounded shadow">
    <Dialog.Header>
      <Dialog.Title>Dialog Title</Dialog.Title>
      <Dialog.Description>Dialog description goes here.</Dialog.Description>
    </Dialog.Header>

    <Dialog.Footer>
      <Button variant="primary">Confirm</Button>
      <Dialog.Close asChild>
        <Button variant="secondary">Cancel</Button>
      </Dialog.Close>
    </Dialog.Footer>
  </Dialog.Content>
</Dialog.Root>
```

Feel free to customize the Tailwind classes to match your design system. Happy coding!