# Context Menu – shadcn‑svelte

A fully‑featured, accessible context‑menu component that works out‑of‑the‑box with Tailwind CSS.  
It supports sub‑menus, checkboxes, radio groups, shortcuts, separators, and more.

> **Tip** – The component is built on top of Radix‑UI primitives, so it follows the same accessibility patterns.

---

## 📦 Installation

```bash
# Using pnpm
pnpm dlx shadcn-svelte@latest add context-menu

# Using npm
npm i shadcn-svelte@latest
npx shadcn-svelte add context-menu

# Using bun
bun x shadcn-svelte@latest add context-menu

# Using yarn
yarn dlx shadcn-svelte@latest add context-menu
```

> The CLI will copy the component files into `src/lib/components/ui/context-menu/` and add the necessary Tailwind classes.

---

## 🔧 Usage

```svelte
<script lang="ts">
  import * as ContextMenu from "$lib/components/ui/context-menu/index.js";
</script>

<ContextMenu.Root>
  <ContextMenu.Trigger>Right click</ContextMenu.Trigger>

  <ContextMenu.Content>
    <ContextMenu.Item>Profile</ContextMenu.Item>
    <ContextMenu.Item>Billing</ContextMenu.Item>
    <ContextMenu.Item>Team</ContextMenu.Item>
    <ContextMenu.Item>Subscription</ContextMenu.Item>
  </ContextMenu.Content>
</ContextMenu.Root>
```

> The `Root` component manages the open/close state.  
> `Trigger` is the element that opens the menu (usually a button or any clickable element).  
> `Content` contains the menu items.

---

## 📚 API Reference

| Component | Description | Props / Slots |
|-----------|-------------|---------------|
| `ContextMenu.Root` | Wrapper that manages state. | `open` (boolean, optional), `onOpenChange` (event) |
| `ContextMenu.Trigger` | Element that opens the menu. | `asChild` (boolean) |
| `ContextMenu.Content` | The floating menu container. | `align`, `side`, `class` |
| `ContextMenu.Item` | A selectable menu item. | `disabled` (boolean), `class` |
| `ContextMenu.Sub` | Container for a sub‑menu. | `class` |
| `ContextMenu.SubTrigger` | Item that opens a sub‑menu. | `class` |
| `ContextMenu.SubContent` | Container for sub‑menu items. | `class` |
| `ContextMenu.Separator` | Visual separator. | `class` |
| `ContextMenu.CheckboxItem` | Item with a checkbox. | `checked` (bindable), `class` |
| `ContextMenu.RadioGroup` | Group of radio items. | `value` (bindable), `class` |
| `ContextMenu.Group` | Logical grouping of items. | `class` |
| `ContextMenu.GroupHeading` | Heading for a group. | `class` |
| `ContextMenu.RadioItem` | Radio‑selectable item. | `value`, `class` |
| `ContextMenu.Shortcut` | Display a keyboard shortcut. | `class` |

> All components accept a `class` prop for custom styling.  
> The `asChild` prop can be used to render the component as a child element instead of a `<div>`.

---

## 🎨 Advanced Example

```svelte
<script lang="ts">
  import * as ContextMenu from "$lib/components/ui/context-menu/index.js";

  let showBookmarks = $state(false);
  let showFullURLs = $state(true);
  let value = $state("pedro");
</script>

<ContextMenu.Root>
  <ContextMenu.Trigger
    class="flex h-[150px] w-[300px] items-center justify-center rounded-md border border-dashed text-sm"
  >
    Right click here
  </ContextMenu.Trigger>

  <ContextMenu.Content class="w-52">
    <ContextMenu.Item inset>
      Back
      <ContextMenu.Shortcut>⌘[</ContextMenu.Shortcut>
    </ContextMenu.Item>

    <ContextMenu.Item inset disabled>
      Forward
      <ContextMenu.Shortcut>⌘]</ContextMenu.Shortcut>
    </ContextMenu.Item>

    <ContextMenu.Item inset>
      Reload
      <ContextMenu.Shortcut>⌘R</ContextMenu.Shortcut>
    </ContextMenu.Item>

    <ContextMenu.Sub>
      <ContextMenu.SubTrigger inset>More Tools</ContextMenu.SubTrigger>
      <ContextMenu.SubContent class="w-48">
        <ContextMenu.Item>
          Save Page As...
          <ContextMenu.Shortcut>⇧⌘S</ContextMenu.Shortcut>
        </ContextMenu.Item>
        <ContextMenu.Item>Create Shortcut...</ContextMenu.Item>
        <ContextMenu.Item>Name Window...</ContextMenu.Item>
        <ContextMenu.Separator />
        <ContextMenu.Item>Developer Tools</ContextMenu.Item>
      </ContextMenu.SubContent>
    </ContextMenu.Sub>

    <ContextMenu.Separator />

    <ContextMenu.CheckboxItem bind:checked={showBookmarks}>
      Show Bookmarks
    </ContextMenu.CheckboxItem>

    <ContextMenu.CheckboxItem bind:checked={showFullURLs}>
      Show Full URLs
    </ContextMenu.CheckboxItem>

    <ContextMenu.Separator />

    <ContextMenu.RadioGroup bind:value>
      <ContextMenu.Group>
        <ContextMenu.GroupHeading inset>People</ContextMenu.GroupHeading>
        <ContextMenu.RadioItem value="pedro">Pedro Duarte</ContextMenu.RadioItem>
        <ContextMenu.RadioItem value="colm">Colm Tuite</ContextMenu.RadioItem>
      </ContextMenu.Group>
    </ContextMenu.RadioGroup>
  </ContextMenu.Content>
</ContextMenu.Root>
```

> *Features demonstrated:*  
> • Sub‑menus (`Sub`, `SubTrigger`, `SubContent`)  
> • Checkboxes (`CheckboxItem`)  
> • Radio groups (`RadioGroup`, `RadioItem`)  
> • Shortcuts (`Shortcut`)  
> • Separators (`Separator`)  
> • Binding state with Svelte’s `$state`

---

## 📦 Registry

If you’re using the shadcn‑svelte registry, add the component to your `registry.json`:

```json
{
  "name": "context-menu",
  "components": [
    {
      "name": "ContextMenu",
      "path": "$lib/components/ui/context-menu/index.js"
    }
  ]
}
```

---

## 📖 Further Reading

- [Shadcn‑Svelte Docs](https://shadcn-svelte.com/docs)  
- [Radix‑UI Context Menu](https://www.radix-ui.com/docs/primitives/overview) – for deeper accessibility insights  
- [Tailwind CSS Docs](https://tailwindcss.com/docs) – for customizing styles

---

Happy coding! 🚀