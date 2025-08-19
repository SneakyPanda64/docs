# shadcn‑svelte – Quickstart & Reference

> **shadcn‑svelte** is a component library for Svelte that follows the design principles of shadcn‑ui.  
> This guide covers the minimal setup for a Vite project, the required TypeScript configuration, and how to add components via the CLI.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Installation](#installation)
   - [Add TailwindCSS](#add-tailwindcss)
   - [TypeScript Configuration](#typescript-configuration)
   - [Vite Alias Setup](#vite-alias-setup)
3. [CLI Usage](#cli-usage)
   - [Initial Setup](#initial-setup)
   - [Adding Components](#adding-components)
4. [Importing Components](#importing-components)
5. [Supported Platforms](#supported-platforms)
6. [Further Reading](#further-reading)

---

## 1. Prerequisites

- Node.js ≥ 18
- Vite (or any bundler that supports Svelte)
- TailwindCSS (v3+)
- TypeScript (optional but recommended)

---

## 2. Installation

### Add TailwindCSS

Use the Svelte CLI to add Tailwind to your project:

```bash
# Choose your package manager
pnpm dlx sv add tailwindcss
# or
npm i -D tailwindcss
```

### TypeScript Configuration

The Vite project splits the TypeScript config into three files.  
Add the following to **`tsconfig.json`**:

```json
{
  "files": [],
  "references": [
    { "path": "./tsconfig.app.json" },
    { "path": "./tsconfig.node.json" }
  ],
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "$lib": ["./src/lib"],
      "$lib/*": ["./src/lib/*"]
    }
  }
}
```

Add the same `paths` section to **`tsconfig.app.json`** (for IDE resolution):

```json
{
  "compilerOptions": {
    // ...other options
    "baseUrl": ".",
    "paths": {
      "$lib": ["./src/lib"],
      "$lib/*": ["./src/lib/*"]
    }
  }
}
```

### Vite Alias Setup

Update **`vite.config.ts`** so Vite can resolve `$lib` imports:

```ts
import path from "path";
import { defineConfig } from "vite";
import { svelte } from "@sveltejs/vite-plugin-svelte";

export default defineConfig({
  plugins: [svelte()],
  resolve: {
    alias: {
      $lib: path.resolve("./src/lib")
    }
  }
});
```

---

## 3. CLI Usage

### Initial Setup

Run the shadcn‑svelte CLI to generate `components.json` and configure paths:

```bash
pnpm dlx shadcn-svelte@latest init
```

You’ll be prompted for:

| Prompt | Example Answer |
|--------|----------------|
| Which base color would you like to use? | Slate |
| Where is your global CSS file? (this file will be overwritten) | `src/app.css` |
| Configure the import alias for lib | `$lib` |
| Configure the import alias for components | `$lib/components` |
| Configure the import alias for utils | `$lib/utils` |
| Configure the import alias for hooks | `$lib/hooks` |
| Configure the import alias for ui | `$lib/components/ui` |

### Adding Components

To add a component, use the CLI:

```bash
pnpm dlx shadcn-svelte@latest add button
```

The command will:

1. Download the component files into `src/lib/components/ui/button`.
2. Update `components.json` with the new component.

---

## 4. Importing Components

After adding a component, import it in your Svelte files:

```svelte
<script lang="ts">
  import { Button } from "$lib/components/ui/button/index.js";
</script>

<Button>Click me</Button>
```

> **Tip:** The import path ends with `/index.js` because the CLI generates a JavaScript entry file for each component.

---

## 5. Supported Platforms

- **SvelteKit** – Works out‑of‑the‑box with the above configuration.
- **Astro** – Use the same Tailwind + TypeScript setup; import components via `$lib/...`.

---

## 6. Further Reading

- **[Official Docs](https://shadcn-svelte.com/docs)** – Full component list, theming, and advanced usage.
- **[Changelog](https://shadcn-svelte.com/changelog)** – Keep track of breaking changes.
- **[Migration Guide](https://shadcn-svelte.com/migration)** – If upgrading from an older version.

---

**Happy coding!**