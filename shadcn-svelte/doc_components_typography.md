# shadcn‑Svelte – Component Library Documentation

> **shadcn‑Svelte** is a collection of UI components built with Svelte and Tailwind CSS.  
> It follows the design system of shadcn‑React, but is fully ported to Svelte.

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [Installation](#installation)
3. [Theming & Dark Mode](#theming--dark-mode)
4. [CLI & Scripts](#cli--scripts)
5. [Component Overview](#component-overview)
6. [Examples](#examples)
7. [FAQ & Troubleshooting](#faq--troubleshooting)
8. [Contributing](#contributing)

---

## 1. Getting Started

```bash
# Using Vite
npm create vite@latest my-app -- --template svelte
cd my-app
npm i shadcn-svelte
```

> For SvelteKit, Astro, or a manual setup, see the **Installation** section below.

---

## 2. Installation

### Vite / SvelteKit

```bash
npm i shadcn-svelte
```

Add the Tailwind config (if you don’t already have one):

```bash
npx tailwindcss init -p
```

```js
// tailwind.config.js
module.exports = {
  content: [
    "./src/**/*.{html,js,svelte,ts}",
    "./node_modules/shadcn-svelte/**/*.{html,js,svelte,ts}"
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

### Astro

```bash
npm i shadcn-svelte
```

Add the component import in your Astro page:

```astro
---
import { Button } from 'shadcn-svelte';
---
<Button>Click me</Button>
```

### Manual Installation

If you prefer to copy the component files directly:

1. Copy the `src/components` folder into your project.
2. Import the components manually:

```svelte
<script>
  import { Button } from './components/Button.svelte';
</script>

<Button>Manual Import</Button>
```

---

## 3. Theming & Dark Mode

shadcn‑Svelte uses Tailwind’s `dark:` variant.  
Add the following to your `tailwind.config.js`:

```js
module.exports = {
  darkMode: 'class', // or 'media'
  // ...
}
```

Toggle dark mode by adding the `dark` class to the `<html>` or `<body>` element:

```html
<html class="dark">
  <!-- ... -->
</html>
```

---

## 4. CLI & Scripts

The library ships with a small CLI to generate component skeletons.

```bash
npx shadcn-svelte init
```

This will scaffold a new component in `src/components`.

---

## 5. Component Overview

| Component | Description |
|-----------|-------------|
| **Accordion** | Collapsible panels |
| **Alert Dialog** | Modal alerts |
| **Alert** | Inline alerts |
| **Aspect Ratio** | Maintain aspect ratio |
| **Avatar** | User avatars |
| **Badge** | Small status tags |
| **Breadcrumb** | Navigation breadcrumbs |
| **Button** | Primary UI button |
| **Calendar** | Date picker |
| **Card** | Content container |
| **Carousel** | Image carousel |
| **Chart** | Data visualisation |
| **Checkbox** | Form checkbox |
| **Collapsible** | Expandable content |
| **Combobox** | Autocomplete input |
| **Command** | Command palette |
| **Context Menu** | Right‑click menu |
| **Data Table** | Tabular data |
| **Date Picker** | Date selection |
| **Dialog** | Modal dialog |
| **Drawer** | Side panel |
| **Dropdown Menu** | Dropdown list |
| **Formsnap** | Form snapshot |
| **Hover Card** | Hover preview |
| **Input OTP** | One‑time password input |
| **Input** | Text input |
| **Label** | Form label |
| **Menubar** | Top‑bar menu |
| **Navigation Menu** | Navigation bar |
| **Pagination** | Page navigation |
| **Popover** | Tooltip‑like popover |
| **Progress** | Progress bar |
| **Radio Group** | Radio buttons |
| **Range Calendar** | Date range picker |
| **Resizable** | Resizable panels |
| **Scroll Area** | Custom scrollable area |
| **Select** | Dropdown select |
| **Separator** | Divider |
| **Sheet** | Bottom sheet |
| **Sidebar** | Side navigation |
| **Skeleton** | Loading skeleton |
| **Slider** | Range slider |
| **Sonner** | Toast notifications |
| **Switch** | Toggle switch |
| **Table** | Data table |
| **Tabs** | Tabbed interface |
| **Textarea** | Multi‑line input |
| **Toggle Group** | Grouped toggles |
| **Toggle** | Single toggle |
| **Tooltip** | Hover tooltip |
| **Typography** | Text styles |

---

## 6. Examples

Below is a full page example that demonstrates many of the components and Tailwind utilities used in shadcn‑Svelte.

```svelte
<script>
  // Import any components you need
  import { Button } from 'shadcn-svelte';
</script>

<div>
  <!-- Heading -->
  <h1 class="scroll-m-20 text-balance text-4xl font-extrabold tracking-tight">
    Taxing Laughter: The Joke Tax Chronicles
  </h1>

  <!-- Paragraph -->
  <p class="text-muted-foreground text-xl leading-7 [&:not(:first-child)]:mt-6">
    Once upon a time, in a far-off land, there was a very lazy king who spent all day lounging on his throne. One day, his advisors came to him with a problem: the kingdom was running out of money.
  </p>

  <!-- Sub‑heading -->
  <h2 class="mt-10 scroll-m-20 border-b pb-2 text-3xl font-semibold tracking-tight transition-colors first:mt-0">
    The King&apos;s Plan
  </h2>

  <!-- Link -->
  <p class="leading-7 [&:not(:first-child)]:mt-6">
    The king thought long and hard, and finally came up with
    <a href="##" class="text-primary font-medium underline underline-offset-4">
      a brilliant plan
    </a>
    : he would tax the jokes in the kingdom.
  </p>

  <!-- Blockquote -->
  <blockquote class="mt-6 border-l-2 pl-6 italic">
    &quot;After all,&quot; he said, &quot;everyone enjoys a good joke, so it&apos;s only fair that they should pay for the privilege.&quot;
  </blockquote>

  <!-- List -->
  <ul class="my-6 ml-6 list-disc [&>li]:mt-2">
    <li>1st level of puns: 5 gold coins</li>
    <li>2nd level of jokes: 10 gold coins</li>
    <li>3rd level of one‑liners : 20 gold coins</li>
  </ul>

  <!-- Table -->
  <div class="my-6 w-full overflow-y-auto">
    <table class="w-full">
      <thead>
        <tr class="even:bg-muted m-0 border-t p-0">
          <th class="border px-4 py-2 text-left font-bold [&[align=center]]:text-center [&[align=right]]:text-right">
            King&apos;s Treasury
          </th>
          <th class="border px-4 py-2 text-left font-bold [&[align=center]]:text-center [&[align=right]]:text-right">
            People&apos;s happiness
          </th>
        </tr>
      </thead>
      <tbody>
        <tr class="even:bg-muted m-0 border-t p-0">
          <td class="border px-4 py-2 text-left [&[align=center]]:text-center [&[align=right]]:text-right">
            Empty
          </td>
          <td class="border px-4 py-2 text-left [&[align=center]]:text-center [&[align=right]]:text-right">
            Overflowing
          </td>
        </tr>
        <tr class="even:bg-muted m-0 border-t p-0">
          <td class="border px-4 py-2 text-left [&[align=center]]:text-center [&[align=right]]:text-right">
            Modest
          </td>
          <td class="border px-4 py-2 text-left [&[align=center]]:text-center [&[align=right]]:text-right">
            Satisfied
          </td>
        </tr>
        <tr class="even:bg-muted m-0 border-t p-0">
          <td class="border px-4 py-2 text-left [&[align=center]]:text-center [&[align=right]]:text-right">
            Full
          </td>
          <td class="border px-4 py-2 text-left [&[align=center]]:text-center [&[align=right]]:text-right">
            Ecstatic
          </td>
        </tr>
      </tbody>
    </table>
  </div>

  <!-- Final paragraph -->
  <p class="leading-7 [&:not(:first-child)]:mt-6">
    The king, seeing how much happier his subjects were, realized the error of his ways and repealed the joke tax. Jokester was declared a hero, and the kingdom lived happily ever after.
  </p>
</div>
```

### Component Usage

```svelte
<!-- Button -->
<Button variant="primary" size="lg">
  Click Me
</Button>

<!-- Tooltip -->
<Tooltip content="This is a tooltip">
  <Button>Hover me</Button>
</Tooltip>
```

---

## 7. FAQ & Troubleshooting

| Question | Answer |
|----------|--------|
| **How do I enable dark mode?** | Add `dark` class to `<html>` or `<body>` and set `darkMode: 'class'` in Tailwind config. |
| **Can I use these components in a non‑Svelte project?** | No, they are Svelte components. |
| **Where are the component source files?** | In `node_modules/shadcn-svelte/src/components`. |
| **How do I customize a component?** | Override Tailwind classes or create a wrapper component. |

---

## 8. Contributing

We welcome contributions! Please read the [CONTRIBUTING.md](CONTRIBUTING.md) file for guidelines.

---

**Happy coding!**