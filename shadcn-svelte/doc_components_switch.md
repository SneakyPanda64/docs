# Switch – shadcn‑svelte

A simple toggle component that lets the user switch between a checked and unchecked state.

> **Note** – This component is part of the official shadcn‑svelte UI library.  
> It is built with Tailwind CSS and is fully accessible.

---

## 📦 Installation

```bash
# Using pnpm
pnpm dlx shadcn-svelte@latest add switch

# Or with npm
npm i shadcn-svelte
```

> The CLI command automatically adds the component files to your project.

---

## 🚀 Usage

```svelte
<script lang="ts">
  import { Switch } from "$lib/components/ui/switch/index.js";
</script>

<Switch />
```

### With a label

```svelte
<script lang="ts">
  import { Label } from "$lib/components/ui/label/index.js";
  import { Switch } from "$lib/components/ui/switch/index.js";
</script>

<div class="flex items-center space-x-2">
  <Switch id="airplane-mode" />
  <Label for="airplane-mode">Airplane Mode</Label>
</div>
```

---

## 📚 API Reference

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `checked` | `boolean` | `false` | Whether the switch is on. |
| `disabled` | `boolean` | `false` | Disables the switch. |
| `id` | `string` | – | The id of the underlying `<input>` element. |
| `name` | `string` | – | The name of the underlying `<input>` element. |
| `value` | `string` | – | The value of the underlying `<input>` element. |
| `on:change` | `Event` | – | Fired when the checked state changes. |

> All props are forwarded to the native `<input type="checkbox">` element.

---

## 🎨 Theming

The component uses Tailwind CSS classes that respect the current theme.  
To enable dark mode, add the `dark:` variants to your Tailwind config or use the built‑in dark mode support.

```css
/* Example Tailwind config */
module.exports = {
  darkMode: 'class',
  // ...
}
```

---

## 📦 Component Source

The source code is available in the repository:

```
src/lib/components/ui/switch/
├─ index.js
├─ switch.svelte
└─ switch.css
```

Feel free to inspect or modify the implementation.

---

## 📄 Example – Form

```svelte
<script lang="ts">
  import { Switch } from "$lib/components/ui/switch/index.js";
  import { Label } from "$lib/components/ui/label/index.js";
</script>

<form class="space-y-4">
  <div class="flex items-center space-x-2">
    <Switch id="email-notifications" />
    <Label for="email-notifications">Email Notifications</Label>
  </div>

  <div class="flex items-center space-x-2">
    <Switch id="marketing-emails" />
    <Label for="marketing-emails">Marketing emails</Label>
  </div>

  <div class="flex items-center space-x-2">
    <Switch id="security-emails" />
    <Label for="security-emails">Security emails</Label>
  </div>

  <button type="submit" class="btn-primary">Submit</button>
</form>
```

---

## 📦 Registry

The component is registered in the shadcn‑svelte registry.  
You can find it in `registry.json` under the `ui` namespace.

---

## 📚 Further Reading

- [Shadcn‑Svelte Docs](https://shadcn-svelte.com/docs)
- [Tailwind CSS Docs](https://tailwindcss.com/docs)
- [Accessibility Guide](https://www.w3.org/WAI/ARIA/apg/)

---