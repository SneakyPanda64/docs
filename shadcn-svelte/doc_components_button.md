# shadcn‑svelte – Button Component

The **Button** component is a fully‑styled, accessible button that supports a variety of variants, icons, loading states, and can be used as a link.

> **Tip** – All examples below are ready to copy‑paste into a SvelteKit or Vite project that has `shadcn-svelte` installed.

---

## Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add button
```

> The CLI will add the component files to your project and update the registry.

### Manual

```ts
<script lang="ts">
  import { Button } from "$lib/components/ui/button/index.js";
</script>
```

---

## Usage

### Basic Button

```svelte
<script lang="ts">
  import { Button } from "$lib/components/ui/button/index.js";
</script>

<Button>Button</Button>
```

### Variants

```svelte
<script lang="ts">
  import { Button } from "$lib/components/ui/button/index.js";
</script>

<Button variant="outline">Button</Button>
```

### Button as a Link

You can turn the `<Button>` into an `<a>` element simply by passing an `href` prop.

```svelte
<script lang="ts">
  import { Button } from "$lib/components/ui/button/index.js";
</script>

<Button href="/dashboard">Dashboard</Button>
```

Alternatively, use the `buttonVariants` helper to style a plain `<a>` as a button.

```svelte
<script lang="ts">
  import { buttonVariants } from "$lib/components/ui/button";
</script>

<a href="/dashboard" class={buttonVariants({ variant: "outline" })}>
  Dashboard
</a>
```

---

## Examples

Below are the most common button styles. Each example is shown with its code snippet.

| Variant | Code |
|---------|------|
| **Primary** | `<Button>Button</Button>` |
| **Secondary** | `<Button variant="secondary">Button</Button>` |
| **Destructive** | `<Button variant="destructive">Button</Button>` |
| **Outline** | `<Button variant="outline">Button</Button>` |
| **Ghost** | `<Button variant="ghost">Button</Button>` |
| **Link** | `<Button variant="link">Button</Button>` |
| **With Icon** | `<Button variant="outline"><Icon name="log-in" /> Login with Email</Button>` |
| **Loading** | `<Button disabled>Loading…</Button>` |

> **Note** – The icon example assumes you have an `<Icon>` component available. Replace it with your preferred icon implementation.

---

## API Reference

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `variant` | `"default" | "secondary" | "destructive" | "outline" | "ghost" | "link"` | `"default"` | Button style variant. |
| `size` | `"sm" | "md" | "lg"` | `"md"` | Button size. |
| `href` | `string` | `undefined` | If provided, renders an `<a>` element. |
| `disabled` | `boolean` | `false` | Disables the button. |
| `type` | `"button" | "submit" | "reset"` | `"button"` | Button type attribute. |
| `class` | `string` | `""` | Additional CSS classes. |
| `...rest` | `any` | – | Any other native button or anchor attributes. |

---

## Theming & Dark Mode

The component automatically adapts to the current Tailwind CSS theme. No additional configuration is required.

---

## License

This component is part of the **shadcn‑svelte** library, licensed under the MIT license.