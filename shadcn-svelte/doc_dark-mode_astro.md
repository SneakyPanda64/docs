# shadcn‑svelte – Dark‑Mode Integration Guide

This guide walks you through adding dark‑mode support to an Astro site that uses **shadcn‑svelte** components.  
It covers:

1. **Installation** of the required packages  
2. **Inline theme script** – a lightweight solution that stores the user’s preference in `localStorage` and prevents a flash of unstyled content (FOUC).  
3. **`mode-watcher`** – a small helper component that keeps the theme in sync with the DOM.  
4. **Mode toggle** – a reusable Svelte component that lets users switch between light and dark themes.  

All code snippets are ready to copy‑paste into your project.

---

## 1. Installation

```bash
# Using pnpm
pnpm i mode-watcher@0.5.1

# Using npm
npm i mode-watcher@0.5.1

# Using bun
bun i mode-watcher@0.5.1

# Using yarn
yarn add mode-watcher@0.5.1
```

> **Tip** – `mode-watcher` is a tiny dependency (~1 kB) that watches the `class` attribute on `<html>` and syncs it with `localStorage`.

---

## 2. Inline Theme Script

Add a script that:

- Detects the user’s preferred color scheme (`prefers-color-scheme` media query).  
- Persists the choice in `localStorage`.  
- Applies the `dark` class to `<html>` immediately to avoid FOUC.  
- Observes changes to the `class` attribute and updates `localStorage` accordingly.

```astro
---
// src/pages/index.astro
import "$lib/styles/app.css";
---
<script is:inline>
  const isBrowser = typeof localStorage !== 'undefined';

  const getThemePreference = () => {
    if (isBrowser && localStorage.getItem('theme')) {
      return localStorage.getItem('theme');
    }
    return window.matchMedia('(prefers-color-scheme: dark)').matches
      ? 'dark' : 'light';
  };

  const isDark = getThemePreference() === 'dark';
  document.documentElement.classList[isDark ? 'add' : 'remove']('dark');

  if (isBrowser) {
    const observer = new MutationObserver(() => {
      const isDark = document.documentElement.classList.contains('dark');
      localStorage.setItem('theme', isDark ? 'dark' : 'light');
    });
    observer.observe(document.documentElement, {
      attributes: true,
      attributeFilter: ['class']
    });
  }
</script>

<html lang="en">
  <body>
    <h1>Astro</h1>
  </body>
</html>
```

> **Why this script?**  
> * It runs **before** the page renders, so the correct theme is applied immediately.  
> * It works in both server‑side and client‑side rendering contexts.  

---

## 3. Using `mode-watcher`

Instead of the inline script, you can use the `ModeWatcher` component.  
It does the same thing but is easier to maintain and can be reused across pages.

```astro
---
// src/pages/index.astro
import "$lib/styles/app.css";
import { ModeWatcher } from "mode-watcher";
---
<html lang="en">
  <body>
    <h1>Astro</h1>
    <ModeWatcher client:load />
  </body>
</html>
```

> **`client:load`** – ensures the component runs only on the client side, preventing SSR mismatches.

---

## 4. Creating a Mode Toggle

A simple Svelte component that toggles the `dark` class on `<html>`.

```svelte
<!-- src/lib/components/mode-toggle.svelte -->
<script>
  import { onMount } from 'svelte';

  let isDark = false;

  onMount(() => {
    isDark = document.documentElement.classList.contains('dark');
  });

  function toggle() {
    isDark = !isDark;
    document.documentElement.classList.toggle('dark', isDark);
    localStorage.setItem('theme', isDark ? 'dark' : 'light');
  }
</script>

<button on:click={toggle} aria-label="Toggle theme">
  {#if isDark}
    🌙
  {:else}
    ☀️
  {/if}
</button>
```

### Adding the Toggle to a Page

```astro
---
// src/pages/index.astro
import "$lib/styles/app.css";
import { ModeWatcher } from "mode-watcher";
import ModeToggle from "$lib/components/mode-toggle.svelte";
---
<html lang="en">
  <body>
    <h1>Astro</h1>
    <ModeWatcher client:load />
    <ModeToggle client:load />
  </body>
</html>
```

> **Result** – A button that switches between light and dark themes, persisting the choice across page loads.

---

## 5. Summary

| Step | What to do | Why |
|------|------------|-----|
| 1 | Install `mode-watcher` | Lightweight helper to sync theme state |
| 2 | Add inline script or `ModeWatcher` | Apply theme immediately, avoid FOUC |
| 3 | Create `ModeToggle` component | Let users switch themes |
| 4 | Add components to your page | Full dark‑mode support |

With these pieces in place, your Astro site will respect the user’s theme preference, provide a smooth toggle experience, and stay consistent with the shadcn‑svelte design system. Happy coding!