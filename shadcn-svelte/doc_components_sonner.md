# Sonner – Toast Notifications for Svelte

> **Sonner** is a lightweight, opinionated toast component for Svelte.  
> It is a Svelte port of the original [Sonner](https://github.com/emilsk/sonner) library created by Emil Kowalski for React.

---

## Table of Contents

- [Overview](#overview)
- [Installation](#installation)
  - [CLI](#cli)
  - [Manual](#manual)
- [Dark‑Mode Support](#dark-mode-support)
- [Adding the `<Toaster>` Component](#adding-the-toaster-component)
- [Usage](#usage)
- [Examples](#examples)
- [License](#license)

---

## Overview

Sonner provides a simple API for showing toast notifications in Svelte applications.  
It automatically respects the user’s system color scheme (light/dark) but can be overridden with a custom theme or a hard‑coded mode.

---

## Installation

### CLI

```bash
# Using pnpm
pnpm dlx shadcn-svelte@latest add sonner

# Using npm
npm dlx shadcn-svelte@latest add sonner

# Using bun
bun dlx shadcn-svelte@latest add sonner

# Using yarn
yarn dlx shadcn-svelte@latest add sonner
```

### Manual

If you prefer to install manually, add the following to your project:

```bash
# Using pnpm
pnpm add svelte-sonner

# Using npm
npm install svelte-sonner

# Using bun
bun add svelte-sonner

# Using yarn
yarn add svelte-sonner
```

Then import the component and styles in your root layout:

```svelte
<script lang="ts">
  import { Toaster } from "$lib/components/ui/sonner/index.js";
  let { children } = $props();
</script>

<Toaster />

{@render children?.()}
```

---

## Dark‑Mode Support

By default, Sonner reads the user’s system preference to decide between light and dark themes.  
If you want to force a specific theme or disable dark‑mode handling:

1. **Pass a custom theme** to the `<Toaster>` component:

   ```svelte
   <Toaster theme="dark" />
   ```

2. **Use `mode-watcher`** to hard‑code a mode:

   ```bash
   pnpm add mode-watcher
   ```

   Then import and use it in your layout:

   ```svelte
   <script lang="ts">
     import { Toaster } from "$lib/components/ui/sonner/index.js";
     import { modeWatcher } from "mode-watcher";
   </script>

   <Toaster theme={modeWatcher()} />
   ```

3. **Opt out of dark‑mode support** by uninstalling `mode-watcher` and removing the `theme` prop.

---

## Adding the `<Toaster>` Component

Place the `<Toaster>` component once in your application (typically in the root layout).  
It will automatically render toast notifications triggered from anywhere in your app.

```svelte
<script lang="ts">
  import { Toaster } from "$lib/components/ui/sonner/index.js";
  let { children } = $props();
</script>

<Toaster />

{@render children?.()}
```

---

## Usage

Import the `toast` function from `svelte-sonner` and call it whenever you need to show a notification.

```svelte
<script lang="ts">
  import { toast } from "svelte-sonner";
  import { Button } from "$lib/components/ui/button/index.js";
</script>

<Button onclick={() => toast("Hello world")}>Show toast</Button>
```

### Advanced Options

You can pass an options object to customize the toast:

```svelte
<script lang="ts">
  import { toast } from "svelte-sonner";
  import { Button } from "$lib/components/ui/button/index.js";
</script>

<Button
  variant="outline"
  onclick={() =>
    toast.success("Event has been created", {
      description: "Sunday, December 03, 2023 at 9:00 AM",
      action: {
        label: "Undo",
        onClick: () => console.info("Undo")
      }
    })
  }
>
  Show Toast
</Button>
```

---

## Examples

### Basic Toast

```svelte
<script lang="ts">
  import { toast } from "svelte-sonner";
  import { Button } from "$lib/components/ui/button/index.js";
</script>

<Button onclick={() => toast("Hello world")}>Show toast</Button>
```

### Success Toast with Description and Action

```svelte
<script lang="ts">
  import { toast } from "svelte-sonner";
  import { Button } from "$lib/components/ui/button/index.js";
</script>

<Button
  variant="outline"
  onclick={() =>
    toast.success("Event has been created", {
      description: "Sunday, December 03, 2023 at 9:00 AM",
      action: {
        label: "Undo",
        onClick: () => console.info("Undo")
      }
    })
  }
>
  Show Toast
</Button>
```

---

## License

Sonner is open source and released under the MIT license.  
See the original repository for details: https://github.com/emilsk/sonner

---