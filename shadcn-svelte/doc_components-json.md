# shadcn‑svelte – Project Configuration

This guide explains how to set up the **`components.json`** file that the shadcn‑svelte CLI uses to generate UI components tailored to your project.  
All examples from the original documentation are preserved.

---

## 1.  What is `components.json`?

`components.json` is an optional JSON file that tells the CLI:

* How Tailwind CSS is configured in your project  
* Where to place generated files (via path aliases)  
* Whether to use TypeScript  
* Which component registry to pull from

If you copy‑paste components manually, you can skip this file.

---

## 2.  Creating the file

Run one of the following commands in your project root:

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

The CLI will scaffold a minimal `components.json` for you.

---

## 3.  JSON Schema

```json
{
  "$schema": "https://shadcn-svelte.com/schema.json"
}
```

---

## 4.  Tailwind Configuration

### 4.1  `tailwind.css`

Specify the path to the CSS file that imports Tailwind into your project.

```json
{
  "tailwind": {
    "css": "src/app.{p,post}css"
  }
}
```

### 4.2  `tailwind.baseColor`

Choose the base color palette for the generated components.  
**This value cannot be changed after initialization.**

```json
{
  "tailwind": {
    "baseColor": "gray" | "neutral" | "slate" | "stone" | "zinc"
  }
}
```

---

## 5.  Path Aliases

The CLI uses these values together with the alias configuration in `svelte.config.js` to place generated files correctly.

| Alias | Description | Example |
|-------|-------------|---------|
| `lib` | Library root (components, utils, hooks, etc.) | `"$lib"` |
| `utils` | Utility functions | `"$lib/utils"` |
| `components` | General components | `"$lib/components"` |
| `ui` | UI components | `"$lib/components/ui"` |
| `hooks` | Svelte 5 reactive functions | `"$lib/hooks"` |

**Example**

```json
{
  "aliases": {
    "lib": "$lib",
    "utils": "$lib/utils",
    "components": "$lib/components",
    "ui": "$lib/components/ui",
    "hooks": "$lib/hooks"
  }
}
```

---

## 6.  TypeScript

Enable or disable TypeScript support:

```json
{
  "typescript": true | false
}
```

If you use a custom TypeScript configuration file, specify its path:

```json
{
  "typescript": {
    "config": "path/to/tsconfig.custom.json"
  }
}
```

---

## 7.  Component Registry

Tell the CLI where to fetch components from.  
You can pin to a specific preview release or use your own fork.

```json
{
  "registry": "https://shadcn-svelte.com/registry"
}
```

---

## 8.  Full Example

Below is a complete `components.json` that covers all options:

```json
{
  "$schema": "https://shadcn-svelte.com/schema.json",
  "tailwind": {
    "css": "src/app.{p,post}css",
    "baseColor": "gray"
  },
  "aliases": {
    "lib": "$lib",
    "utils": "$lib/utils",
    "components": "$lib/components",
    "ui": "$lib/components/ui",
    "hooks": "$lib/hooks"
  },
  "typescript": {
    "config": "tsconfig.custom.json"
  },
  "registry": "https://shadcn-svelte.com/registry"
}
```

---

## 9.  Next Steps

* **Install Tailwind CSS** – follow the [Tailwind installation guide](https://tailwindcss.com/docs/installation).  
* **Add components** – use the CLI (`pnpm dlx shadcn-svelte add <component>`) or copy‑paste.  
* **Theme & Dark Mode** – configure Tailwind’s dark mode and extend the palette as needed.  

---

### FAQ

* **Do I need `components.json`?**  
  Only if you use the CLI to add components. Manual copy‑paste works without it.

* **Can I change `baseColor` later?**  
  No. It is fixed after initialization. Re‑initialize if you need a different palette.

* **Where do I set path aliases?**  
  In `svelte.config.js` under the `kit.paths.alias` section.

---

**Happy coding!**