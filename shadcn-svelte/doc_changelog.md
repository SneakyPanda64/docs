# shadcn‑svelte – Code Documentation

> **shadcn‑svelte** is a collection of fully‑accessible, composable UI components for Svelte, inspired by the original shadcn/ui.  
> The library ships with Tailwind v4 support, a powerful CLI, and a growing set of components (Accordion, Button, Calendar, etc.).  
> This document is a cleaned‑up, code‑centric reference that keeps every example from the original docs.

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [Installation](#installation)
3. [Theming & Dark Mode](#theming--dark-mode)
4. [CLI & JavaScript API](#cli--javascript-api)
5. [Component Overview](#component-overview)
6. [Custom Registry Support](#custom-registry-support)
7. [Changelog Highlights](#changelog-highlights)
8. [Examples & Code Snippets](#examples--code-snippets)
9. [FAQ & Resources](#faq--resources)

---

## 1. Getting Started

```bash
# Using Vite
npm create vite@latest my-app -- --template svelte
cd my-app
npm i shadcn-svelte
```

```bash
# Using SvelteKit
npm init svelte@next my-app
cd my-app
npm i shadcn-svelte
```

> **Tip** – For Astro projects, see the dedicated Astro guide in the official docs.

---

## 2. Installation

### Manual

```bash
npm i shadcn-svelte
```

### SvelteKit

```bash
npm i shadcn-svelte
```

### Vite

```bash
npm i shadcn-svelte
```

> The library is fully tree‑shakable. Import only the components you need.

---

## 3. Theming & Dark Mode

* All components accept Tailwind utility classes.  
* Dark mode is enabled by adding `dark:` variants to your Tailwind config.  
* The CLI can generate a theme file (`theme.css`) that you can import globally.

```css
/* theme.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

```html
<script>
  import { Button } from 'shadcn-svelte';
</script>

<Button class="bg-primary text-white dark:bg-primary-dark">
  Click me
</Button>
```

---

## 4. CLI & JavaScript API

The shadcn‑svelte CLI helps scaffold components, generate registry entries, and manage themes.

```bash
# Generate a new component
npx shadcn-svelte generate component Button

# Add a custom registry
npx shadcn-svelte registry add my-registry
```

The JavaScript API is minimal – import components directly:

```js
import { Accordion, Button, Card } from 'shadcn-svelte';
```

---

## 5. Component Overview

| Category | Component | Description |
|----------|-----------|-------------|
| **Layout** | `Accordion`, `Card`, `Grid`, `Sidebar` | Structural building blocks |
| **Form** | `Input`, `Select`, `Checkbox`, `RadioGroup`, `Form.Control` | Accessible form primitives |
| **Navigation** | `Breadcrumb`, `NavigationMenu`, `Menubar`, `Sidebar` | Navigation patterns |
| **Feedback** | `Alert`, `Dialog`, `Drawer`, `Popover`, `Tooltip`, `Sonner` | User notifications |
| **Data** | `Table`, `DataTable`, `Pagination`, `ScrollArea` | Data presentation |
| **Media** | `Avatar`, `Badge`, `Carousel`, `Chart`, `Skeleton` | Visual components |
| **Utilities** | `AspectRatio`, `Skeleton`, `Skeleton`, `Skeleton` | Helper utilities |
| **Others** | `Command`, `Combobox`, `ToggleGroup`, `RangeCalendar`, `Calendar`, `DatePicker` | Advanced UI patterns |

> **Note** – The full list is available in the official component docs.

---

## 6. Custom Registry Support

Publish your own components and share them with the community.

```bash
# Create a new registry
npx shadcn-svelte registry init my-registry

# Publish a component
npx shadcn-svelte registry publish Button
```

The registry is a simple JSON file (`registry.json`) that lists component metadata.  
Remote registries can be consumed by the CLI to install components on demand.

---

## 7. Changelog Highlights

### June 2025 – New Calendar Components

* Overhauled `Calendar` & `RangeCalendar` with month/year dropdowns.  
* Added 30+ calendar blocks for rapid prototyping.

### May 2025 – Tailwind v4 Support

* Full migration to Tailwind v4.  
* Updated styles and utilities.  
* CLI remains compatible with Tailwind v3 until you upgrade.

### March 2024 – Introducing Blocks

* Ready‑made, fully responsive components.  
* Currently React‑only; Svelte support is a future goal.

### February 2024 – New Component: `Resizable`

* Built on PaneForge (early‑stage).  
* Raise issues on the PaneForge GitHub if you encounter bugs.

### January 2024 – New Components

* `Carousel`, `Drawer`, `Sonner`, `Pagination`.  
* `Sonner` is a Svelte port of the React Sonner library.

### December 2023 – New Components

* `Calendar`, `RangeCalendar`, `DatePicker`.

### November 2023 – New Component: `ToggleGroup`

* Grouped toggle buttons.

### October 2023 – New Components

* `Command`, `Combobox`.  
* Updated `<Form.Label />` usage.

---

## 8. Examples & Code Snippets

### Icon Import – Deep Imports

```js
// Old (imports entire icon set)
import { Check } from "@lucide/svelte";

// New (deep import, improves dev server performance)
import Check from "@lucide/svelte/icons/check";
```

### `<Form.Label />` Usage

```svelte
<script>
  import { Label } from 'shadcn-svelte';
  import { getFormField } from 'formsnap';
  const ids = getFormField(); // returns a store
</script>

<Label
  for={$ids.input}
  class={cn($errors && "text-destructive", className)}
  {...$$restProps}
>
  <slot />
</Label>
```

### Exporting `Form.Control`

```ts
// src/lib/ui/form/index.ts
import * as FormPrimitive from '@formsnap/svelte';

const Control = FormPrimitive.Control;

export {
  // ...other exports
  Control,
  Control as FormControl,
};
```

### Using a Component (e.g., `Button`)

```svelte
<script>
  import { Button } from 'shadcn-svelte';
</script>

<Button class="bg-primary text-white dark:bg-primary-dark">
  Click me
</Button>
```

### Using the CLI to Generate a Component

```bash
npx shadcn-svelte generate component MyButton
```

The CLI will scaffold `MyButton.svelte`, `MyButton.test.ts`, and update the registry.

---

## 9. FAQ & Resources

| Question | Answer |
|----------|--------|
| **How do I enable dark mode?** | Add `dark:` variants to your Tailwind classes or use the `dark` class on `<html>`. |
| **Can I use shadcn‑svelte with Astro?** | Yes – see the Astro guide in the official docs. |
| **Where can I find the component API?** | Each component has its own page in the docs; the source code is in `src/lib/ui`. |
| **How do I contribute?** | Fork the repo, create a PR, and follow the contribution guidelines. |
| **Is there a migration guide for Tailwind v4?** | Yes – see the Tailwind v4 migration guide in the docs. |

> **Resources**  
> * Official Docs: https://shadcn-svelte.com  
> * CLI Docs: https://shadcn-svelte.com/cli  
> * Registry Docs: https://shadcn-svelte.com/registry  
> * Figma Library: https://resources.algolia.com

---

**Happy coding!**