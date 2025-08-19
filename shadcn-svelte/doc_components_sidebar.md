# shadcn‑svelte Sidebar – Code Documentation

> A composable, themeable and customizable sidebar component for SvelteKit, Vite, Astro and plain Svelte projects.

---

## Table of Contents

| Section | Description |
|---------|-------------|
| [Installation](#installation) | Add the sidebar package and required CSS variables |
| [Structure](#structure) | The building blocks of a sidebar |
| [Basic Usage](#basic-usage) | Minimal example – a collapsible sidebar with a menu |
| [Component Reference](#component-reference) | Detailed API for every exported component |
| [Theming](#theming) | CSS variables and dark‑mode support |
| [Styling Tips](#styling-tips) | Using data‑attributes for state‑based styling |
| [Advanced Patterns](#advanced-patterns) | Collapsible groups, sub‑menus, custom triggers, etc. |
| [FAQ & Troubleshooting](#faq--troubleshooting) | Common questions and solutions |

---

## Installation

```bash
# Using pnpm
pnpm dlx shadcn-svelte@latest add sidebar

# Or npm / bun / yarn
npm i shadcn-svelte
```

Add the required CSS variables to your global stylesheet (e.g. `src/app.css`):

```css
/* Light theme */
:root {
  --sidebar: oklch(0.985 0 0);
  --sidebar-foreground: oklch(0.145 0 0);
  --sidebar-primary: oklch(0.205 0 0);
  --sidebar-primary-foreground: oklch(0.985 0 0);
  --sidebar-accent: oklch(0.97 0 0);
  --sidebar-accent-foreground: oklch(0.205 0 0);
  --sidebar-border: oklch(0.922 0 0);
  --sidebar-ring: oklch(0.708 0 0);
}

/* Dark theme */
.dark {
  --sidebar: oklch(0.205 0 0);
  --sidebar-foreground: oklch(0.985 0 0);
  --sidebar-primary: oklch(0.488 0.243 264.376);
  --sidebar-primary-foreground: oklch(0.985 0 0);
  --sidebar-accent: oklch(0.269 0 0);
  --sidebar-accent-foreground: oklch(0.985 0 0);
  --sidebar-border: oklch(1 0 0 / 10%);
  --sidebar-ring: oklch(0.439 0 0);
}
```

> **Tip** – If you need a different width for the sidebar, set the CSS variables `--sidebar-width` and `--sidebar-width-mobile` on the `<Sidebar.Provider>` element.

---

## Structure

A sidebar is composed of the following parts:

| Component | Purpose |
|-----------|---------|
| `Sidebar.Provider` | Provides context and manages open/closed state |
| `Sidebar.Root` | The container that renders the sidebar |
| `Sidebar.Header` / `Sidebar.Footer` | Sticky header/footer |
| `Sidebar.Content` | Scrollable area that holds groups |
| `Sidebar.Group` | A section inside the content |
| `Sidebar.Trigger` | Button that toggles the sidebar |

---

## Basic Usage

### 1. Wrap the app in a `Sidebar.Provider`

```svelte
<!-- src/routes/+layout.svelte -->
<script lang="ts">
  import * as Sidebar from "$lib/components/ui/sidebar/index.js";
  import AppSidebar from "$lib/components/app-sidebar.svelte";
  let { children } = $props();
</script>

<Sidebar.Provider>
  <AppSidebar />
  <main>
    <Sidebar.Trigger />
    {@render children?.()}
  </main>
</Sidebar.Provider>
```

### 2. Create the sidebar component

```svelte
<!-- src/lib/components/app-sidebar.svelte -->
<script lang="ts">
  import * as Sidebar from "$lib/components/ui/sidebar/index.js";
</script>

<Sidebar.Root>
  <Sidebar.Content />
</Sidebar.Root>
```

### 3. Add a menu

```svelte
<script lang="ts">
  import CalendarIcon from "@lucide/svelte/icons/calendar";
  import HouseIcon from "@lucide/svelte/icons/house";
  import InboxIcon from "@lucide/svelte/icons/inbox";
  import SearchIcon from "@lucide/svelte/icons/search";
  import SettingsIcon from "@lucide/svelte/icons/settings";
  import * as Sidebar from "$lib/components/ui/sidebar/index.js";

  const items = [
    { title: "Home", url: "#", icon: HouseIcon },
    { title: "Inbox", url: "#", icon: InboxIcon },
    { title: "Calendar", url: "#", icon: CalendarIcon },
    { title: "Search", url: "#", icon: SearchIcon },
    { title: "Settings", url: "#", icon: SettingsIcon },
  ];
</script>

<Sidebar.Root>
  <Sidebar.Content>
    <Sidebar.Group>
      <Sidebar.GroupLabel>Application</Sidebar.GroupLabel>
      <Sidebar.GroupContent>
        <Sidebar.Menu>
          {#each items as item (item.title)}
            <Sidebar.MenuItem>
              <Sidebar.MenuButton>
                {#snippet child({ props })}
                  <a href={item.url} {...props}>
                    <item.icon />
                    <span>{item.title}</span>
                  </a>
                {/snippet}
              </Sidebar.MenuButton>
            </Sidebar.MenuItem>
          {/each}
        </Sidebar.Menu>
      </Sidebar.GroupContent>
    </Sidebar.Group>
  </Sidebar.Content>
</Sidebar.Root>
```

You now have a fully functional, collapsible sidebar.

---

## Component Reference

### `Sidebar.Provider`

| Prop | Type | Description |
|------|------|-------------|
| `open` | `boolean` | Bindable open state |
| `onOpenChange` | `(open: boolean) => void` | Callback after state change |
| `style` | `string` | Inline CSS to set `--sidebar-width` / `--sidebar-width-mobile` |

**Constants**

```ts
// src/lib/components/ui/sidebar/constants.ts
export const SIDEBAR_WIDTH = "16rem";
export const SIDEBAR_WIDTH_MOBILE = "18rem";
export const SIDEBAR_KEYBOARD_SHORTCUT = "b";
```

### `Sidebar.Root`

| Prop | Type | Description |
|------|------|-------------|
| `side` | `"left" | "right"` | Sidebar side |
| `variant` | `"sidebar" | "floating" | "inset"` | Visual variant |
| `collapsible` | `"offcanvas" | "icon" | "none"` | Collapsible mode |
| `useSidebar` | `() => SidebarContext` | Hook to access state |

### `Sidebar.Header` / `Sidebar.Footer`

Render sticky header/footer. Example with a dropdown:

```svelte
<Sidebar.Header>
  <Sidebar.Menu>
    <Sidebar.MenuItem>
      <DropdownMenu.Root>
        <DropdownMenu.Trigger>
          {#snippet child({ props })}
            <Sidebar.MenuButton {...props}>
              Select Workspace
              <ChevronDown class="ml-auto" />
            </Sidebar.MenuButton>
          {/snippet}
        </DropdownMenu.Trigger>
        <DropdownMenu.Content class="w-(--bits-dropdown-menu-anchor-width)">
          <DropdownMenu.Item><span>Acme Inc</span></DropdownMenu.Item>
          <DropdownMenu.Item><span>Acme Corp.</span></DropdownMenu.Item>
        </DropdownMenu.Content>
      </DropdownMenu.Root>
    </Sidebar.MenuItem>
  </Sidebar.Menu>
</Sidebar.Header>
```

### `Sidebar.Content`

Wraps the scrollable area that contains `Sidebar.Group` components.

### `Sidebar.Group`

| Sub‑component | Purpose |
|---------------|---------|
| `Sidebar.GroupLabel` | Section title |
| `Sidebar.GroupAction` | Optional action button |
| `Sidebar.GroupContent` | Container for menu items |

**Collapsible group example**

```svelte
<Collapsible.Root open class="group/collapsible">
  <Sidebar.Group>
    <Sidebar.GroupLabel>
      {#snippet child({ props })}
        <Collapsible.Trigger {...props}>
          Help
          <ChevronDown class="ml-auto transition-transform group-data-[state=open]/collapsible:rotate-180" />
        </Collapsible.Trigger>
      {/snippet}
    </Sidebar.GroupLabel>
    <Collapsible.Content>
      <Sidebar.GroupContent />
    </Collapsible.Content>
  </Sidebar.Group>
</Collapsible.Root>
```

### `Sidebar.Menu`

Container for menu items. Use `Sidebar.MenuItem`, `Sidebar.MenuButton`, `Sidebar.MenuAction`, `Sidebar.MenuSub`, etc.

#### `Sidebar.MenuButton`

```svelte
<Sidebar.MenuButton>
  {#snippet child({ props })}
    <a href="/home" {...props}> Home </a>
  {/snippet}
</Sidebar.MenuButton>
```

Add icon and label:

```svelte
<Sidebar.MenuButton>
  {#snippet child({ props })}
    <a href="/home" {...props}>
      <House />
      <span>Home</span>
    </a>
  {/snippet}
</Sidebar.MenuButton>
```

Mark active:

```svelte
<Sidebar.MenuButton isActive>
  {#snippet child({ props })}
    <a href="/home" {...props}>
      <House />
      <span>Home</span>
    </a>
  {/snippet}
</Sidebar.MenuButton>
```

#### `Sidebar.MenuAction`

Independent button (e.g., add, edit):

```svelte
<Sidebar.MenuItem>
  <Sidebar.MenuButton>
    {#snippet child({ props })}
      <a href="/home" {...props}>
        <House />
        <span>Home</span>
      </a>
    {/snippet}
  </Sidebar.MenuButton>
  <Sidebar.MenuAction>
    <Plus /> <span class="sr-only">Add Project</span>
  </Sidebar.MenuAction>
</Sidebar.MenuItem>
```

#### `Sidebar.MenuSub`

```svelte
<Sidebar.MenuItem>
  <Sidebar.MenuButton />
  <Sidebar.MenuSub>
    <Sidebar.MenuSubItem>
      <Sidebar.MenuSubButton />
    </Sidebar.MenuSubItem>
    <Sidebar.MenuSubItem>
      <Sidebar.MenuSubButton />
    </Sidebar.MenuSubItem>
  </Sidebar.MenuSub>
</Sidebar.MenuItem>
```

#### `Sidebar.MenuBadge`

```svelte
<Sidebar.MenuItem>
  <Sidebar.MenuButton />
  <Sidebar.MenuBadge>24</Sidebar.MenuBadge>
</Sidebar.MenuItem>
```

#### `Sidebar.MenuSkeleton`

```svelte
<Sidebar.Menu>
  {#each Array.from({ length: 5 }) as _, index (index)}
    <Sidebar.MenuItem>
      <Sidebar.MenuSkeleton />
    </Sidebar.MenuItem>
  {/each}
</Sidebar.Menu>
```

### `Sidebar.Separator`

```svelte
<Sidebar.Root>
  <Sidebar.Header />
  <Sidebar.Separator />
  <Sidebar.Content>
    <Sidebar.Group />
    <Sidebar.Separator />
    <Sidebar.Group />
  </Sidebar.Content>
</Sidebar.Root>
```

### `Sidebar.Trigger`

```svelte
<Sidebar.Provider>
  <Sidebar.Root />
  <main>
    <Sidebar.Trigger />
  </main>
</Sidebar.Provider>
```

**Custom trigger**

```svelte
<script lang="ts">
  import { useSidebar } from "$lib/components/ui/sidebar/index.js";
  const sidebar = useSidebar();
</script>

<button on:click={() => sidebar.toggle()}>Toggle Sidebar</button>
```

### `Sidebar.Rail`

```svelte
<Sidebar.Root>
  <Sidebar.Header />
  <Sidebar.Content>
    <Sidebar.Group />
  </Sidebar.Content>
  <Sidebar.Footer />
  <Sidebar.Rail />
</Sidebar.Root>
```

### Controlled Sidebar

```svelte
<script lang="ts">
  import * as Sidebar from "$lib/components/ui/sidebar/index.js";
  let myOpen = $state(true);
</script>

<Sidebar.Provider bind:open={() => myOpen, (newOpen) => (myOpen = newOpen)}>
  <Sidebar.Root />
</Sidebar.Provider>
```

---

## Theming

All sidebar styles are driven by CSS variables. Override them to match your design system.

```css
/* Example: custom colors */
:root {
  --sidebar: #f8fafc;
  --sidebar-foreground: #1e293b;
  --sidebar-primary: #3b82f6;
  --sidebar-primary-foreground: #ffffff;
  --sidebar-accent: #e2e8f0;
  --sidebar-accent-foreground: #1e293b;
  --sidebar-border: #cbd5e1;
  --sidebar-ring: #3b82f6;
}
```

Dark mode is handled automatically via the `.dark` selector.

---

## Styling Tips

### Hide elements in icon mode

```svelte
<Sidebar.Root collapsible="icon">
  <Sidebar.Content>
    <Sidebar.Group class="group-data-[collapsible=icon]:hidden" />
  </Sidebar.Content>
</Sidebar.Root>
```

### Show menu action when button is active

```svelte
<Sidebar.MenuItem>
  <Sidebar.MenuButton />
  <Sidebar.MenuAction
    class="peer-data-[active=true]/menu-button:opacity-100"
  />
</Sidebar.MenuItem>
```

> These utilities rely on the `data-` attributes emitted by the components. They are fully compatible with Tailwind’s `group-*` and `peer-*` variants.

---

## Advanced Patterns

| Pattern | Example |
|---------|---------|
| **Collapsible Sidebar.Group** | See the “Collapsible group example” above |
| **Sidebar.Menu with Dropdown** | See the “Sidebar.MenuAction with DropdownMenu” example |
| **Sidebar.MenuSub** | See the “Sidebar.MenuSub” example |
| **Custom Trigger** | See the “Custom trigger” example |
| **Controlled Sidebar** | See the “Controlled Sidebar” example |

---

## FAQ & Troubleshooting

| Question | Answer |
|----------|--------|
| **How do I change the keyboard shortcut?** | Edit `SIDEBAR_KEYBOARD_SHORTCUT` in `constants.ts`. |
| **Can I use the sidebar in a non‑SvelteKit project?** | Yes – just import the components and wrap your app in `Sidebar.Provider`. |
| **What if I need a different width on mobile?** | Set `--sidebar-width-mobile` on `<Sidebar.Provider>` or use the `SIDEBAR_WIDTH_MOBILE` constant. |
| **How do I style the sidebar when it’s collapsed to icons?** | Use `group-data-[collapsible=icon]` or `peer-data-[state=open]/menu-button` utilities. |
| **Can I use the sidebar with other shadcn‑svelte components?** | Absolutely – the components are composable. For example, you can nest a `DropdownMenu` inside a `Sidebar.MenuItem`. |

---

### End of Documentation

Feel free to copy, modify, and extend the examples to fit your project’s needs. Happy coding!