# shadcn‑svelte – Component Registry Documentation

This guide walks you through creating, building, and publishing a **shadcn‑svelte** component registry.  
All examples shown below are kept verbatim from the original source.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Getting Started](#getting-started)
3. [Registry Schema](#registry-schema)
4. [Adding a Registry Item](#adding-a-registry-item)
5. [Building the Registry](#building-the-registry)
6. [Serving the Registry](#serving-the-registry)
7. [Publishing the Registry](#publishing-the-registry)
8. [Adding Authentication](#adding-authentication)
9. [Guidelines](#guidelines)
10. [CLI Commands](#cli-commands)

---

## Prerequisites

- Node.js (≥ 18)
- A package manager (`pnpm`, `npm`, `bun`, or `yarn`)
- A SvelteKit / Vite / Astro project (or any project that can output JSON)

---

## Getting Started

> **Assumption**: You already have a project with components that you want to expose as a registry.  
> If you’re starting from scratch, clone the [registry template](https://github.com/shadcn-svelte/registry-template) – it’s pre‑configured.

### 1. Create `registry.json`

The `registry.json` file is required only when using the `shadcn‑svelte` CLI.  
If you use a custom build system that already produces valid JSON, you can skip this step.

```json
{
  "$schema": "https://shadcn-svelte.com/schema/registry.json",
  "name": "acme",
  "homepage": "https://acme.com",
  "items": [
    // ...
  ]
}
```

> **Note**: The file must conform to the registry schema specification.

---

## Adding a Registry Item

### 1. Create the Component

Place your component in the `registry/` directory (or any location you prefer).  
Example: a simple `<HelloWorld />` component.

```svelte
<!-- registry/hello-world/hello-world.svelte -->
<script lang="ts">
  import { Button } from "$lib/components/ui/button/index.js";
</script>

<Button>Hello World</Button>
```

> **Directory layout**  
> ```
> registry
> └── hello-world
>     └── hello-world.svelte
> ```

> **Tip**: If you store the component elsewhere, ensure Tailwind can detect it.  
> Add an import in `src/app.css`:

```css
/* src/app.css */
@source "./registry/@acmecorp/ui-lib";
```

### 2. Register the Component

Add a definition to `registry.json`:

```json
{
  "$schema": "https://shadcn-svelte.com/schema/registry.json",
  "name": "acme",
  "homepage": "https://acme.com",
  "items": [
    {
      "name": "hello-world",
      "type": "registry:block",
      "title": "Hello World",
      "description": "A simple hello world component.",
      "files": [
        {
          "path": "./src/lib/hello-world/hello-world.svelte",
          "type": "registry:component"
        }
      ]
    }
  ]
}
```

> **Fields**  
> * `name` – unique identifier  
> * `type` – e.g. `registry:block`  
> * `title` – human‑readable name  
> * `description` – short description  
> * `files` – array of file objects (`path` + `type`)

---

## Building the Registry

### 1. Install the CLI

```bash
# pnpm
pnpm i shadcn-svelte@latest

# npm
npm i shadcn-svelte@latest

# bun
bun i shadcn-svelte@latest

# yarn
yarn add shadcn-svelte@latest
```

### 2. Add a Build Script

```json
// package.json
{
  "scripts": {
    "registry:build": "pnpm shadcn-svelte registry build"
  }
}
```

### 3. Run the Build

```bash
# pnpm
pnpm run registry:build

# npm
npm run registry:build

# bun
bun run registry:build

# yarn
yarn run registry:build
```

> By default, the CLI outputs JSON files to `static/r/` (e.g. `static/r/hello-world.json`).  
> Override the output directory with `--output`:

```bash
pnpm shadcn-svelte registry build --output ./dist/registry
```

---

## Serving the Registry

Run your dev server:

```bash
# pnpm
pnpm run dev

# npm
npm run dev

# bun
bun run dev

# yarn
yarn run dev
```

Your registry items are now accessible at:

```
http://localhost:5173/r/[NAME].json
```

Example:

```
http://localhost:5173/r/hello-world.json
```

---

## Publishing the Registry

Deploy your project to a public URL (e.g. Vercel, Netlify, Fly.io).  
Once live, other developers can install your items via the CLI:

```bash
# pnpm
pnpm dlx shadcn-svelte@latest add https://your-domain.com/r/hello-world.json

# npm
npm dlx shadcn-svelte@latest add https://your-domain.com/r/hello-world.json

# bun
bun dlx shadcn-svelte@latest add https://your-domain.com/r/hello-world.json

# yarn
yarn dlx shadcn-svelte@latest add https://your-domain.com/r/hello-world.json
```

---

## Adding Authentication

The CLI does **not** provide built‑in auth.  
A common pattern is to use a token query parameter:

```
http://localhost:5173/r/hello-world.json?token=YOUR_SECURE_TOKEN
```

Your server should:

1. Validate the token.
2. Return `401 Unauthorized` if invalid.
3. The CLI will display an error message to the user.

> **Security tip**: Encrypt and rotate tokens regularly.

---

## Guidelines

When building components for a registry, follow these rules:

| Property | Required | Description |
|----------|----------|-------------|
| `name` | ✅ | Unique identifier |
| `description` | ✅ | Short description |
| `type` | ✅ | e.g. `registry:block` |
| `files` | ✅ | Array of file objects |
| `registryDependencies` | ✅ | List of dependencies (component names or URLs) |

**File placement**: Prefer `components/`, `hooks/`, or `lib/` directories inside the registry item.

---

## CLI Commands

| Command | Description |
|---------|-------------|
| `shadcn-svelte registry build` | Build registry JSON files |
| `shadcn-svelte add <url>` | Install a registry item from a URL |

---

## FAQ

- **Built by**: shadcn (original) – ported to Svelte by Huntabyte & CokaKoala.  
- **Legacy Docs**: Available for older versions.  
- **Migration**: See the migration guide for moving from older registries.  

---

Happy component building! 🚀