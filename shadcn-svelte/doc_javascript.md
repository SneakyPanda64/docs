# shadcn‑svelte

> A collection of highly‑customizable, accessible UI components for Svelte, inspired by shadcn‑ui.

---

## Table of Contents

- [Introduction](#introduction)
- [Installation](#installation)
  - [SvelteKit](#sveltekit)
  - [Vite](#vite)
  - [Astro](#astro)
  - [Manual Installation](#manual-installation)
- [Configuration](#configuration)
  - [components.json](#componentsjson)
  - [jsconfig.json](#jsconfigjson)
- [Using the CLI](#using-the-cli)
- [JavaScript Support](#javascript-support)
- [Component Registry](#component-registry)
- [Dark Mode](#dark-mode)
- [FAQ](#faq)
- [Examples](#examples)

---

## Introduction

shadcn‑svelte brings the same design system and component philosophy from shadcn‑ui to the Svelte ecosystem. All components are written in TypeScript, but a JavaScript build is available via the CLI for projects that prefer plain JavaScript.

---

## Installation

### SvelteKit

```bash
npm i shadcn-svelte
```

Add the following to your `svelte.config.js`:

```js
import adapter from '@sveltejs/adapter-auto';
import { vitePreprocess } from '@sveltejs/kit/vite';

export default {
  preprocess: vitePreprocess(),
  kit: {
    adapter: adapter()
  }
};
```

### Vite

```bash
npm i shadcn-svelte
```

Create a `vite.config.js`:

```js
import { defineConfig } from 'vite';
import { svelte } from '@sveltejs/vite-plugin-svelte';

export default defineConfig({
  plugins: [svelte()]
});
```

### Astro

```bash
npm i shadcn-svelte
```

Add the Svelte integration to your `astro.config.mjs`:

```js
import { defineConfig } from 'astro/config';
import svelte from '@astrojs/svelte';

export default defineConfig({
  integrations: [svelte()]
});
```

### Manual Installation

If you prefer to add the components manually, copy the `src/lib/components` folder into your project and import the components you need.

---

## Configuration

shadcn‑svelte uses a `components.json` file to configure the build. The file lives at the root of your project.

### components.json

```json
{
  "style": "default",
  "tailwind": {
    "css": "src/app.css"
  },
  "typescript": false,
  "aliases": {
    "utils": "$lib/utils",
    "components": "$lib/components",
    "hooks": "$lib/hooks",
    "ui": "$lib/components/ui"
  },
  "registry": "https://shadcn-svelte.com/registry"
}
```

- **style** – The default Tailwind style preset (`default`, `dark`, etc.).
- **tailwind.css** – Path to your Tailwind CSS file.
- **typescript** – Set to `false` to generate JavaScript builds.
- **aliases** – Custom import aliases for easier module resolution.
- **registry** – URL of the component registry.

### jsconfig.json

If you use import aliases, create a `jsconfig.json` (or `tsconfig.json`) to let your editor resolve them:

```json
{
  "compilerOptions": {
    "paths": {
      "$lib/*": ["./src/lib/*"]
    }
  }
}
```

---

## Using the CLI

The CLI is used to generate JavaScript builds and to scaffold new components.

```bash
npx shadcn-svelte@latest
```

Follow the prompts to set up your project. The CLI will:

1. Create a `components.json` file if it doesn't exist.
2. Generate JavaScript versions of all components when `typescript` is set to `false`.
3. Install any missing dependencies.

---

## JavaScript Support

All components are written in TypeScript, but a JavaScript build is available via the CLI. To opt‑out of TypeScript:

```json
// components.json
{
  "typescript": false
}
```

After running the CLI, you can import components like this:

```svelte
<script>
  import { Button } from '$lib/components/ui/button';
</script>

<Button>Click me</Button>
```

---

## Component Registry

shadcn‑svelte ships with a component registry that contains all available UI components. The registry is automatically fetched from:

```
https://shadcn-svelte.com/registry
```

You can browse the registry in the UI or use the CLI to add components to your project.

---

## Dark Mode

Dark mode is supported out of the box. Add the `dark` class to your root element or use Tailwind’s `dark:` variants.

```html
<div class="dark:bg-gray-900 dark:text-white">
  <!-- Your content -->
</div>
```

---

## FAQ

- **Q: Can I use shadcn‑svelte with plain JavaScript?**  
  **A:** Yes. Set `"typescript": false` in `components.json` and run the CLI to generate JavaScript builds.

- **Q: How do I add a new component?**  
  **A:** Use the CLI: `npx shadcn-svelte@latest add <component-name>`.

- **Q: Where can I find component documentation?**  
  **A:** Visit the official docs at `https://shadcn-svelte.com/docs`.

---

## Examples

### Importing a Button

```svelte
<script>
  import { Button } from '$lib/components/ui/button';
</script>

<Button variant="primary">Primary Button</Button>
```

### Using a Card

```svelte
<script>
  import { Card, CardHeader, CardTitle, CardContent } from '$lib/components/ui/card';
</script>

<Card>
  <CardHeader>
    <CardTitle>Card Title</CardTitle>
  </CardHeader>
  <CardContent>
    <p>This is the card content.</p>
  </CardContent>
</Card>
```

### Dark Mode Toggle

```svelte
<script>
  import { Switch } from '$lib/components/ui/switch';
  let darkMode = false;
</script>

<Switch bind:checked={darkMode} />
```

---

## Contributing

Feel free to open issues or pull requests on the GitHub repository. Contributions that improve component accessibility, add new components, or enhance documentation are welcome.

---

## License

MIT © shadcn‑svelte contributors.