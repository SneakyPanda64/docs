# shadcn‑svelte Registry Documentation

This guide explains how to create and use **registry items** in the shadcn‑svelte ecosystem.  
Registry items are JSON files that describe components, styles, themes, blocks, and more.  
They can be added to a project with the `shadcn-svelte` CLI (`npx shadcn-svelte@latest add`).

> **Tip** – All examples below are valid JSON and can be copied directly into your own registry files.

---

## 1. Registry Item Schema

```json
{
  "$schema": "https://shadcn-svelte.com/schema/registry-item.json",
  "name": "<unique-name>",
  "type": "<registry:item-type>",
  ...
}
```

| Field | Description |
|-------|-------------|
| `$schema` | URL to the JSON schema (always the same). |
| `name` | Unique identifier for the item. |
| `type` | One of: `registry:style`, `registry:theme`, `registry:block`, `registry:component`. |
| `extends` | Optional. Set to `"none"` to create a brand‑new style. |
| `dependencies` | Array of npm package names that should be installed. |
| `registryDependencies` | Array of other registry items (by name or URL) that this item pulls in. |
| `cssVars` | Custom CSS variables (see below). |
| `css` | Custom CSS rules (see below). |
| `files` | For blocks: array of file descriptors. |

---

## 2. Registry Item Types

| Type | Purpose | Typical Fields |
|------|---------|----------------|
| `registry:style` | Defines a set of CSS variables and optional dependencies. | `dependencies`, `registryDependencies`, `cssVars`, `css` |
| `registry:theme` | Extends the global theme with new variables. | `cssVars` |
| `registry:block` | A reusable page or feature (e.g., a login form). | `registryDependencies`, `files` |
| `registry:component` | A single Svelte component with optional CSS. | `css`, `cssVars` |

---

## 3. CSS Variables

### 3.1 Theme Variables

```json
"cssVars": {
  "theme": {
    "font-sans": "Inter, sans-serif",
    "shadow-card": "0 0 0 1px rgba(0, 0, 0, 0.1)"
  }
}
```

### 3.2 Light / Dark Mode Variables

```json
"cssVars": {
  "light": {
    "brand": "oklch(0.99 0.00 0)"
  },
  "dark": {
    "brand": "oklch(0.14 0.00 286)"
  }
}
```

### 3.3 Tailwind CSS Variable Overrides

```json
"cssVars": {
  "theme": {
    "spacing": "0.2rem",
    "breakpoint-sm": "640px",
    "breakpoint-md": "768px",
    "breakpoint-lg": "1024px",
    "breakpoint-xl": "1280px",
    "breakpoint-2xl": "1536px"
  }
}
```

---

## 4. Custom CSS

### 4.1 Base Layer

```json
"css": {
  "@layer base": {
    "h1": {
      "font-size": "var(--text-2xl)"
    },
    "h2": {
      "font-size": "var(--text-xl)"
    }
  }
}
```

### 4.2 Component Layer

```json
"css": {
  "@layer components": {
    "card": {
      "background-color": "var(--color-white)",
      "border-radius": "var(--rounded-lg)",
      "padding": "var(--spacing-6)",
      "box-shadow": "var(--shadow-xl)"
    }
  }
}
```

### 4.3 Utilities

| Example | Description |
|---------|-------------|
| **Simple Utility** | `@utility content-auto` |
| **Complex Utility** | `@utility scrollbar-hidden` |
| **Functional Utility** | `@utility tab-*` |

```json
"css": {
  "@utility content-auto": {
    "content-visibility": "auto"
  }
}
```

```json
"css": {
  "@utility scrollbar-hidden": {
    "scrollbar-hidden": {
      "&::-webkit-scrollbar": {
        "display": "none"
      }
    }
  }
}
```

```json
"css": {
  "@utility tab-*": {
    "tab-size": "var(--tab-size-*)"
  }
}
```

### 4.4 Animations

> **Note** – Define both `@keyframes` and a theme variable to use the animation.

```json
"cssVars": {
  "theme": {
    "--animate-wiggle": "wiggle 1s ease-in-out infinite"
  }
},
"css": {
  "@keyframes wiggle": {
    "0%, 100%": {
      "transform": "rotate(-3deg)"
    },
    "50%": {
      "transform": "rotate(3deg)"
    }
  }
}
```

---

## 5. Example Registry Items

### 5.1 Custom Style that Extends shadcn‑svelte

```json
{
  "$schema": "https://shadcn-svelte.com/schema/registry-item.json",
  "name": "example-style",
  "type": "registry:style",
  "dependencies": ["phosphor-svelte"],
  "registryDependencies": [
    "login-01",
    "calendar",
    "https://example.com/r/editor.json"
  ],
  "cssVars": {
    "theme": {
      "font-sans": "Inter, sans-serif"
    },
    "light": {
      "brand": "oklch(0.145 0 0)"
    },
    "dark": {
      "brand": "oklch(0.145 0 0)"
    }
  }
}
```

### 5.2 Custom Style from Scratch

```json
{
  "$schema": "https://shadcn-svelte.com/schema/registry-item.json",
  "extends": "none",
  "name": "new-style",
  "type": "registry:style",
  "dependencies": ["tailwind-merge", "clsx"],
  "registryDependencies": [
    "utils",
    "https://example.com/r/button.json",
    "https://example.com/r/input.json",
    "https://example.com/r/label.json",
    "https://example.com/r/select.json"
  ],
  "cssVars": {
    "theme": {
      "font-sans": "Inter, sans-serif"
    },
    "light": {
      "main": "#88aaee",
      "bg": "#dfe5f2",
      "border": "#000",
      "text": "#000",
      "ring": "#000"
    },
    "dark": {
      "main": "#88aaee",
      "bg": "#272933",
      "border": "#000",
      "text": "#e6e6e6",
      "ring": "#fff"
    }
  }
}
```

### 5.3 Custom Theme

```json
{
  "$schema": "https://shadcn-svelte.com/schema/registry-item.json",
  "name": "custom-theme",
  "type": "registry:theme",
  "cssVars": {
    "light": {
      "background": "oklch(1 0 0)",
      "foreground": "oklch(0.141 0.005 285.823)",
      "primary": "oklch(0.546 0.245 262.881)",
      "primary-foreground": "oklch(0.97 0.014 254.604)",
      "ring": "oklch(0.746 0.16 232.661)",
      "sidebar-primary": "oklch(0.546 0.245 262.881)",
      "sidebar-primary-foreground": "oklch(0.97 0.014 254.604)",
      "sidebar-ring": "oklch(0.746 0.16 232.661)"
    },
    "dark": {
      "background": "oklch(1 0 0)",
      "foreground": "oklch(0.141 0.005 285.823)",
      "primary": "oklch(0.707 0.165 254.624)",
      "primary-foreground": "oklch(0.97 0.014 254.604)",
      "ring": "oklch(0.707 0.165 254.624)",
      "sidebar-primary": "oklch(0.707 0.165 254.624)",
      "sidebar-primary-foreground": "oklch(0.97 0.014 254.604)",
      "sidebar-ring": "oklch(0.707 0.165 254.624)"
    }
  }
}
```

### 5.4 Custom Colors

```json
{
  "$schema": "https://shadcn-svelte.com/schema/registry-item.json",
  "name": "custom-style",
  "type": "registry:style",
  "cssVars": {
    "light": {
      "brand": "oklch(0.99 0.00 0)"
    },
    "dark": {
      "brand": "oklch(0.14 0.00 286)"
    }
  }
}
```

### 5.5 Custom Block

```json
{
  "$schema": "https://shadcn-svelte.com/schema/registry-item.json",
  "name": "login-01",
  "type": "registry:block",
  "description": "A simple login form.",
  "registryDependencies": ["button", "card", "input", "label"],
  "files": [
    {
      "path": "blocks/login-01/page.svelte",
      "content": "import { LoginForm } ...",
      "type": "registry:page",
      "target": "src/routes/login/+page.svelte"
    },
    {
      "path": "blocks/login-01/components/login-form.svelte",
      "content": "...",
      "type": "registry:component"
    }
  ]
}
```

### 5.6 Install a Block and Override Primitives

```json
{
  "$schema": "https://shadcn-svelte.com/schema/registry-item.json",
  "name": "custom-login",
  "type": "registry:block",
  "registryDependencies": [
    "login-01",
    "https://example.com/r/button.json",
    "https://example.com/r/input.json",
    "https://example.com/r/label.json"
  ]
}
```

---

## 6. Adding a Registry Item

```bash
# Add a registry item from a local file
npx shadcn-svelte@latest add path/to/your-item.json

# Add a registry item from a remote URL
npx shadcn-svelte@latest add https://example.com/r/your-item.json
```

The CLI will:

1. Install any listed npm dependencies.
2. Pull in any referenced registry dependencies.
3. Merge CSS variables and styles into your project.

---

## 7. FAQ

- **What is `extends: "none"`?**  
  It tells the CLI that this style is a brand‑new style and should not inherit from the default shadcn‑svelte style.

- **Can I override Tailwind CSS variables?**  
  Yes – use the `theme` section of `cssVars` to set values like `spacing`, `breakpoint-sm`, etc.

- **How do I add custom animations?**  
  Define a `@keyframes` block in `css` and expose it via a theme variable in `cssVars`.

---

## 8. Registry File Structure

| File | Purpose |
|------|---------|
| `registry.json` | Root registry file that lists all items. |
| `registry-item.json` | Individual item definition (examples above). |
| `registry-item.json` | Example of a component, block, style, or theme. |

---

## 9. Summary

- **Registry items** are JSON descriptors that let you modularly add styles, themes, blocks, and components.
- Use **`cssVars`** to define theme, light, and dark variables.
- Use **`css`** to inject custom CSS layers, utilities, or animations.
- The CLI handles dependency installation and file placement automatically.

Happy building with shadcn‑svelte!