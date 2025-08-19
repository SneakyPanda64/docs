# shadcn‑svelte – Navigation Menu

> A fully‑featured, accessible navigation menu component built with Tailwind CSS and Svelte.  
> It is part of the **shadcn‑svelte** component library, a port of the original shadcn‑ui components.

---

## Table of Contents

| Section | Description |
|---------|-------------|
| [Overview](#overview) | What the component does |
| [Installation](#installation) | How to add it to your project |
| [Usage](#usage) | Minimal example |
| [API](#api) | Props, slots, and events |
| [Examples](#examples) | Real‑world usage patterns |
| [Theming](#theming) | Customizing colors & styles |
| [Accessibility](#accessibility) | Keyboard & ARIA support |
| [FAQ](#faq) | Common questions |

---

## Overview

The **NavigationMenu** component provides a multi‑level, dropdown‑style navigation system that is:

* **Keyboard‑friendly** – fully operable with arrow keys, `Tab`, `Enter`, and `Esc`.
* **Screen‑reader accessible** – uses proper ARIA roles (`menubar`, `menuitem`, `menu`).
* **Responsive** – collapses into a mobile‑friendly menu on small screens.
* **Composable** – each part (`Root`, `List`, `Item`, `Trigger`, `Content`, `Link`) is a separate Svelte component that can be styled independently.

---

## Installation

### 1. Add the package

```bash
# Using pnpm
pnpm dlx shadcn-svelte@latest add navigation-menu

# Or with npm / yarn / bun
npm i shadcn-svelte
# or
yarn add shadcn-svelte
# or
bun add shadcn-svelte
```

### 2. Import the component

```svelte
<script lang="ts">
  import * as NavigationMenu from "$lib/components/ui/navigation-menu/index.js";
</script>
```

> **Tip** – The `* as NavigationMenu` import gives you access to all sub‑components (`Root`, `List`, `Item`, etc.) in a single namespace.

---

## Usage

### Minimal Example

```svelte
<script lang="ts">
  import * as NavigationMenu from "$lib/components/ui/navigation-menu/index.js";
</script>

<NavigationMenu.Root>
  <NavigationMenu.List>
    <NavigationMenu.Item>
      <NavigationMenu.Trigger>Item One</NavigationMenu.Trigger>
      <NavigationMenu.Content>
        <NavigationMenu.Link>Link</NavigationMenu.Link>
      </NavigationMenu.Content>
    </NavigationMenu.Item>
  </NavigationMenu.List>
</NavigationMenu.Root>
```

### Full‑Featured Example

```svelte
<script lang="ts">
  import * as NavigationMenu from "$lib/components/ui/navigation-menu/index.js";
  import { cn } from "$lib/utils.js";
  import { navigationMenuTriggerStyle } from "$lib/components/ui/navigation-menu/navigation-menu-trigger.svelte";
  import type { HTMLAttributes } from "svelte/elements";
  import CircleHelpIcon from "@lucide/svelte/icons/circle-help";
  import CircleIcon from "@lucide/svelte/icons/circle";
  import CircleCheckIcon from "@lucide/svelte/icons/circle-check";

  const components = [
    {
      title: "Alert Dialog",
      href: "/docs/components/alert-dialog",
      description:
        "A modal dialog that interrupts the user with important content and expects a response."
    },
    {
      title: "Hover Card",
      href: "/docs/components/hover-card",
      description:
        "For sighted users to preview content available behind a link."
    },
    {
      title: "Progress",
      href: "/docs/components/progress",
      description:
        "Displays an indicator showing the completion progress of a task, typically displayed as a progress bar."
    },
    {
      title: "Scroll-area",
      href: "/docs/components/scroll-area",
      description: "Visually or semantically separates content."
    },
    {
      title: "Tabs",
      href: "/docs/components/tabs",
      description:
        "A set of layered sections of content—known as tab panels—that are displayed one at a time."
    },
    {
      title: "Tooltip",
      href: "/docs/components/tooltip",
      description:
        "A popup that displays information related to an element when the element receives keyboard focus or the mouse hovers over it."
    }
  ];

  type ListItemProps = HTMLAttributes<HTMLAnchorElement> & {
    title: string;
    href: string;
    content: string;
  };
</script>

{#snippet ListItem({ title, content, href, class: className, ...restProps }: ListItemProps)}
  <li>
    <NavigationMenu.Link>
      {#snippet child()}
        <a
          {href}
          class={cn(
            "hover:bg-accent hover:text-accent-foreground focus:bg-accent focus:text-accent-foreground block select-none space-y-1 rounded-md p-3 leading-none no-underline outline-none transition-colors",
            className
          )}
          {...restProps}
        >
          <div class="text-sm font-medium leading-none">{title}</div>
          <p class="text-muted-foreground line-clamp-2 text-sm leading-snug">
            {content}
          </p>
        </a>
      {/snippet}
    </NavigationMenu.Link>
  </li>
{/snippet}

<NavigationMenu.Root viewport={false}>
  <NavigationMenu.List>
    <!-- Home -->
    <NavigationMenu.Item>
      <NavigationMenu.Trigger>Home</NavigationMenu.Trigger>
      <NavigationMenu.Content>
        <ul class="grid gap-2 p-2 md:w-[400px] lg:w-[500px] lg:grid-cols-[.75fr_1fr]">
          <li class="row-span-3">
            <NavigationMenu.Link
              class="from-muted/50 to-muted bg-linear-to-b outline-hidden flex h-full w-full select-none flex-col justify-end rounded-md p-6 no-underline focus:shadow-md"
            >
              {#snippet child({ props })}
                <a {...props} href="/">
                  <div class="mb-2 mt-4 text-lg font-medium">shadcn-svelte</div>
                  <p class="text-muted-foreground text-sm leading-tight">
                    Beautifully designed components built with Tailwind CSS.
                  </p>
                </a>
              {/snippet}
            </NavigationMenu.Link>
          </li>
          {@render ListItem({
            href: "/docs",
            title: "Introduction",
            content: "Re‑usable components built using Bits UI and Tailwind CSS."
          })}
          {@render ListItem({
            href: "/docs/installation",
            title: "Installation",
            content: "How to install dependencies and structure your app."
          })}
          {@render ListItem({
            href: "/docs/components/typography",
            title: "Typography",
            content: "Styles for headings, paragraphs, lists…etc"
          })}
        </ul>
      </NavigationMenu.Content>
    </NavigationMenu.Item>

    <!-- Components -->
    <NavigationMenu.Item>
      <NavigationMenu.Trigger>Components</NavigationMenu.Trigger>
      <NavigationMenu.Content>
        <ul class="grid w-[400px] gap-2 p-2 md:w-[500px] md:grid-cols-2 lg:w-[600px]">
          {#each components as component, i (i)}
            {@render ListItem({
              href: component.href,
              title: component.title,
              content: component.description
            })}
          {/each}
        </ul>
      </NavigationMenu.Content>
    </NavigationMenu.Item>

    <!-- Docs Link -->
    <NavigationMenu.Item>
      <NavigationMenu.Link>
        {#snippet child()}
          <a href="/docs" class={navigationMenuTriggerStyle()}>Docs</a>
        {/snippet}
      </NavigationMenu.Link>
    </NavigationMenu.Item>

    <!-- List Example -->
    <NavigationMenu.Item>
      <NavigationMenu.Trigger>List</NavigationMenu.Trigger>
      <NavigationMenu.Content>
        <ul class="grid w-[300px] gap-4 p-2">
          <li>
            <NavigationMenu.Link href="#">
              <div class="font-medium">Components</div>
              <div class="text-muted-foreground">
                Browse all components in the library.
              </div>
            </NavigationMenu.Link>
            <NavigationMenu.Link href="#">
              <div class="font-medium">Documentation</div>
              <div class="text-muted-foreground">
                Learn how to use the library.
              </div>
            </NavigationMenu.Link>
            <NavigationMenu.Link href="#">
              <div class="font-medium">Blog</div>
              <div class="text-muted-foreground">
                Read our latest blog posts.
              </div>
            </NavigationMenu.Link>
          </li>
        </ul>
      </NavigationMenu.Content>
    </NavigationMenu.Item>

    <!-- Simple Example -->
    <NavigationMenu.Item>
      <NavigationMenu.Trigger>Simple</NavigationMenu.Trigger>
      <NavigationMenu.Content>
        <ul class="grid w-[200px] gap-4 p-2">
          <li>
            <NavigationMenu.Link href="#">Components</NavigationMenu.Link>
            <NavigationMenu.Link href="#">Documentation</NavigationMenu.Link>
            <NavigationMenu.Link href="#">Blocks</NavigationMenu.Link>
          </li>
        </ul>
      </NavigationMenu.Content>
    </NavigationMenu.Item>

    <!-- With Icon Example -->
    <NavigationMenu.Item>
      <NavigationMenu.Trigger>With Icon</NavigationMenu.Trigger>
      <NavigationMenu.Content>
        <ul class="grid w-[200px] gap-4 p-2">
          <li>
            <NavigationMenu.Link href="#" class="flex-row items-center gap-2">
              <CircleHelpIcon />
              Backlog
            </NavigationMenu.Link>
            <NavigationMenu.Link href="#" class="flex-row items-center gap-2">
              <CircleIcon />
              To Do
            </NavigationMenu.Link>
            <NavigationMenu.Link href="#" class="flex-row items-center gap-2">
              <CircleCheckIcon />
              Done
            </NavigationMenu.Link>
          </li>
        </ul>
      </NavigationMenu.Content>
    </NavigationMenu.Item>
  </NavigationMenu.List>
</NavigationMenu.Root>
```

---

## API

| Component | Props | Slots | Events |
|-----------|-------|-------|--------|
| `NavigationMenu.Root` | `viewport?: boolean` – whether to render a viewport container | `default` – content of the menu | – |
| `NavigationMenu.List` | – | `default` – list items | – |
| `NavigationMenu.Item` | – | `default` – trigger + content | – |
| `NavigationMenu.Trigger` | – | `default` – trigger label | `click`, `keydown` |
| `NavigationMenu.Content` | – | `default` – dropdown panel | – |
| `NavigationMenu.Link` | `href: string` | `default` – link content | `click` |

> All components forward any unknown props to the underlying DOM element, so you can add `class`, `style`, `id`, etc.

---

## Theming

The component uses Tailwind CSS utility classes. To customize:

1. **Override Tailwind config** – add custom colors, spacing, or variants.
2. **Use the `cn` helper** – combine your own classes with the component’s defaults.
3. **Create a custom trigger style** – see `navigationMenuTriggerStyle()` in the example.

```ts
// utils.ts
export function navigationMenuTriggerStyle() {
  return cn(
    "inline-flex items-center rounded-md px-3 py-2 text-sm font-medium",
    "text-muted-foreground hover:bg-accent hover:text-accent-foreground",
    "focus:outline-none focus:ring-2 focus:ring-ring focus:ring-offset-2"
  );
}
```

---

## Accessibility

* **Keyboard** – `Tab` to focus, `Enter`/`Space` to open, arrow keys to navigate, `Esc` to close.
* **ARIA** – automatically sets `role="menubar"`, `role="menuitem"`, `role="menu"`, and `aria-haspopup="true"` where appropriate.
* **Focus Management** – the first item in a submenu receives focus when opened.

---

## FAQ

| Question | Answer |
|----------|--------|
| *Can I use the component in a SvelteKit project?* | Yes. Import the component as shown and add Tailwind CSS to your project. |
| *How do I add icons to menu items?* | Pass an icon component inside the `NavigationMenu.Link` slot, e.g. `<CircleHelpIcon />`. |
| *Is the component responsive?* | The default styles are mobile‑friendly. For a hamburger menu, wrap the `NavigationMenu.Root` in a toggle button. |
| *Can I customize the dropdown width?* | Yes – set a width on the `NavigationMenu.Content` element or use Tailwind width utilities. |

---

## License

MIT © shadcn‑svelte contributors

---