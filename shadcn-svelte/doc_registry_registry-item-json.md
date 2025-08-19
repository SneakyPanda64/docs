# shadcn‑svelte Registry Item Documentation

This document describes the JSON schema used to define a custom registry item in **shadcn‑svelte**.  
All examples from the original source are preserved.

---

## 1. Overview

A *registry item* is a reusable component, hook, page, or other asset that can be imported into a Svelte project via the shadcn‑svelte CLI.  
The item is described by a `registry-item.json` file that follows the schema located at:

```
https://shadcn-svelte.com/schema/registry-item.json
```

---

## 2. Schema Properties

| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `$schema` | string | URL of the JSON schema. | `"$schema": "https://shadcn-svelte.com/schema/registry-item.json"` |
| `name` | string | Unique identifier for the item. | `"name": "hello-world"` |
| `title` | string | Human‑readable title. | `"title": "Hello World"` |
| `description` | string | Detailed description. | `"description": "A simple hello world component."` |
| `type` | string | Item type. | `"type": "registry:block"` |
| `author` | string | Author contact. | `"author": "John Doe <john@doe.com>"` |
| `dependencies` | array of strings | NPM package dependencies. | `"dependencies": ["bits-ui", "zod", "@lucide/svelte", "name@1.0.2"]` |
| `registryDependencies` | array of strings | References to other registry items. | `"registryDependencies": ["button", "input", "select"]` |
| `files` | array of objects | File definitions. | See **Files** section below. |
| `cssVars` | object | CSS variable definitions. | See **CSS Variables** section below. |
| `css` | object | Custom CSS rules. | See **Custom CSS** section below. |
| `docs` | string | Custom documentation or installation notes. | `"docs": "Remember to add the FOO_BAR environment variable to your .env file."` |
| `categories` | array of strings | Organizational tags. | `"categories": ["sidebar", "dashboard"]` |
| `meta` | object | Arbitrary key/value metadata. | `"meta": { "foo": "bar" }` |

---

## 3. Supported `type` Values

| Value | Description |
|-------|-------------|
| `registry:block` | Complex component with multiple files. |
| `registry:component` | Simple single‑file component. |
| `registry:lib` | Library or utility. |
| `registry:hook` | Custom hook. |
| `registry:ui` | UI component or single‑file primitive. |
| `registry:page` | Page or file‑based route. |
| `registry:file` | Miscellaneous file (e.g., `.env`). |
| `registry:style` | Registry style (e.g., `new-york`). |
| `registry:theme` | Theme definition. |

---

## 4. `registryDependencies`

Defines other registry items that this item depends on.

### 4.1 Local References (CLI)

When building with the CLI, you can reference items in the same registry using a `local:` prefix. The CLI will convert it to a relative path.

```json
{
  "items": [
    {
      "name": "hello-world",
      "registryDependencies": ["local:stepper"]
    }
  ]
}
```

After CLI processing, the output `registry-item.json` will contain:

```json
{
  "registryDependencies": ["./stepper.json"]
}
```

### 4.2 Remote URLs

You can also reference an external registry item by providing a full URL.

```json
{
  "registryDependencies": ["https://example.com/r/hello-world.json"]
}
```

### 4.3 Relative Paths

If you are not using the CLI, you can reference another item with a relative path.

```json
{
  "registryDependencies": ["./stepper.json"]
}
```

---

## 5. `files`

Each file in a registry item is described by an object with the following properties:

| Property | Type | Description |
|----------|------|-------------|
| `path` | string | Path to the file within the registry. |
| `type` | string | File type (see **Supported `type` Values**). |
| `target` | string (optional) | Destination path in the consuming project. Required for `registry:page` and `registry:file`. |

### 5.1 Example

```json
{
  "files": [
    {
      "path": "registry/hello-world/page.svelte",
      "type": "registry:page",
      "target": "src/routes/hello/+page.svelte"
    },
    {
      "path": "registry/hello-world/hello-world.svelte",
      "type": "registry:component"
    },
    {
      "path": "registry/hello-world/use-hello-world.svelte.ts",
      "type": "registry:hook"
    },
    {
      "path": "registry/hello-world/.env",
      "type": "registry:file",
      "target": ".env"
    }
  ]
}
```

---

## 6. `cssVars`

Define CSS variables for different themes or modes.

```json
{
  "cssVars": {
    "theme": {
      "font-heading": "Poppins, sans-serif"
    },
    "light": {
      "brand": "20 14.3% 4.1%",
      "radius": "0.5rem"
    },
    "dark": {
      "brand": "20 14.3% 4.1%"
    }
  }
}
```

---

## 7. `css`

Add custom CSS rules to the project’s stylesheet. Keys represent CSS layers or utilities.

```json
{
  "css": {
    "@layer base": {
      "body": {
        "font-size": "var(--text-base)",
        "line-height": "1.5"
      }
    },
    "@layer components": {
      "button": {
        "background-color": "var(--color-primary)",
        "color": "var(--color-white)"
      }
    },
    "@utility text-magic": {
      "font-size": "var(--text-base)",
      "line-height": "1.5"
    },
    "@keyframes wiggle": {
      "0%, 100%": {
        "transform": "rotate(-3deg)"
      },
      "50%": {
        "transform": "rotate(3deg)"
      }
    }
  }
}
```

---

## 8. `docs`

Custom documentation or installation notes that appear when the item is installed via the CLI.

```json
{
  "docs": "Remember to add the FOO_BAR environment variable to your .env file."
}
```

---

## 9. `categories`

Organize the item within the registry UI.

```json
{
  "categories": ["sidebar", "dashboard"]
}
```

---

## 10. `meta`

Arbitrary key/value pairs for additional metadata.

```json
{
  "meta": { "foo": "bar" }
}
```

---

## 11. Full Example

Below is a complete `registry-item.json` that incorporates all the properties discussed:

```json
{
  "$schema": "https://shadcn-svelte.com/schema/registry-item.json",
  "name": "hello-world",
  "title": "Hello World",
  "description": "A simple hello world component.",
  "type": "registry:block",
  "author": "John Doe <john@doe.com>",
  "dependencies": ["bits-ui", "zod", "@lucide/svelte", "name@1.0.2"],
  "registryDependencies": ["button", "input", "select"],
  "files": [
    {
      "path": "registry/hello-world/page.svelte",
      "type": "registry:page",
      "target": "src/routes/hello/+page.svelte"
    },
    {
      "path": "registry/hello-world/hello-world.svelte",
      "type": "registry:component"
    },
    {
      "path": "registry/hello-world/use-hello-world.svelte.ts",
      "type": "registry:hook"
    },
    {
      "path": "registry/hello-world/.env",
      "type": "registry:file",
      "target": ".env"
    }
  ],
  "cssVars": {
    "theme": {
      "font-heading": "Poppins, sans-serif"
    },
    "light": {
      "brand": "20 14.3% 4.1%",
      "radius": "0.5rem"
    },
    "dark": {
      "brand": "20 14.3% 4.1%"
    }
  },
  "css": {
    "@layer base": {
      "body": {
        "font-size": "var(--text-base)",
        "line-height": "1.5"
      }
    },
    "@layer components": {
      "button": {
        "background-color": "var(--color-primary)",
        "color": "var(--color-white)"
      }
    },
    "@utility text-magic": {
      "font-size": "var(--text-base)",
      "line-height": "1.5"
    },
    "@keyframes wiggle": {
      "0%, 100%": {
        "transform": "rotate(-3deg)"
      },
      "50%": {
        "transform": "rotate(3deg)"
      }
    }
  },
  "docs": "Remember to add the FOO_BAR environment variable to your .env file.",
  "categories": ["sidebar", "dashboard"],
  "meta": { "foo": "bar" }
}
```

---

## 12. References

- **Schema URL**: https://shadcn-svelte.com/schema/registry-item.json  
- **CLI Documentation**: (link to shadcn‑svelte CLI guide)  
- **Registry UI**: (link to shadcn‑svelte registry)

---