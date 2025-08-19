# shadcn‑Svelte Documentation

> **shadcn‑Svelte** – A collection of UI components, blocks, and utilities built with Svelte, Tailwind CSS, and the shadcn‑UI design system.

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [Installation](#installation)
3. [Components](#components)
4. [Blocks](#blocks)
5. [Charts](#charts)
6. [Themes & Colors](#themes--colors)
7. [FAQ](#faq)
8. [Examples](#examples)

---

## 1. Getting Started

shadcn‑Svelte is a UI component library that follows the same design principles as shadcn‑UI. It is fully compatible with SvelteKit, Vite, and Astro projects.

> **Tip** – All components are built with Tailwind CSS v4 and are fully themable.

---

## 2. Installation

### 2.1 SvelteKit

```bash
npm i shadcn-svelte
```

### 2.2 Vite

```bash
npm i shadcn-svelte
```

### 2.3 Astro

```bash
npm i shadcn-svelte
```

> **Manual Installation** – If you prefer to add the library manually, copy the `components.json` file into your project and import the components you need.

---

## 3. Components

| Component | Description |
|-----------|-------------|
| Accordion | Expandable panels |
| Alert Dialog | Modal alerts |
| Alert | Inline alerts |
| Aspect Ratio | Maintain aspect ratios |
| Avatar | User avatars |
| Badge | Small status indicators |
| Breadcrumb | Navigation breadcrumbs |
| Button | Primary UI button |
| Calendar | Date picker |
| Card | Content card |
| Carousel | Image carousel |
| Chart | Data visualisation |
| Checkbox | Form checkbox |
| Collapsible | Collapsible sections |
| Combobox | Autocomplete dropdown |
| Command | Command palette |
| Context Menu | Right‑click menu |
| Data Table | Tabular data |
| Date Picker | Date selection |
| Dialog | Modal dialog |
| Drawer | Side drawer |
| Dropdown Menu | Dropdown menu |
| Formsnap | Form snapshot |
| Hover Card | Hoverable card |
| Input OTP | One‑time password input |
| Input | Text input |
| Label | Form label |
| Menubar | Menu bar |
| Navigation Menu | Navigation menu |
| Pagination | Pagination controls |
| Popover | Popover tooltip |
| Progress | Progress bar |
| Radio Group | Radio button group |
| Range Calendar | Calendar range picker |
| Resizable | Resizable panels |
| Scroll Area | Custom scroll area |
| Select | Select dropdown |
| Separator | Divider |
| Sheet | Bottom sheet |
| Sidebar | Sidebar navigation |
| Skeleton | Loading skeleton |
| Slider | Slider input |
| Sonner | Toast notifications |
| Switch | Toggle switch |
| Table | Data table |
| Tabs | Tabbed interface |
| Textarea | Text area |
| Toggle Group | Group of toggles |
| Toggle | Toggle button |
| Tooltip | Tooltip |
| Typography | Typography utilities |

---

## 4. Blocks

Blocks are reusable page‑level components that can be dropped into your project. They often include multiple sub‑components, hooks, and utilities.

---

## 5. Charts

Customisable chart components built on top of popular charting libraries.

---

## 6. Themes & Colors

### 6.1 Adding a New Tailwind Color

To add a custom color, extend the `cssVars` section in your registry item:

```json
{
  "$schema": "https://shadcn-svelte.com/schema/registry-item.json",
  "name": "hello-world",
  "title": "Hello World",
  "type": "registry:block",
  "description": "A complex hello world component",
  "files": [
    // ...
  ],
  "cssVars": {
    "light": {
      "brand-background": "20 14.3% 4.1%",
      "brand-accent": "20 14.3% 4.1%"
    },
    "dark": {
      "brand-background": "20 14.3% 4.1%",
      "brand-accent": "20 14.3% 4.1%"
    }
  }
}
```

After running the CLI, the new colors become available as utility classes:

```html
<div class="bg-brand text-brand-accent">Hello World</div>
```

### 6.2 Overriding Tailwind Theme Variables

Add or override theme variables under `cssVars.theme`:

```json
{
  "$schema": "https://shadcn-svelte.com/schema/registry-item.json",
  "name": "hello-world",
  "title": "Hello World",
  "type": "registry:block",
  "description": "A complex hello world component",
  "files": [
    // ...
  ],
  "cssVars": {
    "theme": {
      "text-base": "3rem",
      "ease-in-out": "cubic-bezier(0.4, 0, 0.2, 1)",
      "font-heading": "Poppins, sans-serif"
    }
  }
}
```

---

## 7. FAQ

### Q: What does a complex component look like?

A complex component typically includes a page, multiple sub‑components, hooks, utilities, and a configuration file. Example:

```json
{
  "$schema": "https://shadcn-svelte.com/schema/registry-item.json",
  "name": "hello-world",
  "title": "Hello World",
  "type": "registry:block",
  "description": "A complex hello world component",
  "files": [
    {
      "path": "registry/hello-world/page.svelte",
      "type": "registry:page",
      "target": "src/routes/hello/+page.svelte"
    },
    {
      "path": "registry/hello-world/components/hello-world.svelte",
      "type": "registry:component"
    },
    {
      "path": "registry/hello-world/components/formatted-message.svelte",
      "type": "registry:component"
    },
    {
      "path": "registry/hello-world/hooks/use-hello.svelte.ts",
      "type": "registry:hook"
    },
    {
      "path": "registry/hello-world/lib/format-date.ts",
      "type": "registry:utils"
    },
    {
      "path": "registry/hello-world/hello.config.ts",
      "type": "registry:file",
      "target": "hello.config.ts"
    }
  ]
}
```

### Q: How do I add a new Tailwind color?

See **6.1 Adding a New Tailwind Color** above.

### Q: How do I add or override a Tailwind theme variable?

See **6.2 Overriding Tailwind Theme Variables** above.

---

## 8. Examples

### Example 1 – Complex Component

```json
{
  "$schema": "https://shadcn-svelte.com/schema/registry-item.json",
  "name": "hello-world",
  "title": "Hello World",
  "type": "registry:block",
  "description": "A complex hello world component",
  "files": [
    {
      "path": "registry/hello-world/page.svelte",
      "type": "registry:page",
      "target": "src/routes/hello/+page.svelte"
    },
    {
      "path": "registry/hello-world/components/hello-world.svelte",
      "type": "registry:component"
    },
    {
      "path": "registry/hello-world/components/formatted-message.svelte",
      "type": "registry:component"
    },
    {
      "path": "registry/hello-world/hooks/use-hello.svelte.ts",
      "type": "registry:hook"
    },
    {
      "path": "registry/hello-world/lib/format-date.ts",
      "type": "registry:utils"
    },
    {
      "path": "registry/hello-world/hello.config.ts",
      "type": "registry:file",
      "target": "hello.config.ts"
    }
  ]
}
```

### Example 2 – Adding a Tailwind Color

```json
{
  "$schema": "https://shadcn-svelte.com/schema/registry-item.json",
  "name": "hello-world",
  "title": "Hello World",
  "type": "registry:block",
  "description": "A complex hello world component",
  "files": [
    // ...
  ],
  "cssVars": {
    "light": {
      "brand-background": "20 14.3% 4.1%",
      "brand-accent": "20 14.3% 4.1%"
    },
    "dark": {
      "brand-background": "20 14.3% 4.1%",
      "brand-accent": "20 14.3% 4.1%"
    }
  }
}
```

### Example 3 – Overriding a Theme Variable

```json
{
  "$schema": "https://shadcn-svelte.com/schema/registry-item.json",
  "name": "hello-world",
  "title": "Hello World",
  "type": "registry:block",
  "description": "A complex hello world component",
  "files": [
    // ...
  ],
  "cssVars": {
    "theme": {
      "text-base": "3rem",
      "ease-in-out": "cubic-bezier(0.4, 0, 0.2, 1)",
      "font-heading": "Poppins, sans-serif"
    }
  }
}
```

---

### End of Documentation

Feel free to explore the full component list, read the changelog, or contribute to the registry. Happy coding!