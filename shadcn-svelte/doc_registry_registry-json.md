# shadcn‑svelte – Component Registry Documentation

> **shadcn‑svelte** is a collection of UI components built with Svelte and Tailwind CSS.  
> This guide explains how to set up a custom component registry, the JSON schema that powers it, and how to configure imports, dependencies, and more.

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [Components Overview](#components-overview)
3. [Registry JSON Schema](#registry-json-schema)
4. [Definitions & Properties](#definitions--properties)
5. [Aliases](#aliases)
6. [Override Dependencies](#override-dependencies)
7. [Examples](#examples)
8. [Further Reading](#further-reading)

---

## 1. Getting Started

### Installation

```bash
# Using npm
npm install shadcn-svelte

# Using pnpm
pnpm add shadcn-svelte

# Using yarn
yarn add shadcn-svelte
```

> **Tip:** For a full SvelteKit or Vite setup, see the official [Installation](https://shadcn-svelte.com/docs/installation) guide.

---

## 2. Components Overview

| Category | Component | Description |
|----------|-----------|-------------|
| **UI** | Accordion, Alert Dialog, Alert, Aspect Ratio, Avatar, Badge, Breadcrumb, Button, Calendar, Card, Carousel, Chart, Checkbox, Collapsible, Combobox, Command, Context Menu, Data Table, Date Picker, Dialog, Drawer, Dropdown Menu, Formsnap, Hover Card, Input OTP, Input, Label, Menubar, Navigation Menu, Pagination, Popover, Progress, Radio Group, Range Calendar, Resizable, Scroll Area, Select, Separator, Sheet, Sidebar, Skeleton, Slider, Sonner, Switch, Table, Tabs, Textarea, Toggle Group, Toggle, Tooltip, Typography | A comprehensive list of UI components. |
| **Blocks** | … | Custom blocks for rapid prototyping. |
| **Charts** | … | Data visualization components. |
| **Themes** | … | Theme utilities and color palettes. |

> *All components are fully typed and come with Tailwind CSS utilities.*

---

## 3. Registry JSON Schema

The `registry.json` file defines your custom component registry. It follows the schema located at:

```
https://shadcn-svelte.com/schema/registry.json
```

### Minimal Example

```json
{
  "$schema": "https://shadcn-svelte.com/schema/registry.json",
  "name": "shadcn-svelte",
  "homepage": "https://shadcn-svelte.com",
  "items": [
    {
      "name": "hello-world",
      "type": "registry:block",
      "title": "Hello World",
      "description": "A simple hello world component.",
      "files": [
        {
          "path": "src/lib/registry/blocks/hello-world/hello-world.svelte",
          "type": "registry:component"
        }
      ]
    }
  ]
}
```

---

## 4. Definitions & Properties

| Property | Description | Example |
|----------|-------------|---------|
| **$schema** | Points to the JSON schema for validation. | `"$schema": "https://shadcn-svelte.com/schema/registry.json"` |
| **name** | Registry name (used for data attributes). | `"name": "acme"` |
| **homepage** | URL of the registry’s homepage. | `"homepage": "https://acme.com"` |
| **items** | Array of registry items (components, blocks, etc.). | See the *Minimal Example* above. |
| **aliases** | Import path transformations. | See the *Aliases* section. |
| **overrideDependencies** | Force specific dependency versions. | See the *Override Dependencies* section. |

> **Note:** Each item in `items` must conform to the `registry-item` schema.

---

## 5. Aliases

Aliases map internal import paths to the paths that users will see after installation. They should mirror the import statements used inside your registry code.

### Example

If your component imports like this:

```svelte
<script lang="ts">
  import { Button } from "@/lib/registry/ui/button/index.js";
  import { cn } from "@/lib/utils.js";
</script>
```

Your `registry.json` should contain:

```json
{
  "aliases": {
    "lib": "@/lib",
    "ui": "@/lib/registry/ui",
    "components": "@/lib/registry/components",
    "utils": "@/lib/utils",
    "hooks": "@/lib/hooks"
  }
}
```

### Default Aliases

If you omit `aliases`, the following defaults are applied:

```json
{
  "aliases": {
    "lib": "$lib/registry/lib",
    "ui": "$lib/registry/ui",
    "components": "$lib/registry/components",
    "utils": "$lib/utils",
    "hooks": "$lib/registry/hooks"
  }
}
```

---

## 6. Override Dependencies

`overrideDependencies` lets you enforce specific version ranges for dependencies, overriding what the registry build detects from your `package.json`.

### Common Use Cases

| Scenario | Example |
|----------|---------|
| **Latest pre‑release** | `"overrideDependencies": ["paneforge@next"]` |
| **Pin to a specific version** | `"overrideDependencies": ["dep@1.5.0"]` |

> **Caution:** Overriding dependencies can cause version conflicts. Use sparingly.

### Example Transformation

**Your registry’s `package.json`:**

```json
{
  "dependencies": {
    "paneforge": "1.0.0-next.1"
  }
}
```

**After user installation with override:**

```json
{
  "dependencies": {
    "paneforge": "1.0.0-next.5" // overridden by "paneforge@next"
  }
}
```

---

## 7. Examples

### `registry-item.json`

```json
{
  "$schema": "https://shadcn-svelte.com/schema/registry-item.json",
  "name": "hello-world",
  "type": "registry:block",
  "title": "Hello World",
  "description": "A simple hello world component.",
  "files": [
    {
      "path": "src/lib/registry/blocks/hello-world/hello-world.svelte",
      "type": "registry:component"
    }
  ]
}
```

> *Built by shadcn, ported to Svelte by Huntabyte & CokaKoala.*

---

## 8. Further Reading

- **Installation Guides** – SvelteKit, Vite, Astro, Manual Installation  
- **Dark Mode** – Theming and dark mode support  
- **CLI** – Command‑line tools for component generation  
- **Figma** – Design system integration  
- **Changelog** – Release notes  
- **Legacy Docs** – Migration guides  
- **Svelte 5 / Tailwind v4** – Compatibility notes  

> For the full documentation, visit the official site: <https://shadcn-svelte.com/docs>.

---