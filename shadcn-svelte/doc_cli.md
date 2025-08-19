# shadcn‑svelte Documentation

> **shadcn‑svelte** – A collection of reusable, accessible UI components for Svelte, inspired by the shadcn‑ui design system.

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [Installation](#installation)
3. [CLI Commands](#cli-commands)
   - [init](#init)
   - [add](#add)
   - [registry build](#registry-build)
4. [Configuration Options](#configuration-options)
5. [Component Registry](#component-registry)
6. [Examples](#examples)
7. [Advanced Topics](#advanced-topics)
8. [FAQ](#faq)

---

## 1. Getting Started

shadcn‑svelte provides a set of ready‑to‑use UI components that can be dropped into any Svelte project. The components are built with Tailwind CSS and are fully accessible.

---

## 2. Installation

You can install shadcn‑svelte via your preferred package manager. The CLI will handle the rest.

```bash
# Using pnpm
pnpm dlx shadcn-svelte@latest init

# Using npm
npm dlx shadcn-svelte@latest init

# Using bun
bun dlx shadcn-svelte@latest init

# Using yarn
yarn dlx shadcn-svelte@latest init
```

---

## 3. CLI Commands

The shadcn‑svelte CLI provides three main commands:

| Command | Purpose |
|---------|---------|
| `init`  | Initialise a new project, install dependencies, and set up configuration. |
| `add`   | Add one or more components to an existing project. |
| `registry build` | Generate registry JSON files from a `registry.json` definition. |

### 3.1 `init`

Initialises a new project, installs dependencies, adds the `cn` utility, and creates CSS variables.

```bash
pnpm dlx shadcn-svelte@latest init
```

You will be prompted to answer a few configuration questions:

```
Which base color would you like to use? › Slate
Where is your global CSS file? (this file will be overwritten) › src/app.css
Configure the import alias for lib: › $lib
Configure the import alias for components: › $lib/components
Configure the import alias for utils: › $lib/utils
Configure the import alias for hooks: › $lib/hooks
Configure the import alias for ui: › $lib/components/ui
```

#### Usage

```bash
shadcn-svelte init [options]
```

#### Options

| Flag | Description |
|------|-------------|
| `-c, --cwd <path>` | Working directory (default: current directory) |
| `-o, --overwrite` | Overwrite existing files (default: false) |
| `--no-deps` | Disable adding & installing dependencies |
| `--base-color <name>` | Base color for components (`slate`, `gray`, `zinc`, `neutral`, `stone`) |
| `--css <path>` | Path to the global CSS file |
| `--components-alias <path>` | Import alias for components |
| `--lib-alias <path>` | Import alias for lib |
| `--utils-alias <path>` | Import alias for utils |
| `--hooks-alias <path>` | Import alias for hooks |
| `--ui-alias <path>` | Import alias for ui |
| `--proxy <proxy>` | Fetch items from registry using a proxy |
| `-h, --help` | Display help |

---

### 3.2 `add`

Adds components and their dependencies to an existing project.

```bash
pnpm dlx shadcn-svelte@latest add [component]
```

You will be presented with a list of components to choose from:

```
Which components would you like to add? › Space to select. Return to submit.

◯  accordion
◯  alert
◯  alert-dialog
◯  aspect-ratio
◯  avatar
◯  badge
◯  button
◯  card
◯  checkbox
◯  collapsible
```

#### Usage

```bash
shadcn-svelte add [options] [components...]
```

#### Options

| Flag | Description |
|------|-------------|
| `-c, --cwd <path>` | Working directory (default: current directory) |
| `--no-deps` | Skip adding & installing package dependencies |
| `-a, --all` | Install all components to your project (default: false) |
| `-y, --yes` | Skip confirmation prompt (default: false) |
| `-o, --overwrite` | Overwrite existing files (default: false) |
| `--proxy <proxy>` | Fetch components from registry using a proxy |
| `-h, --help` | Display help |

---

### 3.3 `registry build`

Generates the registry JSON files from a `registry.json` definition.

```bash
pnpm dlx shadcn-svelte@latest registry build [registry.json]
```

#### Usage

```bash
shadcn-svelte registry build [options] [registry]
```

#### Options

| Flag | Description |
|------|-------------|
| `registry` | Path to `registry.json` file (default: `./registry.json`) |
| `-c, --cwd <path>` | Working directory (default: current directory) |
| `-o, --output <path>` | Destination directory for JSON files (default: `./static/r`) |
| `-h, --help` | Display help |

---

## 4. Configuration Options

All CLI options can also be set via environment variables or a `.env` file. For example, to set a proxy:

```bash
HTTP_PROXY="http://proxy.example.com:8080" npx shadcn-svelte@latest init
```

---

## 5. Component Registry

The registry defines all available components and their metadata. It is used by the CLI to fetch component files.

- `registry.json` – The source definition file.
- `static/r` – The generated JSON files used by the CLI.

---

## 6. Examples

Below are the exact examples from the original documentation, preserved for reference.

### 6.1 Initialising a Project

```bash
pnpm dlx shadcn-svelte@latest init
```

### 6.2 Adding Components

```bash
pnpm dlx shadcn-svelte@latest add [component]
```

### 6.3 Building the Registry

```bash
pnpm dlx shadcn-svelte@latest registry build [registry.json]
```

### 6.4 Using a Proxy

```bash
HTTP_PROXY="<proxy-url>" npx shadcn-svelte@latest init
```

---

## 7. Advanced Topics

### 7.1 Dark Mode

shadcn‑svelte supports dark mode out of the box. Add the following to your global CSS:

```css
/* src/app.css */
@tailwind base;
@tailwind components;
@tailwind utilities;

:root {
  --color-slate-50: 255 255 255;
  /* ... other color variables ... */
}
```

### 7.2 Custom Themes

You can override the base color by passing `--base-color` during `init` or by editing the generated CSS variables.

---

## 8. FAQ

**Q: Can I use shadcn‑svelte with SvelteKit?**  
A: Yes. The CLI supports SvelteKit projects out of the box.

**Q: How do I add a component that isn’t listed?**  
A: You can provide a URL to the component file in the `add` command.

**Q: Where can I find the source code for each component?**  
A: The registry JSON files in `static/r` contain the component definitions.

---

*Built by shadcn. Ported to Svelte by Huntabyte & CokaKoala.*