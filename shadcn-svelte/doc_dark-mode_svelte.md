# shadcn‑svelte

> **shadcn‑svelte** – A collection of fully‑styled, accessible UI components for Svelte, inspired by shadcn‑ui.  
> Built with Tailwind CSS and fully type‑safe.

---

## Table of Contents

- [Introduction](#introduction)
- [Installation](#installation)
  - [SvelteKit](#sveltekit)
  - [Vite](#vite)
  - [Astro](#astro)
  - [Manual Installation](#manual-installation)
- [Dark Mode](#dark-mode)
  - [Install `mode-watcher`](#install-mode-watcher)
  - [Add the `ModeWatcher` component](#add-the-modewatcher-component)
  - [Add a mode toggle](#add-a-mode-toggle)
- [Components](#components)
- [Theming](#theming)
- [CLI](#cli)
- [JavaScript](#javascript)
- [Figma](#figma)
- [Changelog](#changelog)
- [Legacy Docs](#legacy-docs)
- [Migration](#migration)
- [Svelte 5](#svelte-5)
- [Tailwind v4](#tailwind-v4)
- [FAQ](#faq)
- [Examples](#examples)
- [Registry](#registry)
- [Getting Started](#getting-started)
- [Support the Project](#support-the-project)

---

## Introduction

shadcn‑svelte brings the same design system and component philosophy that powers shadcn‑ui to the Svelte ecosystem. All components are:

- **Accessible** – built with ARIA and keyboard‑friendly interactions.
- **Composable** – tiny, focused pieces that can be combined into complex UIs.
- **Tailwind‑powered** – fully customizable via Tailwind’s utility classes.
- **Type‑safe** – written in TypeScript with full type inference.

---

## Installation

### SvelteKit

```bash
pnpm add shadcn-svelte
```

> **Tip** – If you’re using Vite or Astro, the same command works.

### Vite

```bash
pnpm add shadcn-svelte
```

### Astro

```bash
pnpm add shadcn-svelte
```

### Manual Installation

If you prefer to add the components manually, copy the `components.json` file from the repository into your project and import the desired components directly.

---

## Dark Mode

shadcn‑svelte ships with a lightweight dark‑mode helper called **`mode-watcher`**. It automatically syncs the UI with the user’s system preference and allows you to toggle between light and dark themes.

### Install `mode-watcher`

```bash
pnpm i mode-watcher
```

> You can also use `npm`, `yarn`, or `bun` – the command is the same.

### Add the `ModeWatcher` component

Import the component in your root layout (e.g., `src/routes/+layout.svelte`) and render it once:

```svelte
<script lang="ts">
  import "../app.css";
  import { ModeWatcher } from "mode-watcher";
  let { children } = $props();
</script>

<ModeWatcher />
{@render children?.()}
```

> The `ModeWatcher` component listens for changes to the `prefers-color-scheme` media query and updates the `class` on `<html>` accordingly.

### Add a mode toggle

Place a toggle button anywhere in your app to switch between light and dark mode. A minimal example:

```svelte
<script lang="ts">
  import { useMode } from "mode-watcher";
  const { mode, toggle } = useMode();
</script>

<button on:click={toggle} aria-label="Toggle theme">
  {#if mode === "dark"}🌙{:else}☀️{/if}
</button>
```

> The `useMode` hook returns the current mode (`"light"` or `"dark"`) and a `toggle` function that flips the theme.

---

## Components

Below is a list of all available components. Each component is fully documented in its own file.

- **Accordion**
- **Alert Dialog**
- **Alert**
- **Aspect Ratio**
- **Avatar**
- **Badge**
- **Breadcrumb**
- **Button**
- **Calendar**
- **Card**
- **Carousel**
- **Chart**
- **Checkbox**
- **Collapsible**
- **Combobox**
- **Command**
- **Context Menu**
- **Data Table**
- **Date Picker**
- **Dialog**
- **Drawer**
- **Dropdown Menu**
- **Formsnap**
- **Hover Card**
- **Input OTP**
- **Input**
- **Label**
- **Menubar**
- **Navigation Menu**
- **Pagination**
- **Popover**
- **Progress**
- **Radio Group**
- **Range Calendar**
- **Resizable**
- **Scroll Area**
- **Select**
- **Separator**
- **Sheet**
- **Sidebar**
- **Skeleton**
- **Slider**
- **Sonner**
- **Switch**
- **Table**
- **Tabs**
- **Textarea**
- **Toggle Group**
- **Toggle**
- **Tooltip**
- **Typography**

---

## Theming

shadcn‑svelte supports custom themes via Tailwind’s `theme` configuration. Add your own color palette, spacing, or typography overrides in `tailwind.config.js`:

```js
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: {
          DEFAULT: "#1d4ed8",
          foreground: "#ffffff",
        },
      },
    },
  },
};
```

---

## CLI

The library ships with a small CLI to scaffold new components or update the registry. Run:

```bash
npx shadcn-svelte-cli
```

Follow the prompts to generate a new component or update existing ones.

---

## JavaScript

All components expose a small JavaScript API for advanced usage. For example, the `Dialog` component can be controlled programmatically:

```js
import { openDialog, closeDialog } from "shadcn-svelte";

openDialog({ title: "Hello", content: "World" });
```

---

## Figma

Designers can use the official Figma plugin to generate component code directly from Figma files. Install the plugin from the Figma community and follow the on‑screen instructions.

---

## Changelog

All releases are documented in the `CHANGELOG.md` file. Each entry includes:

- Added
- Changed
- Fixed
- Deprecated

---

## Legacy Docs

Older documentation (pre‑v1.0) is archived in the `legacy` folder. It contains migration guides and backward‑compatibility notes.

---

## Migration

If you’re upgrading from an older version of shadcn‑svelte, consult the migration guide for breaking changes and recommended upgrade paths.

---

## Svelte 5

shadcn‑svelte is fully compatible with Svelte 5. No additional configuration is required.

---

## Tailwind v4

The library targets Tailwind CSS v4. Ensure your project uses the latest Tailwind version for full compatibility.

---

## FAQ

- **Q:** *How do I customize the button styles?*  
  **A:** Override the Tailwind classes in your global CSS or use the `class` prop.

- **Q:** *Can I use these components in a non‑Svelte project?*  
  **A:** No – they rely on Svelte’s reactivity and component model.

---

## Examples

The `examples/` directory contains a collection of working demos for each component. Run them locally with:

```bash
pnpm dev
```

---

## Registry

The `registry.json` file lists all components and their metadata. It is used by the CLI to generate component skeletons.

---

## Getting Started

1. Install the library (`pnpm add shadcn-svelte`).
2. Add the `ModeWatcher` component to your root layout.
3. Import and use any component you need.

---

## Support the Project

shadcn‑svelte is open‑source and maintained by the community. If you find it useful, consider:

- ⭐️ Star the repository
- 💬 Join the Discord community
- 💰 Sponsor the project on GitHub

---

*© 2025 shadcn‑svelte. All rights reserved.*