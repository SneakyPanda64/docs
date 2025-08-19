# shadcn‑svelte – Card Component

The **Card** component is a flexible container that can hold a header, content, and footer. It’s useful for forms, dashboards, or any UI that needs a clear visual hierarchy.

---

## 📦 Installation

```bash
# Using pnpm
pnpm dlx shadcn-svelte@latest add card

# Using npm
npm i shadcn-svelte@latest
npx shadcn-svelte add card

# Using bun
bun x shadcn-svelte@latest add card

# Using yarn
yarn dlx shadcn-svelte@latest add card
```

> **Tip:** The CLI will automatically add the necessary files to your `$lib/components/ui/card` folder.

---

## 🔧 Usage

```svelte
<script lang="ts">
  import * as Card from "$lib/components/ui/card/index.js";
</script>

<Card.Root>
  <Card.Header>
    <Card.Title>Card Title</Card.Title>
    <Card.Description>Card Description</Card.Description>
  </Card.Header>

  <Card.Content>
    <p>Card Content</p>
  </Card.Content>

  <Card.Footer>
    <p>Card Footer</p>
  </Card.Footer>
</Card.Root>
```

### Props & Variants

| Element | Props | Description |
|---------|-------|-------------|
| `Card.Root` | `class` | Tailwind classes for the outer wrapper. |
| `Card.Header` | — | Container for title, description, and actions. |
| `Card.Title` | — | Title text. |
| `Card.Description` | — | Subtitle or description. |
| `Card.Action` | — | Optional action area (e.g., a link). |
| `Card.Content` | — | Main content area. |
| `Card.Footer` | — | Footer area, often for buttons. |

---

## 📄 Example: Login Card

```svelte
<script lang="ts">
  import { Button } from "$lib/components/ui/button/index.js";
  import { Label } from "$lib/components/ui/label/index.js";
  import { Input } from "$lib/components/ui/input/index.js";
  import * as Card from "$lib/components/ui/card/index.js";
</script>

<Card.Root class="w-full max-w-sm">
  <Card.Header>
    <Card.Title>Login to your account</Card.Title>
    <Card.Description>
      Enter your email below to login to your account
    </Card.Description>
    <Card.Action>
      <Button variant="link">Sign Up</Button>
    </Card.Action>
  </Card.Header>

  <Card.Content>
    <form>
      <div class="flex flex-col gap-6">
        <div class="grid gap-2">
          <Label for="email">Email</Label>
          <Input id="email" type="email" placeholder="m@example.com" required />
        </div>

        <div class="grid gap-2">
          <div class="flex items-center">
            <Label for="password">Password</Label>
            <a
              href="##"
              class="ml-auto inline-block text-sm underline-offset-4 hover:underline"
            >
              Forgot your password?
            </a>
          </div>
          <Input id="password" type="password" required />
        </div>
      </div>
    </form>
  </Card.Content>

  <Card.Footer class="flex-col gap-2">
    <Button type="submit" class="w-full">Login</Button>
    <Button variant="outline" class="w-full">Login with Google</Button>
  </Card.Footer>
</Card.Root>
```

---

## 🎨 Theming & Dark Mode

The Card component inherits Tailwind’s utility classes, so you can style it with any Tailwind theme or dark‑mode variant. For example:

```html
<Card.Root class="bg-white dark:bg-gray-800 shadow-md rounded-lg p-6">
  <!-- ... -->
</Card.Root>
```

---

## 📚 Related Components

- **Button** – `$lib/components/ui/button`
- **Label** – `$lib/components/ui/label`
- **Input** – `$lib/components/ui/input`
- **Alert** – `$lib/components/ui/alert`
- **Dialog** – `$lib/components/ui/dialog`

---

## 📦 Registry

The Card component is part of the official shadcn‑svelte registry. You can view the full JSON definition in `registry-item.json` or explore the entire registry in `registry.json`.

---

## 📖 Further Reading

- [Installation Guide](https://shadcn-svelte.com/docs/installation)
- [Theming & Dark Mode](https://shadcn-svelte.com/docs/theming)
- [CLI Reference](https://shadcn-svelte.com/docs/cli)
- [Migration to Svelte 5](https://shadcn-svelte.com/docs/migration)

---

Happy coding! 🚀