# shadcn‑svelte – Manual Installation Guide

This guide walks you through setting up **shadcn‑svelte** in a fresh project.  
All commands are shown for the four most common package managers (`pnpm`, `npm`, `bun`, `yarn`).  
Feel free to copy‑paste the snippets directly into your terminal or editor.

> **Tip** – If you’re using a framework that already ships with Tailwind (e.g. SvelteKit, Astro, Vite), you can skip the “Add Tailwind” step and jump straight to the dependency installation.

---

## 1. Add Tailwind CSS

Use the `sv` CLI to scaffold Tailwind into your project.

```bash
# pnpm
pnpm dlx sv add tailwindcss

# npm
npm dlx sv add tailwindcss

# bun
bun dlx sv add tailwindcss

# yarn
yarn dlx sv add tailwindcss
```

---

## 2. Install shadcn‑svelte Dependencies

```bash
# pnpm
pnpm i tailwind-variants clsx tailwind-merge tw-animate-css

# npm
npm i tailwind-variants clsx tailwind-merge tw-animate-css

# bun
bun i tailwind-variants clsx tailwind-merge tw-animate-css

# yarn
yarn add tailwind-variants clsx tailwind-merge tw-animate-css
```

---

## 3. Add an Icon Library

shadcn‑svelte ships with **Lucide** icons by default. Install the Svelte wrapper:

```bash
# pnpm
pnpm i @lucide/svelte

# npm
npm i @lucide/svelte

# bun
bun i @lucide/svelte

# yarn
yarn add @lucide/svelte
```

---

## 4. Configure Path Aliases

### SvelteKit

If you’re not using the default `$lib` alias, update `svelte.config.js`:

```js
// svelte.config.js
const config = {
  // …other config
  kit: {
    // …other kit config
    alias: {
      "@/*": "./path/to/lib/*",
    },
  },
};
```

### Non‑SvelteKit (Vite, Astro, etc.)

Add the alias to both `tsconfig.json` **and** `vite.config.ts`.

```json
// tsconfig.json
{
  "compilerOptions": {
    // …other options
    "paths": {
      "$lib": ["./src/lib"],
      "$lib/*": ["./src/lib/*"]
    }
  }
}
```

```ts
// vite.config.ts
import path from "path";
import { defineConfig } from "vite";

export default defineConfig({
  // …other config
  resolve: {
    alias: {
      $lib: path.resolve("./src/lib"),
    },
  },
});
```

---

## 5. Configure Global Styles

Create or edit `src/app.css` and paste the following.  
It imports Tailwind, the animation plugin, and defines a full set of CSS variables for theming.

```css
/* src/app.css */
@import "tailwindcss";
@import "tw-animate-css";
@custom-variant dark (&:is(.dark *));

/* Light theme variables */
:root {
  --radius: 0.625rem;
  --background: oklch(1 0 0);
  --foreground: oklch(0.145 0 0);
  --card: oklch(1 0 0);
  --card-foreground: oklch(0.145 0 0);
  --popover: oklch(1 0 0);
  --popover-foreground: oklch(0.145 0 0);
  --primary: oklch(0.205 0 0);
  --primary-foreground: oklch(0.985 0 0);
  --secondary: oklch(0.97 0 0);
  --secondary-foreground: oklch(0.205 0 0);
  --muted: oklch(0.97 0 0);
  --muted-foreground: oklch(0.556 0 0);
  --accent: oklch(0.97 0 0);
  --accent-foreground: oklch(0.205 0 0);
  --destructive: oklch(0.577 0.245 27.325);
  --border: oklch(0.922 0 0);
  --input: oklch(0.922 0 0);
  --ring: oklch(0.708 0 0);
  --chart-1: oklch(0.646 0.222 41.116);
  --chart-2: oklch(0.6 0.118 184.704);
  --chart-3: oklch(0.398 0.07 227.392);
  --chart-4: oklch(0.828 0.189 84.429);
  --chart-5: oklch(0.769 0.188 70.08);
  --sidebar: oklch(0.985 0 0);
  --sidebar-foreground: oklch(0.145 0 0);
  --sidebar-primary: oklch(0.205 0 0);
  --sidebar-primary-foreground: oklch(0.985 0 0);
  --sidebar-accent: oklch(0.97 0 0);
  --sidebar-accent-foreground: oklch(0.205 0 0);
  --sidebar-border: oklch(0.922 0 0);
  --sidebar-ring: oklch(0.708 0 0);
}

/* Dark theme overrides */
.dark {
  --background: oklch(0.145 0 0);
  --foreground: oklch(0.985 0 0);
  --card: oklch(0.205 0 0);
  --card-foreground: oklch(0.985 0 0);
  --popover: oklch(0.269 0 0);
  --popover-foreground: oklch(0.985 0 0);
  --primary: oklch(0.922 0 0);
  --primary-foreground: oklch(0.205 0 0);
  --secondary: oklch(0.269 0 0);
  --secondary-foreground: oklch(0.985 0 0);
  --muted: oklch(0.269 0 0);
  --muted-foreground: oklch(0.708 0 0);
  --accent: oklch(0.371 0 0);
  --accent-foreground: oklch(0.985 0 0);
  --destructive: oklch(0.704 0.191 22.216);
  --border: oklch(1 0 0 / 10%);
  --input: oklch(1 0 0 / 15%);
  --ring: oklch(0.556 0 0);
  --chart-1: oklch(0.488 0.243 264.376);
  --chart-2: oklch(0.696 0.17 162.48);
  --chart-3: oklch(0.769 0.188 70.08);
  --chart-4: oklch(0.627 0.265 303.9);
  --chart-5: oklch(0.645 0.246 16.439);
  --sidebar: oklch(0.205 0 0);
  --sidebar-foreground: oklch(0.985 0 0);
  --sidebar-primary: oklch(0.488 0.243 264.376);
  --sidebar-primary-foreground: oklch(0.985 0 0);
  --sidebar-accent: oklch(0.269 0 0);
  --sidebar-accent-foreground: oklch(0.985 0 0);
  --sidebar-border: oklch(1 0 0 / 10%);
  --sidebar-ring: oklch(0.439 0 0);
}

/* Theme helper variables */
@theme inline {
  --radius-sm: calc(var(--radius) - 4px);
  --radius-md: calc(var(--radius) - 2px);
  --radius-lg: var(--radius);
  --radius-xl: calc(var(--radius) + 4px);
  --color-background: var(--background);
  --color-foreground: var(--foreground);
  --color-card: var(--card);
  --color-card-foreground: var(--card-foreground);
  --color-popover: var(--popover);
  --color-popover-foreground: var(--popover-foreground);
  --color-primary: var(--primary);
  --color-primary-foreground: var(--primary-foreground);
  --color-secondary: var(--secondary);
  --color-secondary-foreground: var(--secondary-foreground);
  --color-muted: var(--muted);
  --color-muted-foreground: var(--muted-foreground);
  --color-accent: var(--accent);
  --color-accent-foreground: var(--accent-foreground);
  --color-destructive: var(--destructive);
  --color-border: var(--border);
  --color-input: var(--input);
  --color-ring: var(--ring);
  --color-chart-1: var(--chart-1);
  --color-chart-2: var(--chart-2);
  --color-chart-3: var(--chart-3);
  --color-chart-4: var(--chart-4);
  --color-chart-5: var(--chart-5);
  --color-sidebar: var(--sidebar);
  --color-sidebar-foreground: var(--sidebar-foreground);
  --color-sidebar-primary: var(--sidebar-primary);
  --color-sidebar-primary-foreground: var(--sidebar-primary-foreground);
  --color-sidebar-accent: var(--sidebar-accent);
  --color-sidebar-accent-foreground: var(--sidebar-accent-foreground);
  --color-sidebar-border: var(--sidebar-border);
  --color-sidebar-ring: var(--sidebar-ring);
}

/* Base layer – global resets */
@layer base {
  * {
    @apply border-border outline-ring/50;
  }

  body {
    @apply bg-background text-foreground;
  }
}
```

---

## 6. Create a Utility Helper

Add a small helper to merge Tailwind classes safely.

```ts
// src/lib/utils.ts
import { type ClassValue, clsx } from "clsx";
import { twMerge } from "tailwind-merge";

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```

---

## 7. Import Styles in Your App

Create a root layout that imports the global CSS.

```svelte
<!-- src/routes/+layout.svelte -->
<script lang="ts">
  import "../app.css";
  let { children } = $props();
</script>

{@render children?.()}
```

---

## 8. Add Components

Now you’re ready to add shadcn‑svelte components.  
For example, to add a button component:

```bash
# pnpm
pnpm dlx shadcn-svelte@latest add button

# npm
npm dlx shadcn-svelte@latest add button

# bun
bun dlx shadcn-svelte@latest add button

# yarn
yarn dlx shadcn-svelte@latest add button
```

Repeat the command for any other component you need.

---

## 9. Done!

You can now start building your UI with shadcn‑svelte components.  
Happy coding!