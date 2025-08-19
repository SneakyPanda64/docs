# Collapsible – shadcn‑svelte

> An interactive component that expands / collapses a panel.

---

## 📦 Installation

```bash
# Using the shadcn‑svelte CLI
pnpm dlx shadcn-svelte@latest add collapsible
```

> **Note**  
> The component is available for all supported package managers (`pnpm`, `npm`, `bun`, `yarn`).  
> After running the command, the component will be added to your `$lib/components/ui/collapsible` folder.

---

## 🚀 Usage

```svelte
<script lang="ts">
  import * as Collapsible from "$lib/components/ui/collapsible/index.js";
</script>

<Collapsible.Root>
  <Collapsible.Trigger>
    Can I use this in my project?
  </Collapsible.Trigger>

  <Collapsible.Content>
    Yes. Free to use for personal and commercial projects. No attribution required.
  </Collapsible.Content>
</Collapsible.Root>
```

### Advanced Example

```svelte
<script lang="ts">
  import ChevronsUpDownIcon from "@lucide/svelte/icons/chevrons-up-down";
  import * as Collapsible from "$lib/components/ui/collapsible/index.js";
  import { buttonVariants } from "$lib/components/ui/button/index.js";
</script>

<Collapsible.Root class="w-[350px] space-y-2">
  <div class="flex items-center justify-between space-x-4 px-4">
    <h4 class="text-sm font-semibold">@huntabyte starred 3 repositories</h4>

    <Collapsible.Trigger
      class={buttonVariants({ variant: "ghost", size: "sm", class: "w-9 p-0" })}
    >
      <ChevronsUpDownIcon />
      <span class="sr-only">Toggle</span>
    </Collapsible.Trigger>
  </div>

  <div class="rounded-md border px-4 py-3 font-mono text-sm">
    @huntabyte/bits-ui
  </div>

  <Collapsible.Content class="space-y-2">
    <div class="rounded-md border px-4 py-3 font-mono text-sm">
      @melt-ui/melt-ui
    </div>
    <div class="rounded-md border px-4 py-3 font-mono text-sm">
      @sveltejs/svelte
    </div>
  </Collapsible.Content>
</Collapsible.Root>
```

---

## 📚 API Reference

| Element | Description | Props |
|---------|-------------|-------|
| `Collapsible.Root` | The container that manages the open/closed state. | `open?: boolean`<br>`on:openChange` |
| `Collapsible.Trigger` | The element that toggles the panel. | `as?: string | SvelteComponent`<br>`class?: string` |
| `Collapsible.Content` | The collapsible panel content. | `as?: string | SvelteComponent`<br>`class?: string` |

> **Tip** – The component uses Svelte’s built‑in `<svelte:fragment>` for slotting, so you can nest any markup inside `Trigger` and `Content`.

---

## 🔧 Theming & Dark Mode

The component inherits Tailwind CSS utilities. To support dark mode, simply add the `dark:` variants to your classes:

```svelte
<div class="rounded-md border px-4 py-3 font-mono text-sm dark:border-gray-700">
  @huntabyte/bits-ui
</div>
```

---

## 📦 Registry

The component is part of the official shadcn‑svelte registry. You can find it in the `registry.json` under the `ui` namespace.

---

## 📖 Further Reading

- **Installation** – [SvelteKit](https://github.com/shadcn-svelte/docs#sveltekit), [Vite](https://github.com/shadcn-svelte/docs#vite), [Astro](https://github.com/shadcn-svelte/docs#astro)
- **Theming** – [Dark Mode](https://github.com/shadcn-svelte/docs#dark-mode)
- **CLI** – `pnpm dlx shadcn-svelte@latest add <component>`
- **Migration** – [Legacy Docs](https://github.com/shadcn-svelte/docs#legacy-docs)

---

> Built by **shadcn**. Ported to Svelte by **Huntabyte** & **CokaKoala**.  
> Feel free to use it in any personal or commercial project – no attribution required.