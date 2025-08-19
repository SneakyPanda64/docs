# shadcn‑svelte

> **shadcn‑svelte** – A collection of UI components for Svelte, inspired by shadcn‑ui.  
> Built with Tailwind CSS and fully themable, it ships with a CLI, a registry, and support for dark mode.

---

## Table of Contents

1. [Introduction](#introduction)  
2. [Installation](#installation)  
   - [SvelteKit](#sveltekit)  
   - [Vite](#vite)  
   - [Astro](#astro)  
   - [Manual Installation](#manual-installation)  
3. [Theming](#theming)  
4. [Dark Mode](#dark-mode)  
5. [CLI](#cli)  
6. [Components](#components)  
7. [Registry](#registry)  
8. [FAQ](#faq)  
9. [Examples](#examples)  

---

## Introduction

shadcn‑svelte brings a set of ready‑to‑use, highly‑customisable UI components to Svelte projects.  
All components are built with Tailwind CSS, follow the shadcn‑ui design principles, and are fully accessible.

---

## Installation

### SvelteKit

```bash
# Using npm
npm i shadcn-svelte

# Using pnpm
pnpm add shadcn-svelte

# Using yarn
yarn add shadcn-svelte
```

> **Tip** – After installing, import the global styles in `src/app.html` or your root component:

```svelte
<script>
  import 'shadcn-svelte/styles.css';
</script>
```

### Vite

```bash
# Using npm
npm i shadcn-svelte

# Using pnpm
pnpm add shadcn-svelte

# Using yarn
yarn add shadcn-svelte
```

> **Tip** – Add the Tailwind directives to your `index.css`:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### Astro

```bash
# Using npm
npm i shadcn-svelte

# Using pnpm
pnpm add shadcn-svelte

# Using yarn
yarn add shadcn-svelte
```

> **Tip** – In `src/components/` create a Svelte component and import it into your Astro page.

### Manual Installation

If you prefer to add the library manually:

1. Download the latest release from the GitHub releases page.  
2. Copy the `dist` folder into your project.  
3. Import the CSS and components as needed.

---

## Theming

shadcn‑svelte supports a fully‑customisable theme system.  
Add a `theme.config.js` (or `.ts`) file at the root of your project:

```js
export default {
  colors: {
    primary: '#3b82f6',
    secondary: '#6b7280',
    // …
  },
  // Add more theme options here
};
```

Then import the theme in your root component:

```svelte
<script>
  import { ThemeProvider } from 'shadcn-svelte';
  import theme from '../theme.config.js';
</script>

<ThemeProvider {theme}>
  <slot />
</ThemeProvider>
```

---

## Dark Mode

Dark mode is enabled by default.  
To toggle dark mode programmatically:

```svelte
<script>
  import { useDarkMode } from 'shadcn-svelte';
  const { isDark, toggle } = useDarkMode();
</script>

<button on:click={toggle}>
  {isDark ? 'Light Mode' : 'Dark Mode'}
</button>
```

You can also set the initial mode via CSS:

```css
/* In your global CSS */
:root {
  color-scheme: light dark;
}
```

---

## CLI

shadcn‑svelte ships with a CLI to scaffold components, pages, and utilities.

```bash
# Install globally
npm i -g shadcn-svelte-cli

# Generate a new component
shadcn-svelte generate component Button

# Generate a new page
shadcn-svelte generate page About
```

The CLI will create the component file, add it to the registry, and update your Tailwind configuration automatically.

---

## Components

Below is a quick reference of the available components.  
Each component is fully documented in its own file under `src/lib/components`.

| Component | Description |
|-----------|-------------|
| **Accordion** | Collapsible panels with optional animation. |
| **Alert Dialog** | Modal dialog for critical alerts. |
| **Alert** | Inline alert messages. |
| **Aspect Ratio** | Maintains a fixed aspect ratio for media. |
| **Avatar** | User avatar with fallback. |
| **Badge** | Small status indicators. |
| **Breadcrumb** | Navigation breadcrumb trail. |
| **Button** | Primary, secondary, and variant buttons. |
| **Calendar** | Date picker component. |
| **Card** | Content container with optional header/footer. |
| **Carousel** | Image or content carousel. |
| **Chart** | Data visualization charts. |
| **Checkbox** | Accessible checkbox input. |
| **Collapsible** | Generic collapsible container. |
| **Combobox** | Autocomplete dropdown. |
| **Command** | Command palette. |
| **Context Menu** | Right‑click context menu. |
| **Data Table** | Tabular data with sorting/pagination. |
| **Date Picker** | Inline date picker. |
| **Dialog** | Modal dialog. |
| **Drawer** | Slide‑in panel. |
| **Dropdown Menu** | Dropdown menu. |
| **Formsnap** | Form snapshot utility. |
| **Hover Card** | Hover‑activated card. |
| **Input OTP** | One‑time‑password input. |
| **Input** | Text input. |
| **Label** | Form label. |
| **Menubar** | Top‑level menu bar. |
| **Navigation Menu** | Navigation menu. |
| **Pagination** | Pagination controls. |
| **Popover** | Popover component. |
| **Progress** | Progress bar. |
| **Radio Group** | Radio button group. |
| **Range Calendar** | Calendar with date range selection. |
| **Resizable** | Resizable panels. |
| **Scroll Area** | Custom scrollable area. |
| **Select** | Select dropdown. |
| **Separator** | Visual separator. |
| **Sheet** | Bottom sheet. |
| **Sidebar** | Sidebar navigation. |
| **Skeleton** | Loading skeletons. |
| **Slider** | Range slider. |
| **Sonner** | Toast notifications. |
| **Switch** | Toggle switch. |
| **Table** | Data table. |
| **Tabs** | Tabbed interface. |
| **Textarea** | Text area. |
| **Toggle Group** | Group of toggle buttons. |
| **Toggle** | Toggle button. |
| **Tooltip** | Tooltip component. |
| **Typography** | Headings, paragraphs, etc. |

---

## Registry

shadcn‑svelte maintains a component registry (`registry.json`) that tracks all available components and their metadata.  
You can view or modify the registry to add custom components or adjust existing ones.

```json
{
  "components": [
    {
      "name": "Button",
      "path": "src/lib/components/Button.svelte",
      "description": "Primary button component."
    },
    // …
  ]
}
```

---

## FAQ

| Question | Answer |
|----------|--------|
| **How do I customize a component?** | Override the component’s props or create a wrapper component that passes custom classes. |
| **Can I use shadcn‑svelte with Tailwind v4?** | Yes, the library is compatible with Tailwind v4. |
| **Is there support for Svelte 5?** | The library is actively maintained and will support Svelte 5 once released. |
| **Where can I report bugs?** | Open an issue on the GitHub repository. |

---

## Examples

> *No code examples are included in the original documentation snippet.  
> If you need example usage for a specific component, refer to the component’s own documentation file in `src/lib/components/`.*

---

**Enjoy building beautiful, accessible UIs with shadcn‑svelte!**