# shadcn‑svelte

> **⚠️ Disclaimer**  
> This project is an unofficial, community‑led Svelte port of [shadcn/ui](https://github.com/shadcn/ui).  
> We are not affiliated with shadcn, but we received his blessing before creating this Svelte version.

---

## Table of Contents

1. [Introduction](#introduction)  
2. [Core Principles](#core-principles)  
   - [Open Code](#open-code)  
   - [Composition](#composition)  
   - [Distribution](#distribution)  
   - [Beautiful Defaults](#beautiful-defaults)  
   - [AI‑Ready](#ai-ready)  
3. [Installation](#installation)  
4. [CLI & Schema](#cli‑and‑schema)  
5. [Component List](#component-list)  
6. [Examples](#examples)  
7. [FAQ & Resources](#faq‑and‑resources)

---

## Introduction

shadcn‑svelte is a **component‑building framework** rather than a traditional component library.  
It gives you the actual component source code so you can:

- Inspect how a component works  
- Modify it directly  
- Extend it without wrappers or style hacks  

The goal is to make building a custom design system fast, predictable, and AI‑friendly.

---

## Core Principles

### Open Code

* **Full Transparency** – Every component’s source is exposed.  
* **Easy Customization** – Edit the component file directly.  
* **AI Integration** – LLMs can read, understand, and improve your components.

> *Typical library workflow:*  
> ```bash
> npm install @shadcn/ui
> import { Button } from '@shadcn/ui'
> ```  
> *shadcn‑svelte workflow:*  
> ```bash
> npm install shadcn-svelte
> // Edit src/lib/Button.svelte directly
> ```

### Composition

All components expose a **common, composable API**.  
If a component is missing, we add it, make it composable, and style it to match the rest of the system.

### Distribution

shadcn‑svelte ships with:

* **Flat‑file schema** – `registry.json` defines components, dependencies, and props.  
* **CLI** – `shadcn-svelte` command to distribute and install components across projects.

```json
// registry.json (excerpt)
{
  "components": [
    {
      "name": "Button",
      "file": "src/lib/Button.svelte",
      "props": ["variant", "size", "disabled"]
    },
    {
      "name": "Alert",
      "file": "src/lib/Alert.svelte",
      "props": ["type", "dismissible"]
    }
  ]
}
```

### Beautiful Defaults

* Clean, minimal look out‑of‑the‑box.  
* Consistent design across components.  
* Simple overrides for brand‑specific styling.

### AI‑Ready

Because the code is open and the API is consistent, LLMs can:

* Learn component behavior.  
* Suggest improvements.  
* Generate new components that fit the existing design system.

---

## Installation

### SvelteKit

```bash
npm i shadcn-svelte
```

Add the following to `src/routes/+layout.svelte` (or wherever you want the Tailwind config):

```svelte
<script>
  import { ThemeProvider } from 'shadcn-svelte';
</script>

<ThemeProvider>
  <slot />
</ThemeProvider>
```

### Vite

```bash
npm i shadcn-svelte
```

Add the Tailwind plugin in `vite.config.js`:

```js
import { defineConfig } from 'vite';
import svelte from '@sveltejs/vite-plugin-svelte';
import shadcnSvelte from 'shadcn-svelte/vite';

export default defineConfig({
  plugins: [svelte(), shadcnSvelte()]
});
```

### Astro

```bash
npm i shadcn-svelte
```

Add the plugin in `astro.config.mjs`:

```js
import { defineConfig } from 'astro/config';
import shadcnSvelte from 'shadcn-svelte/astro';

export default defineConfig({
  integrations: [shadcnSvelte()]
});
```

---

## CLI & Schema

The CLI helps you distribute components between projects.

```bash
# Install the CLI globally
npm i -g shadcn-svelte

# Publish a component package
shadcn-svelte publish

# Install a component from a registry
shadcn-svelte install Button
```

The `registry.json` file lives at the root of your project and looks like the snippet above.  
You can add new components by editing this file and running `shadcn-svelte publish`.

---

## Component List

| Category | Component | Description |
|----------|-----------|-------------|
| **UI** | Accordion | Collapsible panels |
| | Alert | Notification messages |
| | Badge | Small status indicators |
| | Button | Primary action button |
| | Card | Content container |
| | Dialog | Modal dialog |
| | Dropdown Menu | Contextual menu |
| | Input | Text input field |
| | Label | Form label |
| | Radio Group | Radio buttons |
| | Select | Dropdown selector |
| | Slider | Range slider |
| | Switch | Toggle switch |
| | Table | Data table |
| | Tabs | Tabbed navigation |
| | Tooltip | Hover tooltip |
| | Typography | Headings, paragraphs, etc. |
| **Layout** | Breadcrumb | Navigation trail |
| | Sidebar | Side navigation |
| | Skeleton | Loading placeholder |
| | Scroll Area | Custom scrollable area |
| | Resizable | Resizable panels |
| | Skeleton | Loading placeholder |
| **Data** | Data Table | Advanced table with sorting |
| | Chart | Data visualization |
| | Calendar | Date picker |
| | Range Calendar | Date range picker |
| **Navigation** | Menubar | Top navigation bar |
| | Navigation Menu | Nested navigation |
| | Pagination | Page navigation |
| | Sidebar | Side navigation |
| **Feedback** | Alert Dialog | Confirmation dialog |
| | Dialog | Modal dialog |
| | Drawer | Slide‑in panel |
| | Popover | Hover/focus popover |
| | Tooltip | Hover tooltip |
| **Forms** | Checkbox | Checkbox input |
| | Input OTP | One‑time password input |
| | Input | Text input |
| | Label | Form label |
| | Radio Group | Radio buttons |
| | Select | Dropdown selector |
| | Switch | Toggle switch |
| | Textarea | Multi‑line text input |
| | Toggle | Toggle button |
| | Toggle Group | Group of toggles |
| **Misc** | Avatar | User avatar |
| | Badge | Small status indicator |
| | Skeleton | Loading placeholder |
| | Sonner | Toast notifications |
| | Hover Card | Hover card preview |

---

## Examples

### Using a Button

```svelte
<script>
  import Button from 'shadcn-svelte/src/lib/Button.svelte';
</script>

<Button variant="primary" size="lg">
  Submit
</Button>
```

### Customizing a Component

Edit `src/lib/Button.svelte`:

```svelte
<script>
  export let variant = 'primary';
  export let size = 'md';
  export let disabled = false;
</script>

<button
  class={`btn ${variant} ${size}`}
  disabled={disabled}
>
  <slot />
</button>

<style>
  .btn { /* default styles */ }
  .primary { /* primary variant */ }
  .lg { /* large size */ }
</style>
```

### Adding a New Component via Schema

1. Create `src/lib/MyComponent.svelte`.  
2. Add to `registry.json`:

```json
{
  "name": "MyComponent",
  "file": "src/lib/MyComponent.svelte",
  "props": ["title", "content"]
}
```

3. Publish:

```bash
shadcn-svelte publish
```

Now any project can install it:

```bash
shadcn-svelte install MyComponent
```

---

## FAQ & Resources

| Question | Answer |
|----------|--------|
| **How do I pull upstream updates?** | Since the code is open, you can `git pull` from the upstream repo or use the CLI to sync. |
| **Can I use this with Tailwind v4?** | Yes – the project supports Tailwind v4. |
| **Is this a library?** | No, it’s a framework for building your own component library. |
| **Where can I find the changelog?** | See the `CHANGELOG.md` in the repo. |
| **How do I contribute?** | Fork the repo, make changes, and open a PR. |

---

### Further Reading

* [Official shadcn/ui](https://github.com/shadcn/ui) – the original React library.  
* [SvelteKit Docs](https://kit.svelte.dev/docs) – for SvelteKit integration.  
* [Tailwind CSS Docs](https://tailwindcss.com/docs) – for styling.  

---