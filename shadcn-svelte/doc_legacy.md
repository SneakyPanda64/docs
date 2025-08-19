# shadcn‑svelte

> **shadcn‑svelte** – A collection of highly‑customizable, Tailwind‑powered UI components for Svelte.  
> Built by **shadcn** and ported to Svelte by **Huntabyte** & **CokaKoala**.

---

## Table of Contents

| Section | Description |
|---------|-------------|
| [Getting Started](#getting-started) | Quick setup for SvelteKit, Vite, Astro, or manual installation. |
| [Installation](#installation) | Install the package and its peer dependencies. |
| [Theming](#theming) | How to enable dark mode and customize colors. |
| [CLI](#cli) | Generate components, themes, and more from the command line. |
| [Components](#components) | Full list of available UI primitives. |
| [Examples](#examples) | Working code snippets for each component. |
| [Migration](#migration) | Upgrade notes from Tailwind v3 → v4 and Svelte 4 → 5. |
| [Changelog](#changelog) | Version history. |
| [FAQ](#faq) | Common questions. |
| [Support & Sponsorship](#support) | How to help the project. |

---

## Getting Started

### 1️⃣ SvelteKit

```bash
npm create svelte@latest my-app
cd my-app
npm i shadcn-svelte
```

Add the Tailwind config (if you’re using Tailwind v4):

```bash
npx tailwindcss init -p
```

```js
// tailwind.config.js
import { shadcn } from 'shadcn-svelte/tailwind';

export default {
  content: ['./src/**/*.{svelte,js,ts}'],
  plugins: [shadcn()],
};
```

### 2️⃣ Vite

```bash
npm init vite@latest my-vite-app -- --template svelte
cd my-vite-app
npm i shadcn-svelte
```

Same Tailwind config as above.

### 3️⃣ Astro

```bash
npm create astro@latest
cd my-astro-app
npm i shadcn-svelte
```

Add the Tailwind config and import the components in your Astro pages.

### 4️⃣ Manual Installation

If you’re not using a bundler that supports ESM, you can import the components directly from the `dist` folder:

```html
<script type="module">
  import { Button } from 'shadcn-svelte/dist/Button.js';
</script>
```

---

## Installation

```bash
npm i shadcn-svelte
```

> **Peer Dependencies**  
> - `svelte` (≥ 4.0)  
> - `tailwindcss` (≥ 3.0)  
> - `postcss` (≥ 8.0)  
> - `autoprefixer` (≥ 10.0)

---

## Theming

### Dark Mode

Add the following to your `tailwind.config.js`:

```js
module.exports = {
  darkMode: 'class', // or 'media'
  // ...
};
```

Toggle dark mode by adding the `dark` class to the `<html>` or `<body>` element.

### Custom Colors

```js
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: {
          DEFAULT: '#1d4ed8',
          foreground: '#ffffff',
        },
      },
    },
  },
};
```

---

## CLI

```bash
npx shadcn-svelte@latest init
```

The CLI can generate component skeletons, update themes, and scaffold new projects.

---

## Components

Below is a quick reference to all available components. Each component is a Svelte file that can be imported directly.

| Component | Description |
|-----------|-------------|
| **Accordion** | Collapsible panels. |
| **Alert Dialog** | Modal alerts with actions. |
| **Alert** | Inline alert messages. |
| **Aspect Ratio** | Maintain aspect ratios. |
| **Avatar** | User avatars. |
| **Badge** | Small status indicators. |
| **Breadcrumb** | Navigation breadcrumbs. |
| **Button** | Primary UI button. |
| **Calendar** | Date picker. |
| **Card** | Content container. |
| **Carousel** | Image or content carousel. |
| **Chart** | Data visualisation. |
| **Checkbox** | Form checkbox. |
| **Collapsible** | Expandable content. |
| **Combobox** | Autocomplete dropdown. |
| **Command** | Command palette. |
| **Context Menu** | Right‑click context menu. |
| **Data Table** | Tabular data. |
| **Date Picker** | Date selection. |
| **Dialog** | Modal dialog. |
| **Drawer** | Side panel. |
| **Dropdown Menu** | Dropdown menu. |
| **Formsnap** | Form snapshot. |
| **Hover Card** | Hover‑over card. |
| **Input OTP** | One‑time‑password input. |
| **Input** | Text input. |
| **Label** | Form label. |
| **Menubar** | Top‑bar menu. |
| **Navigation Menu** | Navigation menu. |
| **Pagination** | Pagination controls. |
| **Popover** | Popover tooltip. |
| **Progress** | Progress bar. |
| **Radio Group** | Radio button group. |
| **Range Calendar** | Date range picker. |
| **Resizable** | Resizable panels. |
| **Scroll Area** | Scrollable container. |
| **Select** | Dropdown select. |
| **Separator** | Divider. |
| **Sheet** | Bottom sheet. |
| **Sidebar** | Sidebar navigation. |
| **Skeleton** | Loading skeleton. |
| **Slider** | Range slider. |
| **Sonner** | Toast notifications. |
| **Switch** | Toggle switch. |
| **Table** | Data table. |
| **Tabs** | Tabbed interface. |
| **Textarea** | Multi‑line input. |
| **Toggle Group** | Group of toggles. |
| **Toggle** | Single toggle. |
| **Tooltip** | Hover tooltip. |
| **Typography** | Text styles. |

---

## Examples

Below are minimal working examples for a few components. Feel free to copy‑paste into your Svelte files.

### Button

```svelte
<script>
  import { Button } from 'shadcn-svelte';
</script>

<Button on:click={() => alert('Clicked!')}>
  Click Me
</Button>
```

### Accordion

```svelte
<script>
  import { Accordion, AccordionItem, AccordionTrigger, AccordionContent } from 'shadcn-svelte';
</script>

<Accordion type="single" collapsible>
  <AccordionItem value="item-1">
    <AccordionTrigger>Section 1</AccordionTrigger>
    <AccordionContent>
      <p>This is the content of section 1.</p>
    </AccordionContent>
  </AccordionItem>

  <AccordionItem value="item-2">
    <AccordionTrigger>Section 2</AccordionTrigger>
    <AccordionContent>
      <p>This is the content of section 2.</p>
    </AccordionContent>
  </AccordionItem>
</Accordion>
```

### Dialog

```svelte
<script>
  import { Dialog, DialogTrigger, DialogContent, DialogHeader, DialogTitle, DialogDescription } from 'shadcn-svelte';
</script>

<Dialog>
  <DialogTrigger as="button" class="btn">
    Open Dialog
  </DialogTrigger>

  <DialogContent>
    <DialogHeader>
      <DialogTitle>Dialog Title</DialogTitle>
      <DialogDescription>
        This is a description inside the dialog.
      </DialogDescription>
    </DialogHeader>
  </DialogContent>
</Dialog>
```

> **Tip** – All components accept Tailwind utility classes for styling.  
> Example: `<Button class="bg-primary text-primary-foreground">`.

---

## Migration

### Tailwind v3 → v4

- `@tailwindcss/forms` is now part of the core.  
- `@tailwindcss/typography` has been renamed to `@tailwindcss/typography`.  
- Update your `tailwind.config.js` accordingly.

### Svelte 4 → 5

- Svelte 5 introduces the `svelte:options` directive for component options.  
- No breaking changes for shadcn‑svelte components; they remain compatible.

---

## Changelog

See the full changelog on GitHub:  
[https://github.com/shadcn-svelte/shadcn-svelte/blob/main/CHANGELOG.md](https://github.com/shadcn-svelte/shadcn-svelte/blob/main/CHANGELOG.md)

---

## FAQ

| Question | Answer |
|----------|--------|
| **How do I add a new component?** | Run `npx shadcn-svelte@latest add <component-name>` or manually create a Svelte file in `src/lib/components`. |
| **Can I use these components in a non‑Svelte project?** | No, they rely on Svelte’s reactivity. |
| **Is there a TypeScript version?** | Yes, all components ship with `.d.ts` files. |
| **How do I contribute?** | Fork the repo, create a PR, and follow the contribution guidelines. |

---

## Support & Sponsorship

> **We’re looking for one partner to be featured here.**  
> Support the project and reach thousands of developers.  
> Reach out: `support@shadcn-svelte.com`

---

### Legacy Docs

If you need older documentation for **Svelte 4 + Tailwind v3** or **Svelte 5 + Tailwind v3**, you can find them in the legacy docs section of the website.

---

**Happy coding!** 🚀