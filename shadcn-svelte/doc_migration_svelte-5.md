# Migrating shadcn‑svelte from **Svelte 4 / Tailwind 3** to **Svelte 5 / Tailwind 3**

This guide walks you through the changes required to upgrade a shadcn‑svelte project that was built with Svelte 4 and Tailwind 3 to the new Svelte 5 release.  
It focuses exclusively on the shadcn‑svelte side – the migration of the underlying **bits‑ui** library is covered in the Bits‑UI migration guide.

> **Prerequisite** – Read the official [Svelte 5 migration guide](https://svelte.dev/blog/svelte-5) before starting.

---

## 1. Prerequisites

| Item | What to do |
|------|------------|
| **Commit** | Commit any pending changes to your repo. |
| **Identify custom code** | Note which components have custom behavior or styles so you can re‑implement them after the upgrade. |
| **Install `sv-migrate`** | `pnpm i -D sv-migrate` (or `npm i -D sv-migrate`). This tool helps you run the migration commands. |

---

## 2. Update Project Configuration

### 2.1 `components.json`

Add the registry URL and new alias keys:

```json
{
  "$schema": "https://shadcn-svelte.com/schema.json",
  "style": "default",
  "tailwind": {
    "css": "src/app.css",
    "baseColor": "slate"
  },
  "aliases": {
    "components": "$lib/components",
    "utils": "$lib/utils",
    "ui": "$lib/components/ui",
    "hooks": "$lib/hooks",
    "lib": "$lib"
  },
  "typescript": true,
  "registry": "https://shadcn-svelte.com/registry"
}
```

### 2.2 `tailwind.config.js`

Add the `tailwindcss-animate` plugin, sidebar colors, and keyframes/animations used by the new components.

```js
import type { Config } from "tailwindcss";
import tailwindcssAnimate from "tailwindcss-animate";

const config: Config = {
  darkMode: ["class"],
  content: ["./src/**/*.{html,js,svelte,ts}"],
  safelist: ["dark"],
  theme: {
    container: {
      // unchanged ...
    },
    extend: {
      colors: {
        // unchanged ...
        sidebar: {
          DEFAULT: "hsl(var(--sidebar-background))",
          foreground: "hsl(var(--sidebar-foreground))",
          primary: "hsl(var(--sidebar-primary))",
          "primary-foreground": "hsl(var(--sidebar-primary-foreground))",
          accent: "hsl(var(--sidebar-accent))",
          "accent-foreground": "hsl(var(--sidebar-accent-foreground))",
          border: "hsl(var(--sidebar-border))",
          ring: "hsl(var(--sidebar-ring))",
        },
      },
      borderRadius: {
        // unchanged ...
      },
      fontFamily: {
        // unchanged ...
      },
      keyframes: {
        "accordion-down": {
          from: { height: "0" },
          to: { height: "var(--bits-accordion-content-height)" },
        },
        "accordion-up": {
          from: { height: "var(--bits-accordion-content-height)" },
          to: { height: "0" },
        },
        "caret-blink": {
          "0%,70%,100%": { opacity: "1" },
          "20%,50%": { opacity: "0" },
        },
      },
      animation: {
        "accordion-down": "accordion-down 0.2s ease-out",
        "accordion-up": "accordion-up 0.2s ease-out",
        "caret-blink": "caret-blink 1.25s ease-out infinite",
      },
    },
  },
  plugins: [tailwindcssAnimate],
};

export default config;
```

> **Install the plugin**  
> ```bash
> pnpm i tailwindcss-animate
> ```

### 2.3 `src/lib/utils.ts`

The new `utils.ts` only exports the `cn` helper and a few type utilities. Remove any references to the removed `flyAndScale` function.

```ts
import { type ClassValue, clsx } from "clsx";
import { twMerge } from "tailwind-merge";

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}

// eslint-disable-next-line @typescript-eslint/no-explicit-any
export type WithoutChild<T> = T extends { child?: any } ? Omit<T, "child"> : T;
// eslint-disable-next-line @typescript-eslint/no-explicit-any
export type WithoutChildren<T> = T extends { children?: any }
  ? Omit<T, "children">
  : T;
export type WithoutChildrenOrChild<T> = WithoutChildren<WithoutChild<T>>;
export type WithElementRef<T, U extends HTMLElement = HTMLElement> = T & {
  ref?: U | null;
};
```

> **Tip** – If you need the old `flyAndScale` helper, re‑implement it yourself or postpone this step until after you’ve updated all components.

---

## 3. Upgrade Dependencies

Run the following command to bring the latest Svelte 5‑compatible versions into your project:

```bash
pnpm i bits-ui@latest svelte-sonner@latest @lucide/svelte@latest paneforge@next vaul-svelte@next mode-watcher@latest -D
```

| Package | New version | Notes |
|---------|-------------|-------|
| `bits-ui` | `^1.0.0` | Core UI primitives |
| `svelte-sonner` | `^1.0.0` | Toast notifications |
| `@lucide/svelte` | `^0.482.0` | Icon set |
| `paneforge` | `^1.0.0-next.5` | Utility library |
| `vaul-svelte` | `^1.0.0-next.7` | Modal & dialog helpers |
| `mode-watcher` | `^1.0.0` | Dark‑mode helper |

> **Deprecated packages** – The following packages are no longer needed and should be removed after migration:
> * `cmdk-sv` → Bits‑UI Command
> * `svelte-headless-table` → `@tanstack/table-core`
> * `svelte-radix` → `@lucide/svelte`
> * `lucide-svelte` → `@lucide/svelte`

---

## 4. (Optional) Alias Old Dependencies

If you want to keep both the old and new versions of a dependency while you migrate, alias the old one in `package.json`:

```json
{
  "devDependencies": {
    "bits-ui-old": "npm:bits-ui@0.22.0"
  }
}
```

Then replace imports in your code:

```svelte
<script lang="ts">
  // Old import
  // import { Dialog as DialogPrimitive } from "bits-ui";

  // New import (alias)
  import { Dialog as DialogPrimitive } from "bits-ui-old";
</script>
```

Repeat for any other dependencies you wish to alias.

---

## 5. Migrate Components

### 5.1 Commit Your Current State

```bash
git add .
git commit -m "before migration"
```

### 5.2 Run the Migration CLI

```bash
pnpm dlx shadcn-svelte@latest add dialog --overwrite
```

> Replace `dialog` with the component you want to migrate.  
> The CLI will overwrite the existing component with the new Svelte 5‑compatible version.

### 5.3 Review and Adjust

After each migration run, review the diff:

```bash
git diff
```

Make any necessary adjustments (e.g., re‑implement custom styles or behavior).

Repeat for every component you wish to upgrade.

---

## 6. Clean Up Unused Dependencies

Once all components are migrated, remove the old packages:

```bash
pnpm remove cmdk-sv
pnpm remove svelte-headless-table
pnpm remove svelte-radix
pnpm remove lucide-svelte
```

---

## 7. Next Steps

* Verify that the application builds and runs correctly.  
* Run your test suite and manually test critical UI flows.  
* If you’re also migrating to Tailwind 4, follow the Tailwind 4 migration guide next.

---

## 8. Summary

| Step | What you did |
|------|--------------|
| Updated `components.json` | Added registry and new alias keys |
| Updated `tailwind.config.js` | Added animate plugin, sidebar colors, keyframes |
| Updated `utils.ts` | Simplified to `cn` helper |
| Upgraded dependencies | Switched to Svelte 5‑compatible versions |
| (Optional) Aliased old deps | Allowed coexistence during migration |
| Migrated components | Replaced old components with new ones |
| Cleaned up | Removed deprecated packages |

You’re now ready to enjoy the new features and improvements that come with Svelte 5 and the updated shadcn‑svelte components. Happy coding!