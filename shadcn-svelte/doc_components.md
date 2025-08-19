# shadcn‑svelte – Component Library Documentation

> **shadcn‑svelte** is a collection of fully‑styled, accessible UI components for Svelte, inspired by the shadcn‑ui design system.  
> It ships with Tailwind CSS v4 support, dark‑mode out of the box, and a CLI for generating component skeletons.

---

## Table of Contents

| Section | Description |
|---------|-------------|
| [Getting Started](#getting-started) | Quick installation for SvelteKit, Vite, Astro, or manual setup |
| [Theming](#theming) | Dark‑mode support and custom theme configuration |
| [CLI](#cli) | Generate component files from the command line |
| [Components](#components) | Full list of available components |
| [Registry](#registry) | JSON schema for component registry |
| [FAQ](#faq) | Frequently asked questions |
| [Examples](#examples) | Sample usage snippets |
| [Changelog](#changelog) | Release history |
| [Migration](#migration) | Upgrade guide from older versions |
| [Legacy Docs](#legacy-docs) | Deprecated documentation |
| [Svelte 5](#svelte-5) | Compatibility notes |
| [Tailwind v4](#tailwind-v4) | Tailwind configuration tips |

---

## Getting Started

### 1️⃣ Installation

#### SvelteKit

```bash
# Install the library
npm i shadcn-svelte

# Add Tailwind CSS (if not already present)
npm i -D tailwindcss postcss autoprefixer
npx tailwindcss init -p

# Configure Tailwind (tailwind.config.cjs)
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

#### Vite

```bash
npm i shadcn-svelte
npm i -D tailwindcss postcss autoprefixer
npx tailwindcss init -p

# Add the same `content` array as above
```

#### Astro

```bash
npm i shadcn-svelte
npm i -D tailwindcss postcss autoprefixer
npx tailwindcss init -p

# In `astro.config.mjs` add:
export default defineConfig({
  integrations: [
    // ...
  ],
  vite: {
    // ...
  },
  // Tailwind config goes in `tailwind.config.cjs`
})
```

#### Manual Installation

If you prefer to add the library manually:

1. Copy the `components` folder into your project.
2. Import the components you need:

```svelte
<script>
  import { Button } from './components/Button.svelte';
</script>

<Button>Click me</Button>
```

---

### 2️⃣ Dark Mode

The library automatically respects the `prefers-color-scheme` media query.  
To enable dark mode manually, add the following to your global CSS:

```css
/* tailwind.css */
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer utilities {
  .dark\:bg-gray-800 { background-color: #1f2937; }
  /* ... other dark utilities ... */
}
```

You can also toggle dark mode via a Svelte store:

```svelte
<script>
  import { writable } from 'svelte/store';
  export const darkMode = writable(false);
</script>

<button on:click={() => darkMode.update(v => !v)}>
  Toggle Dark Mode
</button>
```

---

## CLI

Generate a new component skeleton with:

```bash
npx shadcn-svelte generate Button
```

This creates `Button.svelte`, `Button.test.js`, and a Storybook story.

---

## Components

Below is the full list of components available in the library.  
Each component is fully accessible, responsive, and styled with Tailwind CSS.

| Category | Component |
|----------|-----------|
| **Layout** | Accordion, Alert Dialog, Alert, Aspect Ratio, Avatar, Badge, Breadcrumb, Button, Calendar, Card, Carousel, Chart, Checkbox, Collapsible, Combobox, Command, Context Menu, Data Table, Date Picker, Dialog, Drawer, Dropdown Menu, Formsnap, Hover Card, Input OTP, Input, Label, Menubar, Navigation Menu, Pagination, Popover, Progress, Radio Group, Range Calendar, Resizable, Scroll Area, Select, Separator, Sheet, Sidebar, Skeleton, Slider, Sonner, Switch, Table, Tabs, Textarea, Toggle Group, Toggle, Tooltip, Typography |
| **Utilities** | Theme, Colors, Search, Registry |
| **Extras** | Legacy Docs, Migration, Svelte 5, Tailwind v4 |

> **Tip:** Use the component registry (`registry.json`) to discover component metadata and usage examples.

---

## Registry

The registry is a JSON schema that describes each component’s props, slots, and examples.

### `registry.json`

```json
{
  "components": [
    {
      "name": "Button",
      "description": "A clickable button component.",
      "props": [
        { "name": "variant", "type": "string", "default": "primary" },
        { "name": "size", "type": "string", "default": "md" }
      ],
      "slots": ["default"],
      "examples": [
        {
          "title": "Primary Button",
          "code": "<Button variant=\"primary\">Click me</Button>"
        }
      ]
    }
    // ... other components
  ]
}
```

### `registry-item.json`

```json
{
  "name": "Alert",
  "description": "Displays an alert message.",
  "props": [
    { "name": "type", "type": "string", "default": "info" }
  ],
  "slots": ["default"],
  "examples": [
    {
      "title": "Error Alert",
      "code": "<Alert type=\"error\">Something went wrong.</Alert>"
    }
  ]
}
```

---

## FAQ

| Question | Answer |
|----------|--------|
| **How do I customize the theme?** | Edit `tailwind.config.cjs` and add your own colors under `theme.extend.colors`. |
| **Can I use this with Svelte 5?** | Yes, the library is fully compatible with Svelte 5. |
| **Is there a Storybook integration?** | Each component comes with a Storybook story that can be imported into your own Storybook setup. |

---

## Examples

Below are a few quick usage snippets for common components.

### Button

```svelte
<script>
  import { Button } from 'shadcn-svelte';
</script>

<Button variant="primary" size="lg">
  Submit
</Button>
```

### Alert

```svelte
<script>
  import { Alert } from 'shadcn-svelte';
</script>

<Alert type="warning">
  Please check your input.
</Alert>
```

### Dialog

```svelte
<script>
  import { Dialog, DialogTrigger, DialogContent } from 'shadcn-svelte';
</script>

<Dialog>
  <DialogTrigger>Open Dialog</DialogTrigger>
  <DialogContent>
    <h2>Dialog Title</h2>
    <p>This is the dialog content.</p>
  </DialogContent>
</Dialog>
```

---

## Changelog

All releases are documented in the `CHANGELOG.md` file.  
Key highlights:

- **v1.0.0** – Initial release with 50+ components.
- **v1.2.0** – Added dark‑mode support and CLI.
- **v1.3.0** – Migrated to Tailwind v4 and Svelte 5 compatibility.

---

## Migration

If you’re upgrading from an older version, follow the migration guide:

1. Update `shadcn-svelte` to the latest version.
2. Run `npx shadcn-svelte migrate`.
3. Review the generated diff and adjust any custom styles.

---

## Legacy Docs

Older documentation can be found in the `legacy` folder.  
These docs are kept for reference but are no longer actively maintained.

---

## Svelte 5

The library is fully compatible with Svelte 5.  
No additional configuration is required beyond the standard Tailwind setup.

---

## Tailwind v4

Tailwind CSS v4 introduces new utilities and a revamped configuration.  
Make sure to update your `tailwind.config.cjs` accordingly:

```js
module.exports = {
  content: [
    "./src/**/*.{html,js,svelte,ts}",
    "./node_modules/shadcn-svelte/**/*.{html,js,svelte,ts}"
  ],
  theme: {
    extend: {
      // Add custom colors, spacing, etc.
    },
  },
  plugins: [],
}
```

---

### Need Help?

- **Community** – Join the Discord or GitHub Discussions for support.
- **Sponsor** – We’re looking for a partner to feature here. Reach out if you’re interested.

---

*© 2025 shadcn‑svelte. All rights reserved.*