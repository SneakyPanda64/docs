# shadcn‑svelte

> A Svelte port of the popular **shadcn/ui** component library.  
> Built by shadcn, ported to Svelte by Huntabyte & CokaKoala.

---

## Table of Contents

- [Introduction](#introduction)
- [Installation](#installation)
  - [SvelteKit](#sveltekit)
  - [Vite](#vite)
  - [Astro](#astro)
  - [Manual](#manual)
- [Importing Components](#importing-components)
  - [Tree‑shaking](#tree-shaking)
  - [Example: Accordion](#example-accordion)
- [CLI](#cli)
- [VSCode Extension](#vscode-extension)
- [JetBrains IDEs Extension](#jetbrains-ides-extension)
- [Component Registry](#component-registry)
- [Theming & Dark Mode](#theming--dark-mode)
- [FAQ](#faq)

---

## Introduction

shadcn‑svelte brings the same design system and component set that you know from shadcn/ui for React, but fully adapted to Svelte.  
Unlike the original React implementation, each component is split into multiple files because Svelte does not support multiple components in a single file. The CLI automatically generates the folder structure and an `index.ts` that re‑exports the component(s).

---

## Installation

### SvelteKit

```bash
npm i shadcn-svelte
```

Add the components to your project:

```bash
npx shadcn-svelte add accordion
```

### Vite

```bash
npm i shadcn-svelte
```

Same CLI usage as above.

### Astro

```bash
npm i shadcn-svelte
```

Use the same CLI commands.

### Manual

If you prefer to add components manually, copy the component folder from `node_modules/shadcn-svelte/components/ui` into your `src/lib/components/ui` directory.

---

## Importing Components

Each component folder contains an `index.ts` that re‑exports all sub‑components. Import them in two ways:

```ts
// Import the whole namespace
import * as Accordion from '$lib/components/ui/accordion';

// Import individual parts
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger
} from '$lib/components/ui/accordion';
```

### Tree‑shaking

Rollup (or Vite) will automatically tree‑shake unused components, so you only bundle what you actually import.

### Example: Accordion

```svelte
<script lang="ts">
  import * as Accordion from '$lib/components/ui/accordion';
</script>

<Accordion.Root type="single" collapsible>
  <Accordion.Item value="item-1">
    <Accordion.Trigger>Item 1</Accordion.Trigger>
    <Accordion.Content>
      <p>This is the content for item 1.</p>
    </Accordion.Content>
  </Accordion.Item>
  <Accordion.Item value="item-2">
    <Accordion.Trigger>Item 2</Accordion.Trigger>
    <Accordion.Content>
      <p>This is the content for item 2.</p>
    </Accordion.Content>
  </Accordion.Item>
</Accordion.Root>
```

---

## CLI

The shadcn‑svelte CLI automates component installation and updates.

```bash
# Initialize the CLI in your project
npx shadcn-svelte init

# Add a component
npx shadcn-svelte add button

# Remove a component
npx shadcn-svelte remove button
```

The CLI creates a folder for each component (e.g., `accordion/`) containing:

- `accordion.svelte`
- `accordion-content.svelte`
- `accordion-item.svelte`
- `accordion-trigger.svelte`
- `index.ts` (re‑exports)

---

## VSCode Extension

Install the **shadcn‑svelte** extension by @selemondev.

Features:

- Initialize the CLI from the command palette.
- Add components with a single click.
- Navigate directly to a component’s documentation page.
- Snippets for quick imports and markup.

---

## JetBrains IDEs Extension

Install the **shadcn/ui Components Manager** extension by @WarningImHack3r.

Features:

- Auto‑detect shadcn components in your project.
- Add, remove, and update components with a single click.
- Supports Svelte, React, Vue, and Solid implementations.
- Search for remote or existing components.

---

## Component Registry

The library ships with a registry of all available components.  
You can view the full list in `registry.json` or `registry-item.json`.

| Category | Component |
|----------|-----------|
| **UI** | Accordion, Alert Dialog, Alert, Aspect Ratio, Avatar, Badge, Breadcrumb, Button, Calendar, Card, Carousel, Chart, Checkbox, Collapsible, Combobox, Command, Context Menu, Data Table, Date Picker, Dialog, Drawer, Dropdown Menu, Formsnap, Hover Card, Input OTP, Input, Label, Menubar, Navigation Menu, Pagination, Popover, Progress, Radio Group, Range Calendar, Resizable, Scroll Area, Select, Separator, Sheet, Sidebar, Skeleton, Slider, Sonner, Switch, Table, Tabs, Textarea, Toggle Group, Toggle, Tooltip, Typography |

---

## Theming & Dark Mode

The library supports Tailwind v4 and SvelteKit’s built‑in dark mode.  
Add the following to your `tailwind.config.cjs`:

```js
module.exports = {
  darkMode: 'class',
  // ...
};
```

Then toggle dark mode by adding the `dark` class to the root element.

---

## FAQ

**Q: Why are components split into multiple files?**  
A: Svelte does not allow multiple components in a single file, so each logical component is split into its own `.svelte` file and re‑exported via `index.ts`.

**Q: Will unused components be bundled?**  
A: No. Rollup/Vite will tree‑shake any component you do not import.

**Q: How do I add a new component?**  
A: Use the CLI: `npx shadcn-svelte add <component-name>`.

---

*For more detailed usage, refer to the official documentation or the component‑specific pages in the registry.*