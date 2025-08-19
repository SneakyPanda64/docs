# shadcn‑svelte

> **shadcn‑svelte** is an unofficial, community‑led Svelte port of the shadcn/ui design system.  
> It is *not* a traditional component library; instead it provides the **source code** for each component so you can modify, extend, and distribute them as part of your own design system.

---

## Core Principles

| Principle | What it means |
|-----------|---------------|
| **Open Code** | Every component’s source is exposed. You can edit the code directly instead of wrapping or overriding styles. |
| **Composition** | All components expose a common, composable API. The same patterns apply across the entire system, making it predictable for developers and LLMs alike. |
| **Distribution** | A flat‑file schema (`components.json`) and a CLI let you package, share, and install components across projects and frameworks. |
| **Beautiful Defaults** | Components come with carefully chosen default styles that look good out‑of‑the‑box and mesh well together. |
| **AI‑Ready** | The open, consistent codebase is easy for LLMs to read, understand, and even generate new components. |

---

## How It Works

1. **Open Code** – You see the full implementation of each component.  
   *Example:* If you need a custom button, simply edit `Button.svelte` instead of wrapping it.

2. **Composition** – Every component follows the same interface.  
   *Example:* All components accept a `class` prop for styling and expose a `variant` prop for visual variants.

3. **Distribution** – Use the CLI to publish or install components.  
   *Example:* `shadcn-svelte publish` or `shadcn-svelte install Button`.

4. **Beautiful Defaults** – The default styles are minimal and consistent.  
   *Example:* A `<Button>` renders with a subtle border radius and a neutral background.

5. **AI‑Ready** – The code is structured for easy parsing by language models.  
   *Example:* A model can read `Input.svelte` and suggest a new `PasswordInput` component that reuses the same API.

---

## Components

Below is a list of the components available in the current release.  
(Each component is a Svelte file that can be imported directly.)

- Accordion
- Alert Dialog
- Alert
- Aspect Ratio
- Avatar
- Badge
- Breadcrumb
- Button
- Calendar
- Card
- Carousel
- Chart
- Checkbox
- Collapsible
- Combobox
- Command
- Context Menu
- Data Table
- Date Picker
- Dialog
- Drawer
- Dropdown Menu
- Formsnap
- Hover Card
- Input OTP
- Input
- Label
- Menubar
- Navigation Menu
- Pagination
- Popover
- Progress
- Radio Group
- Range Calendar
- Resizable
- Scroll Area
- Select
- Separator
- Sheet
- Sidebar
- Skeleton
- Slider
- Sonner
- Switch
- Table
- Tabs
- Textarea
- Toggle Group
- Toggle
- Tooltip
- Typography

---

## Installation

> *The following steps assume you are using a modern Svelte setup (SvelteKit, Vite, Astro, etc.).*

1. **Add the package**  
   ```bash
   npm install shadcn-svelte
   ```

2. **Import components**  
   ```svelte
   <script>
     import { Button } from 'shadcn-svelte';
   </script>

   <Button variant="primary">Click me</Button>
   ```

> *If you prefer manual installation, copy the component files into your project and import them directly.*

---

## Theming & Dark Mode

- The components are built with Tailwind CSS, so you can use Tailwind’s dark mode utilities to switch themes.
- Each component accepts a `class` prop, allowing you to override or extend styles as needed.

---

## CLI & Schema

- **Schema** – `components.json` defines each component’s metadata, dependencies, and props.
- **CLI** – `shadcn-svelte` provides commands to publish, install, and manage components across projects.

---

## Getting Started

1. **Create a new Svelte project** (SvelteKit, Vite, Astro, etc.).
2. **Install shadcn‑svelte** as shown above.
3. **Start building** – import components, tweak defaults, or create new ones.

---

## FAQ

- **Is shadcn‑svelte officially supported by shadcn?**  
  No, it is a community‑led port. We have received permission from shadcn to create this version.

- **Can I use these components in a non‑Svelte project?**  
  The CLI supports cross‑framework distribution, but the source files are Svelte components.

- **How do I pull upstream updates?**  
  Since the code is open, you can merge changes from the upstream repository directly into your fork or copy the updated files.

---

## Resources

- **Documentation** – This guide
- **CLI** – `shadcn-svelte --help`
- **Community** – Join the Discord or GitHub discussions for support

---

*shadcn‑svelte* empowers you to build a fully custom, AI‑friendly component library that stays in sync with your design system. Happy coding!