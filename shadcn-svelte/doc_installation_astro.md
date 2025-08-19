# shadcn‑svelte Documentation

> **shadcn‑svelte** – A collection of reusable, accessible UI components for Svelte, inspired by shadcn‑ui.  
> Built with Tailwind CSS and fully type‑safe (TypeScript).

---

## Table of Contents

1. [Getting Started](#getting-started)
   - [Create a Project](#create-a-project)
   - [Configure Astro](#configure-astro-project)
   - [Add Svelte](#add-svelte)
   - [Add Tailwind CSS](#add-tailwindcss)
   - [Path Aliases](#setup-path-aliases)
   - [Global CSS](#create-global-css)
   - [Import Global CSS](#import-global-css)
2. [shadcn‑svelte CLI](#shadcn-svelte-cli)
   - [Initialize](#initialize)
   - [Add Components](#add-components)
3. [Astro Tailwind Configuration](#astro-tailwind-configuration)
4. [Using Components](#using-components)
5. [Interactive Components in Astro](#interactive-components-in-astro)
6. [Additional Resources](#additional-resources)

---

## 1. Getting Started

### Create a Project

```bash
# Using pnpm
pnpm create astro@latest

# Or using npm
npm init astro@latest
```

Follow the prompts:

| Question | Example Answer |
|----------|----------------|
| Where should we create your new project? | `./your-app-name` |
| How would you like to start your new project? | Choose a starter template (or Empty) |
| Do you plan to write TypeScript? | Yes |
| How strict should TypeScript be? | Strict |
| Install dependencies? | Yes |
| Initialize a new git repository? | Yes / No |

### Configure Astro Project

After the project is created, navigate into it:

```bash
cd your-app-name
```

### Add Svelte

```bash
# Using pnpm
pnpm dlx astro add svelte

# Or using npm
npm i -D @astrojs/svelte
```

Answer **Yes** to all prompts.

### Add Tailwind CSS

```bash
# Using pnpm
pnpm dlx astro add tailwind

# Or using npm
npm i -D tailwindcss postcss autoprefixer
```

Answer **Yes** to all prompts.

### Setup Path Aliases

Add the following to `tsconfig.json` to resolve imports:

```json
{
  "compilerOptions": {
    // ...
    "baseUrl": ".",
    "paths": {
      "$lib": ["./src/lib"],
      "$lib/*": ["./src/lib/*"]
    }
    // ...
  }
}
```

Adjust the paths if your project structure differs.

### Create a Global CSS File

Create `src/styles/app.css`:

```css
/* src/styles/app.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### Import the Global CSS

In `src/pages/index.astro`:

```astro
---
import "$lib/styles/app.css";
---
<html lang="en">
  <head>
    <title>Astro</title>
  </head>
  <body>
    <!-- Your content goes here -->
  </body>
</html>
```

---

## 2. shadcn‑svelte CLI

### Initialize

Run the CLI to scaffold the component configuration:

```bash
# Using pnpm
pnpm dlx shadcn-svelte@latest init

# Or using npm
npm i -D shadcn-svelte
npx shadcn-svelte@latest init
```

You’ll be prompted to answer:

| Prompt | Example Answer |
|--------|----------------|
| Which base color would you like to use? | Slate |
| Where is your global CSS file? (this file will be overwritten) | `src/app.css` |
| Configure the import alias for lib | `$lib` |
| Configure the import alias for components | `$lib/components` |
| Configure the import alias for utils | `$lib/utils` |
| Configure the import alias for hooks | `$lib/hooks` |
| Configure the import alias for ui | `$lib/components/ui` |

### Add Components

To add a component, use:

```bash
# Example: Add Button
pnpm dlx shadcn-svelte@latest add button

# Or using npm
npx shadcn-svelte@latest add button
```

The component files will be created under `$lib/components/ui/button`.

---

## 3. Astro Tailwind Configuration

To avoid duplicate base styles, disable Tailwind’s base styles in Astro:

```js
// astro.config.mjs
import { defineConfig } from 'astro/config';
import tailwind from '@astrojs/tailwind';

export default defineConfig({
  integrations: [
    tailwind({
      applyBaseStyles: false,
    }),
    // ...other integrations
  ],
});
```

---

## 4. Using Components

Import the component in an `.astro` file:

```astro
---
import { Button } from "$lib/components/ui/button/index.js";
---
<html lang="en">
  <head>
    <title>Astro</title>
  </head>
  <body>
    <Button>Hello World</Button>
  </body>
</html>
```

> **Tip:** Use the `client:load` directive for interactive components in Astro (see next section).

---

## 5. Interactive Components in Astro

Astro renders static HTML by default. For components that rely on client‑side interactivity (e.g., modals, dropdowns), add a client directive:

```astro
---
import { Dialog } from "$lib/components/ui/dialog/index.js";
---
<div client:load>
  <Dialog>
    <!-- dialog content -->
  </Dialog>
</div>
```

Refer to the [Astro docs](https://docs.astro.build/en/guides/client-side-rendering/) for more details.

---

## 6. Additional Resources

- **Component Registry** – Browse all available components: `registry.json`
- **CLI Help** – `npx shadcn-svelte@latest --help`
- **Tailwind Docs** – https://tailwindcss.com/docs
- **Astro Docs** – https://docs.astro.build

---

### Example: Adding a Button Component

```bash
pnpm dlx shadcn-svelte@latest add button
```

```astro
---
import { Button } from "$lib/components/ui/button/index.js";
---
<Button class="bg-primary text-white">Click Me</Button>
```

---

### Example: Adding a Dialog Component

```bash
pnpm dlx shadcn-svelte@latest add dialog
```

```astro
---
import { Dialog, DialogTrigger, DialogContent } from "$lib/components/ui/dialog/index.js";
---
<div client:load>
  <Dialog>
    <DialogTrigger>Open Dialog</DialogTrigger>
    <DialogContent>
      <p>This is a dialog!</p>
    </DialogContent>
  </Dialog>
</div>
```

---

**Happy coding!**