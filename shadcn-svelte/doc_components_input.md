# shadcn‑svelte – Input Component

The **Input** component renders a standard form input field or a styled component that mimics an input. It supports all native `<input>` attributes and can be customized with Tailwind classes.

---

## 📦 Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add input
```

> **Note:** The CLI will add the component to your `$lib/components/ui/input` folder.

### Manual

```bash
# npm
npm i shadcn-svelte

# yarn
yarn add shadcn-svelte

# bun
bun add shadcn-svelte
```

Then import the component in your Svelte file:

```svelte
<script lang="ts">
  import { Input } from "$lib/components/ui/input/index.js";
</script>
```

---

## ⚙️ Usage

```svelte
<script lang="ts">
  import { Input } from "$lib/components/ui/input/index.js";
</script>

<Input />
```

You can pass any native `<input>` attribute:

```svelte
<Input type="email" placeholder="email" class="max-w-xs" />
```

---

## 📚 Examples

Below are a variety of common use‑cases. Each example includes a preview and the corresponding code snippet.

| Example | Preview | Code |
|---------|---------|------|
| **Default** | <img src="https://via.placeholder.com/200x40?text=Default+Input" alt="Default Input"> | ```svelte<br><Input />``` |
| **Disabled** | <img src="https://via.placeholder.com/200x40?text=Disabled+Input" alt="Disabled Input"> | ```svelte<br><Input disabled />``` |
| **With Label** | <img src="https://via.placeholder.com/200x40?text=Label+Input" alt="Label Input"> | ```svelte<br><label for="email">Email</label><Input id="email" />``` |
| **Email** | <img src="https://via.placeholder.com/200x40?text=Email+Input" alt="Email Input"> | ```svelte<br><Input type="email" placeholder="email" />``` |
| **With Text** | <img src="https://via.placeholder.com/200x40?text=Text+Input" alt="Text Input"> | ```svelte<br><Input placeholder="Enter your email address." />``` |
| **With Button** | <img src="https://via.placeholder.com/200x40?text=Input+with+Button" alt="Input with Button"> | ```svelte<br><div class="flex items-center"><Input placeholder="Subscribe" /><button class="ml-2">Subscribe</button></div>``` |
| **Invalid** | <img src="https://via.placeholder.com/200x40?text=Invalid+Input" alt="Invalid Input"> | ```svelte<br><Input class="border-red-500" />``` |
| **File** | <img src="https://via.placeholder.com/200x40?text=File+Input" alt="File Input"> | ```svelte<br><Input type="file" />``` |
| **Picture** | <img src="https://via.placeholder.com/200x40?text=Picture+Input" alt="Picture Input"> | ```svelte<br><Input type="file" accept="image/*" />``` |
| **Form** | <img src="https://via.placeholder.com/200x40?text=Form+Input" alt="Form Input"> | ```svelte<br><form><Input placeholder="Username" /><button type="submit">Submit</button></form>``` |

> **Tip:** Combine the Input component with Tailwind utilities for layout, spacing, and styling.

---

## 📜 License

This component is part of the **shadcn‑svelte** library, built by **shadcn** and ported to Svelte by **Huntabyte & CokaKoala**. Feel free to use, modify, and contribute.

---