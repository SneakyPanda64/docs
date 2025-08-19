# shadcn‑svelte

> **⚠️ Disclaimer**  
> This project is an unofficial, community‑led Svelte port of the original `shadcn/ui`.  
> We are *not* affiliated with shadcn, but we received his blessing before creating this Svelte version.

---

## Table of Contents

- [Overview](#overview)
- [Why shadcn‑svelte?](#why-shadcn-svelte)
- [Key Principles](#key-principles)
  - [Open Code](#open-code)
  - [Composition](#composition)
  - [Distribution](#distribution)
  - [Beautiful Defaults](#beautiful-defaults)
  - [AI‑Ready](#ai-ready)
- [Installation](#installation)
  - [SvelteKit](#sveltekit)
  - [Vite](#vite)
  - [Astro](#astro)
  - [Manual Installation](#manual-installation)
- [Theming & Dark Mode](#theming--dark-mode)
- [CLI](#cli)
- [Component Registry](#component-registry)
- [Examples](#examples)
- [FAQ](#faq)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

`shadcn‑svelte` is a collection of reusable UI components built with **Bits UI** and **Tailwind CSS**.  
Unlike traditional component libraries that ship pre‑compiled binaries, this project gives you the *actual source code* of every component, so you can:

- Inspect how a component works
- Modify it directly
- Extend it without fighting with CSS overrides
- Let LLMs read, understand, and improve your UI

---

## Why shadcn‑svelte?

Traditional component libraries often force you to:

- Wrap components to change behaviour
- Write CSS hacks to override styles
- Mix incompatible APIs from different libraries

`shadcn‑svelte` solves this by:

- Providing **open source** component code
- Using a **composable API** that is consistent across all components
- Offering a **flat‑file schema** and a CLI for easy distribution
- Shipping with **beautiful defaults** that look great out‑of‑the‑box
- Being **AI‑ready** – the code is easy for LLMs to read and generate new components

---

## Key Principles

### Open Code

- **Full Transparency** – see the implementation of every component
- **Easy Customization** – edit the component file directly
- **AI Integration** – LLMs can read and improve your components

### Composition

- Every component exposes a **common, composable interface**
- Predictable APIs mean you can learn one pattern and reuse it everywhere
- Third‑party components can be wrapped to match the same interface

### Distribution

- **Schema** – a flat‑file structure (`components.json`, `registry.json`, `registry-item.json`) that defines components, dependencies, and props
- **CLI** – a command‑line tool that installs and distributes components across projects, with cross‑framework support

### Beautiful Defaults

- Components come with carefully chosen default styles
- They look good on their own and fit together seamlessly
- Customization is straightforward – just override the Tailwind classes you need

### AI‑Ready

- Consistent API + open code = easy for LLMs to understand
- AI can suggest improvements or generate new components that integrate with your design system

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

1. Clone the repo  
   `git clone https://github.com/your-org/shadcn-svelte.git`
2. Copy the `src/components` folder into your project
3. Import components as needed

---

## Theming & Dark Mode

`shadcn-svelte` uses Tailwind’s dark mode utilities.  
Add the following to your `tailwind.config.cjs`:

```js
module.exports = {
  darkMode: 'class', // or 'media'
  // ...
};
```

Wrap your app in a `<div class="dark">` to enable dark mode:

```svelte
<div class="dark">
  <Button variant="primary">Dark Mode Button</Button>
</div>
```

---

## CLI

The CLI helps you distribute and install components across projects.

```bash
# Install the CLI globally
npm i -g shadcn-svelte-cli

# List available components
svelte-cli list

# Install a component
svelte-cli install Button

# Publish a component to your registry
svelte-cli publish Button
```

---

## Component Registry

The registry is a flat‑file schema that describes each component:

- `components.json` – top‑level component definitions
- `registry.json` – registry of all components
- `registry-item.json` – individual component metadata

Example `components.json`:

```json
{
  "Button": {
    "props": {
      "variant": "string",
      "size": "string"
    },
    "dependencies": ["Icon"]
  }
}
```

---

## Examples

Below are a few example usages of the most common components.

### Button

```svelte
<script>
  import { Button } from 'shadcn-svelte';
</script>

<Button variant="primary" size="lg">Primary Button</Button>
```

### Card

```svelte
<script>
  import { Card } from 'shadcn-svelte';
</script>

<Card>
  <h3 class="text-xl font-semibold">Card Title</h3>
  <p class="mt-2 text-sm text-gray-600">Card description goes here.</p>
</Card>
```

### Alert

```svelte
<script>
  import { Alert } from 'shadcn-svelte';
</script>

<Alert variant="error">
  <Alert.Title>Error</Alert.Title>
  <Alert.Description>Something went wrong.</Alert.Description>
</Alert>
```

---

## FAQ

| Question | Answer |
|----------|--------|
| **Do I need to use Tailwind?** | Yes, Tailwind CSS is required for styling. |
| **Can I use this with React?** | No, this library is Svelte‑only. |
| **How do I contribute?** | See the [Contributing](#contributing) section. |

---

## Contributing

1. Fork the repo  
2. Create a feature branch  
3. Commit your changes  
4. Open a pull request

We welcome PRs that add new components, improve documentation, or fix bugs.

---

## License

MIT © 2025 shadcn‑svelte contributors

---