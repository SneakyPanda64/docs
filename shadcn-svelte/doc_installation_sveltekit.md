# shadcn‑svelte

> **shadcn‑svelte** is a collection of reusable, accessible UI components for SvelteKit, built on top of Tailwind CSS.  
> It follows the same design philosophy as the original shadcn‑ui (React) library, but is fully ported to Svelte.

---

## Table of Contents

- [Getting Started](#getting-started)
  - [Create a Project](#create-a-project)
  - [Add Tailwind CSS](#add-tailwind-css)
  - [Configure Path Aliases](#configure-path-aliases)
  - [Run the CLI](#run-the-cli)
  - [Configure `components.json`](#configure-componentsjson)
- [Adding Components](#adding-components)
- [Importing Components](#importing-components)
- [Component List](#component-list)
- [Theming & Dark Mode](#theming--dark-mode)
- [CLI Commands](#cli-commands)
- [FAQ](#faq)

---

## Getting Started

### 1. Create a Project

Use the SvelteKit CLI to bootstrap a new project.

```bash
# pnpm
pnpm dlx sv create my-app

# npm
npm create sv my-app

# bun
bun create sv my-app

# yarn
yarn dlx sv create my-app
```

### 2. Add Tailwind CSS

Add Tailwind to your project with the Svelte CLI.

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

### 3. Configure Path Aliases

If you’re not using the default `$lib` alias, update `svelte.config.js`:

```js
// svelte.config.js
const config = {
  // ... other config
  kit: {
    // ... other config
    alias: {
      "@/*": "./path/to/lib/*",
    },
  },
};
```

### 4. Run the CLI

Initialize shadcn‑svelte in your project:

```bash
# pnpm
pnpm dlx shadcn-svelte@latest init

# npm
npm dlx shadcn-svelte@latest init

# bun
bun dlx shadcn-svelte@latest init

# yarn
yarn dlx shadcn-svelte@latest init
```

### 5. Configure `components.json`

During the init process you’ll be prompted to set up the component configuration:

```
Which base color would you like to use? › Slate
Where is your global CSS file? (this file will be overwritten) › src/app.css
Configure the import alias for lib: › $lib
Configure the import alias for components: › $lib/components
Configure the import alias for utils: › $lib/utils
Configure the import alias for hooks: › $lib/hooks
Configure the import alias for ui: › $lib/components/ui
```

After answering, a `components.json` file will be created in your project root.

---

## Adding Components

Use the CLI to add any component you need. For example, to add a Button:

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

The command will:

1. Download the component files into `$lib/components/ui/button`.
2. Update `components.json` with the new component.
3. Add any required Tailwind utilities.

---

## Importing Components

After adding a component, import it in your Svelte files:

```svelte
<script lang="ts">
  import { Button } from "$lib/components/ui/button/index.js";
</script>

<Button>Click me</Button>
```

> **Tip:** The import path follows the pattern `$lib/components/ui/<component-name>/index.js`.

---

## Component List

Below is a quick reference of all available components.  
(Full documentation for each component can be found in the `docs` folder or the official website.)

| Category | Component |
|----------|-----------|
| **Layout** | Accordion, Alert Dialog, Alert, Aspect Ratio, Avatar, Badge, Breadcrumb, Button, Calendar, Card, Carousel, Chart, Checkbox, Collapsible, Combobox, Command, Context Menu, Data Table, Date Picker, Dialog, Drawer, Dropdown Menu, Formsnap, Hover Card, Input OTP, Input, Label, Menubar, Navigation Menu, Pagination, Popover, Progress, Radio Group, Range Calendar, Resizable, Scroll Area, Select, Separator, Sheet, Sidebar, Skeleton, Slider, Sonner, Switch, Table, Tabs, Textarea, Toggle Group, Toggle, Tooltip, Typography |
| **Utilities** | Theme, Colors, Dark Mode, CLI, JavaScript, Figma, Changelog, Legacy Docs, Migration, Svelte 5, Tailwind v4 |
| **Registry** | registry.json, registry-item.json |

---

## Theming & Dark Mode

shadcn‑svelte supports Tailwind’s dark mode out of the box.  
Add the following to your `tailwind.config.cjs`:

```js
module.exports = {
  darkMode: 'class', // or 'media'
  // ... other config
};
```

Then toggle dark mode by adding the `dark` class to a parent element (e.g., `<html class="dark">`).

---

## CLI Commands

| Command | Description |
|---------|-------------|
| `shadcn-svelte@latest init` | Initialize the library and create `components.json`. |
| `shadcn-svelte@latest add <component>` | Add a component to your project. |
| `shadcn-svelte@latest remove <component>` | Remove a component. |
| `shadcn-svelte@latest upgrade` | Upgrade the library to the latest version. |

---

## FAQ

**Q: How do I change the base color after initialization?**  
A: Edit `components.json` and run `shadcn-svelte@latest upgrade` to regenerate the components with the new color.

**Q: Can I use this library with Astro?**  
A: Yes. The same installation steps apply; just ensure Tailwind is configured in your Astro project.

**Q: Where can I find the source code for a component?**  
A: Components are stored under `$lib/components/ui/<component-name>`. Each component has its own `index.js`, `style.css`, and optional `tests`.

---

Happy building! 🚀