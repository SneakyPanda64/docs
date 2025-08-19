# shadcn‑svelte Documentation (Tailwind v4 / Svelte 5)

> This guide covers the core concepts, installation, upgrade path, and key examples for using **shadcn‑svelte** with Tailwind v4 and Svelte 5.  
> All code snippets are kept verbatim from the original docs.

---

## Table of Contents

1. [What’s New](#whats-new)
2. [Getting Started](#getting-started)
   - [Installation](#installation)
   - [CLI](#cli)
3. [Upgrade Your Project](#upgrade-your-project)
   - [1. Tailwind v4 Upgrade Guide](#1-tailwind-v4-upgrade-guide)
   - [2. Replace PostCSS with Vite](#2-replace-postcss-with-vite)
   - [3. Update `app.css`](#3-update-appcss)
   - [4. Remove `tailwind.config.ts`](#4-remove-tailwind-configts)
   - [5. Use `size-*` utilities](#5-use-size-utilities)
   - [6. Update Dependencies](#6-update-dependencies)
   - [7. Update Utils (optional)](#7-update-utils-optional)
   - [8. Update Dark‑Mode Colors (optional)](#8-update-dark-mode-colors-optional)
4. [Key Utilities](#key-utilities)
5. [Component Registry](#component-registry)
6. [FAQ & Troubleshooting](#faq--troubleshooting)

---

## What’s New

| Feature | Description |
|---------|-------------|
| **CLI vlatest** | Initializes projects with Tailwind v4 & Svelte 5. |
| **@theme directive** | Full support for `@theme` and `@theme inline`. |
| **All components updated** | Tailwind v4 & Svelte 5 ready. |
| **`data-slot` attribute** | Every primitive that renders an element now has `data-slot` for styling. |
| **Button cursor** | Buttons now use the default cursor. |
| **New default theme** | New projects use the `new-york` theme. |
| **HSL → OKLCH** | HSL colors are automatically converted to OKLCH. |

> Existing Tailwind v3 projects continue to work; new components are still in Tailwind v3 until you upgrade.

---

## Getting Started

### Installation

```bash
# SvelteKit
pnpm add shadcn-svelte@latest

# Astro
pnpm add shadcn-svelte@latest

# Vite (manual)
pnpm add shadcn-svelte@latest
```

### CLI

```bash
# Initialize a new project
pnpm dlx shadcn-svelte@latest init

# Add a component
pnpm dlx shadcn-svelte@latest add button

# Add all components
pnpm dlx shadcn-svelte@latest add --all
```

---

## Upgrade Your Project

> **Prerequisite**: You’re migrating from a Svelte 5 + Tailwind v3 project.  
> If you’re coming from Svelte 4, first follow the Svelte 4 → Svelte 5 guide.

### 1. Tailwind v4 Upgrade Guide

Follow the official Tailwind upgrade guide:  
<https://tailwindcss.com/docs/upgrade-guide>

Use the `@tailwindcss/upgrade` codemod to remove deprecated utilities and update your config.

### 2. Replace PostCSS with Vite

1. **Delete** `postcss.config.js`

   ```diff
   - export default {
   -   plugins: {
   -     '@tailwindcss/postcss': {},
   -   }
   - };
   ```

2. **Uninstall** PostCSS plugin

   ```bash
   pnpm remove @tailwindcss/postcss
   ```

3. **Install** Vite plugin

   ```bash
   pnpm i @tailwindcss/vite -D
   ```

4. **Update** `vite.config.ts`

   ```ts
   import { sveltekit } from '@sveltejs/kit/vite';
   import { defineConfig } from 'vite';
   import tailwindcss from '@tailwindcss/vite';

   export default defineConfig({
     plugins: [tailwindcss(), sveltekit()],
   });
   ```

5. **Verify** the upgrade

   ```bash
   pnpm run dev
   ```

### 3. Update `app.css`

The codemod will generate a file similar to:

```css
@import "tailwindcss";

@config '../tailwind.config.ts';

/* Compatibility styles for Tailwind v3 border defaults */
@layer base {
  *,
  ::after,
  ::before,
  ::backdrop,
  ::file-selector-button {
    border-color: var(--color-gray-200, currentcolor);
  }
}

@layer base {
  :root {
    --background: 0 0% 100%;
    --foreground: 240 10% 3.9%;
    /* ... other vars ... */
  }

  .dark {
    --background: 240 10% 3.9%;
    --foreground: 0 0% 98%;
    /* ... other vars ... */
  }
}

@layer base {
  * { @apply border-border; }
  body { @apply bg-background text-foreground; }
}
```

#### Replace `tailwind-css-animate` with `tw-animate-css`

```bash
pnpm remove tailwindcss-animate
pnpm i tw-animate-css -D
```

Add the import and custom variant:

```css
@import "tailwindcss";
@import "tw-animate-css";

@custom-variant dark (&:is(.dark *));
```

#### Remove Compatibility Styles

Delete the block that sets `border-color` to `currentcolor`.

#### CSS Variables & Theme Config

Move all color variables to `:root` and `.dark`, wrap values in `hsl()`, and add an `@theme inline` block:

```css
@import "tailwindcss";
@import "tw-animate-css";

@custom-variant dark (&:is(.dark *));

/* Root variables */
:root {
  --background: hsl(0 0% 100%);
  --foreground: hsl(240 10% 3.9%);
  /* ... other vars ... */
  --radius: 0.5rem;
}

/* Dark mode variables */
.dark {
  --background: hsl(240 10% 3.9%);
  --foreground: hsl(0 0% 98%);
  /* ... other vars ... */
}

/* Theme inline */
@theme inline {
  /* Radius */
  --radius-sm: calc(var(--radius) - 4px);
  --radius-md: calc(var(--radius) - 2px);
  --radius-lg: var(--radius);
  --radius-xl: calc(var(--radius) + 4px);

  /* Colors */
  --color-background: var(--background);
  --color-foreground: var(--foreground);
  /* ... other colors ... */
}

/* Base layer */
@layer base {
  * { @apply border-border; }
  body { @apply bg-background text-foreground; }
}
```

### 4. Remove `tailwind.config.ts`

After verifying styles, delete the file:

```bash
rm tailwind.config.ts
```

### 5. Use `size-*` Utilities

Replace `w-* h-*` with the new `size-*` utility:

```diff
- w-4 h-4
+ size-4
```

### 6. Update Dependencies

```bash
pnpm i bits-ui@latest @lucide/svelte@latest tailwind-variants@latest tailwind-merge@latest clsx@latest svelte-sonner@latest paneforge@next vaul-svelte@next formsnap@latest
```

### 7. Update Utils (Optional)

Create or update `src/lib/utils.ts`:

```ts
import { clsx, type ClassValue } from "clsx";
import { twMerge } from "tailwind-merge";

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}

/* Type helpers (previously from bits-ui) */
export type WithoutChild<T> = T extends { child?: any } ? Omit<T, "child"> : T;
export type WithoutChildren<T> = T extends { children?: any } ? Omit<T, "children"> : T;
export type WithoutChildrenOrChild<T> = WithoutChildren<WithoutChild<T>>;
export type WithElementRef<T, U extends HTMLElement = HTMLElement> = T & { ref?: U | null };
```

Replace imports in components:

```svelte
<script lang="ts">
  import type { WithElementRef } from "$lib/utils.js";
</script>
```

### 8. Update Dark‑Mode Colors (Optional)

If you want the new OKLCH dark‑mode colors, re‑add components via the CLI:

```bash
git add .
git commit -m "Prepare for CLI overwrite"
pnpm dlx shadcn-svelte@latest add --all --overwrite
```

Then adjust `app.css` to match the new color values (see the Base Colors reference).

---

## Key Utilities

| Utility | Description |
|---------|-------------|
| `cn()` | Combines Tailwind classes with `clsx` and `tailwind-merge`. |
| `size-*` | Unified width/height utility (e.g., `size-4`). |
| `@theme inline` | Injects theme variables into CSS. |
| `data-slot` | Attribute added to primitives for slot‑based styling. |

---

## Component Registry

The registry JSON files (`registry.json`, `registry-item.json`) describe all available components.  
Use the CLI to generate or update components:

```bash
pnpm dlx shadcn-svelte@latest add <component-name>
```

---

## FAQ & Troubleshooting

| Question | Answer |
|----------|--------|
| **Why does my project still use Tailwind v3 utilities?** | New components are still in Tailwind v3 until you upgrade. Existing projects keep v3 until you run the upgrade steps. |
| **How do I switch themes?** | Use the `@theme` directive or `@theme inline` block in your CSS. |
| **Can I keep PostCSS?** | It’s recommended to switch to Vite for Tailwind v4, but you can keep PostCSS if you prefer. |
| **Where are the color variables?** | In `app.css` under `:root` and `.dark`. |

---

### End of Documentation

Feel free to explore the full component list and examples on the official site: <https://v4.shadcn-svelte.com>.

---