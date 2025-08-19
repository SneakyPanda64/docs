# shadcn‑svelte

> **shadcn‑svelte** – A community‑led, open‑code Svelte port of the shadcn/ui component system.  
> It is *not* a traditional component library; it is a **framework for building your own component library** with composable, customizable, and AI‑ready components.

---

## Table of Contents

- [Overview](#overview)
- [Installation](#installation)
  - [SvelteKit](#sveltekit)
  - [Vite](#vite)
  - [Astro](#astro)
  - [Manual Installation](#manual-installation)
- [Theming & Dark Mode](#theming--dark-mode)
- [CLI](#cli)
- [Component Schema](#component-schema)
- [Distribution](#distribution)
- [Examples](#examples)
- [FAQ](#faq)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

shadcn‑svelte is built around four core principles:

| Principle | What it means |
|-----------|----------------|
| **Open Code** | The component source is fully exposed. Edit it directly instead of wrapping or overriding. |
| **Composition** | Every component exposes a predictable, composable API. |
| **Distribution** | A flat‑file schema + CLI makes it trivial to ship components across projects. |
| **Beautiful Defaults** | Carefully chosen default styles give you a clean, consistent UI out‑of‑the‑box. |

> **Why not a traditional library?**  
> Traditional libraries ship pre‑styled components that are hard to customize. With shadcn‑svelte you own the code, so you can tweak any part of a component without fighting CSS or API quirks.

---

## Installation

### SvelteKit

```bash
npm i shadcn-svelte
```

```svelte
<script>
  import { Button } from 'shadcn-svelte';
</script>

<Button>Click me</Button>
```

### Vite

```bash
npm i shadcn-svelte
```

```svelte
<script>
  import { Card } from 'shadcn-svelte';
</script>

<Card>
  <h2>Card Title</h2>
  <p>Card content goes here.</p>
</Card>
```

### Astro

```bash
npm i shadcn-svelte
```

```astro
---
import { Alert } from 'shadcn-svelte';
---
<Alert variant="warning">This is a warning alert.</Alert>
```

### Manual Installation

If you prefer to copy the source directly:

1. Clone the repo:  
   ```bash
   git clone https://github.com/huntabyte/shadcn-svelte.git
   ```
2. Copy the `src/components` folder into your project.
3. Import components as needed.

---

## Theming & Dark Mode

shadcn‑svelte uses Tailwind CSS for styling.  
Add the following to your `tailwind.config.cjs`:

```js
module.exports = {
  darkMode: 'class', // or 'media'
  theme: {
    extend: {
      colors: {
        // Add your custom colors here
      }
    }
  }
};
```

Toggle dark mode by adding the `dark` class to the `<html>` or `<body>` element.

```svelte
<script>
  import { Switch } from 'shadcn-svelte';
  let dark = false;
</script>

<Switch bind:checked={dark} on:change={() => document.documentElement.classList.toggle('dark', dark)} />
```

---

## CLI

shadcn‑svelte ships a CLI for component distribution.

```bash
npx shadcn-svelte-cli init
```

### Commands

| Command | Description |
|---------|-------------|
| `init` | Create a new component schema. |
| `add <component>` | Add a component to the schema. |
| `publish` | Publish components to a registry. |
| `install <registry>` | Install components from a registry. |

> **Example** – Adding a new component:

```bash
npx shadcn-svelte-cli add Button
```

---

## Component Schema

Components are described in a flat‑file JSON schema.  
Typical files:

- `components.json` – Root schema listing all components.
- `registry.json` – Registry of available components.
- `registry-item.json` – Individual component definition.

### `components.json`

```json
{
  "components": [
    {
      "name": "Button",
      "path": "./src/components/Button.svelte",
      "props": {
        "variant": "string",
        "size": "string"
      }
    },
    {
      "name": "Card",
      "path": "./src/components/Card.svelte",
      "props": {
        "title": "string"
      }
    }
  ]
}
```

### `registry.json`

```json
{
  "registry": [
    {
      "name": "Button",
      "description": "A clickable button component."
    },
    {
      "name": "Card",
      "description": "A container with optional header and footer."
    }
  ]
}
```

### `registry-item.json`

```json
{
  "name": "Button",
  "props": {
    "variant": {
      "type": "string",
      "default": "primary"
    },
    "size": {
      "type": "string",
      "default": "md"
    }
  },
  "example": "<Button variant=\"secondary\" size=\"lg\">Submit</Button>"
}
```

---

## Distribution

1. **Create a registry** – Store your component definitions in a Git repo or a static file server.
2. **Publish** – Use the CLI to push your components to the registry.
3. **Install** – Other projects can pull the components with:

```bash
npx shadcn-svelte-cli install https://my-registry.com/registry.json
```

The CLI will copy the component files into the target project and update the schema automatically.

---

## Examples

Below are quick usage examples for some core components.

### Button

```svelte
<script>
  import { Button } from 'shadcn-svelte';
</script>

<Button variant="primary" size="md">Primary</Button>
<Button variant="secondary" size="lg">Secondary</Button>
```

### Card

```svelte
<script>
  import { Card } from 'shadcn-svelte';
</script>

<Card title="Welcome">
  <p>This is a simple card component.</p>
</Card>
```

### Alert

```svelte
<script>
  import { Alert } from 'shadcn-svelte';
</script>

<Alert variant="error">Something went wrong.</Alert>
```

---

## FAQ

| Question | Answer |
|----------|--------|
| **Can I use shadcn‑svelte with Tailwind v4?** | Yes, the library is compatible with Tailwind v4. |
| **Is this library maintained?** | It is community‑led; contributions are welcome. |
| **How do I contribute?** | Fork the repo, create a PR, and follow the contribution guidelines. |

---

## Contributing

1. Fork the repository.  
2. Create a feature branch: `git checkout -b feature/your-feature`.  
3. Commit your changes.  
4. Open a pull request.  

Please run the test suite before submitting:

```bash
npm test
```

---

## License

MIT © shadcn‑svelte contributors

---