# shadcn‑svelte – Avatar Component

> **shadcn‑svelte** is a collection of UI primitives for Svelte, inspired by shadcn‑ui.  
> This page documents the **Avatar** component, one of the most frequently used primitives.

---

## 1. Overview

The `Avatar` component renders a user avatar with an optional fallback.  
It is built with accessibility and theming in mind, and works seamlessly with Tailwind CSS.

```html
<Avatar.Root>
  <Avatar.Image src="https://github.com/shadcn.png" alt="@shadcn" />
  <Avatar.Fallback>CN</Avatar.Fallback>
</Avatar.Root>
```

---

## 2. Installation

### 2.1. Using the CLI

```bash
pnpm dlx shadcn-svelte@latest add avatar
```

> The CLI will add the component files to your `$lib/components/ui/avatar` directory and update your `registry.json`.

### 2.2. Manual Installation

1. Install the package:

   ```bash
   pnpm add shadcn-svelte
   ```

2. Copy the component files from the repository into your project (e.g., `$lib/components/ui/avatar`).

---

## 3. Usage

Import the component namespace and use the sub‑components as shown:

```svelte
<script lang="ts">
  import * as Avatar from "$lib/components/ui/avatar/index.js";
</script>

<Avatar.Root>
  <Avatar.Image src="https://github.com/shadcn.png" alt="@shadcn" />
  <Avatar.Fallback>CN</Avatar.Fallback>
</Avatar.Root>
```

### 3.1. Rounded Avatar

```svelte
<Avatar.Root class="rounded-lg">
  <Avatar.Image src="https://github.com/evilrabbit.png" alt="@evilrabbit" />
  <Avatar.Fallback>ER</Avatar.Fallback>
</Avatar.Root>
```

### 3.2. Stacked Avatars

```svelte
<div class="*:data-[slot=avatar]:ring-background flex -space-x-2 *:data-[slot=avatar]:ring-2 *:data-[slot=avatar]:grayscale">
  <Avatar.Root>
    <Avatar.Image src="https://github.com/shadcn.png" alt="@shadcn" />
    <Avatar.Fallback>CN</Avatar.Fallback>
  </Avatar.Root>

  <Avatar.Root>
    <Avatar.Image src="https://github.com/leerob.png" alt="@leerob" />
    <Avatar.Fallback>LR</Avatar.Fallback>
  </Avatar.Root>

  <Avatar.Root>
    <Avatar.Image src="https://github.com/evilrabbit.png" alt="@evilrabbit" />
    <Avatar.Fallback>ER</Avatar.Fallback>
  </Avatar.Root>
</div>
```

---

## 4. API Reference

| Sub‑Component | Props | Description |
|---------------|-------|-------------|
| `Avatar.Root` | `class?: string` | Wrapper element. |
| `Avatar.Image` | `src: string`, `alt: string`, `class?: string` | The image element. |
| `Avatar.Fallback` | `class?: string` | Fallback content shown when the image fails to load. |

> All sub‑components forward any additional props to the underlying DOM element.

---

## 5. Theming & Dark Mode

The component respects Tailwind’s dark mode utilities.  
Add `dark:` variants to any class to style the avatar in dark mode.

```html
<Avatar.Root class="rounded-full dark:border-white">
  <Avatar.Image src="..." alt="..." />
  <Avatar.Fallback class="dark:bg-gray-800">AB</Avatar.Fallback>
</Avatar.Root>
```

---

## 6. Related Components

- **Badge** – Small status indicator that can be combined with an avatar.  
- **Button** – Use an avatar inside a button for user actions.  
- **Dropdown Menu** – Avatar can trigger a user menu.

---

## 7. FAQ

**Q:** *Can I use a custom fallback component?*  
**A:** Yes – simply replace `<Avatar.Fallback>` with any Svelte component or markup.

**Q:** *How do I handle image loading errors?*  
**A:** The fallback is automatically displayed when the image fails to load.

---

## 8. Credits

Built by **shadcn**.  
Ported to Svelte by **Huntabyte** & **CokaKoala**.

---