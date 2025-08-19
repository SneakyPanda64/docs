# shadcn‑svelte – Breadcrumb Component

The **Breadcrumb** component displays the current navigation path as a hierarchy of links.  
It is fully accessible, theme‑aware, and composable with other shadcn‑svelte UI primitives.

---

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Examples](#examples)
  - [Custom Separator](#custom-separator)
  - [Dropdown in Breadcrumb](#dropdown-in-breadcrumb)
  - [Collapsed Breadcrumb](#collapsed-breadcrumb)
  - [Custom Link Component](#custom-link-component)
  - [Responsive Breadcrumb](#responsive-breadcrumb)
- [API Reference](#api-reference)
  - [`Breadcrumb.Root`](#breadcrumbroot)
  - [`Breadcrumb.List`](#breadcrumblist)
  - [`Breadcrumb.Item`](#breadcrumbitem)
  - [`Breadcrumb.Link`](#breadcrumblink)
  - [`Breadcrumb.Page`](#breadcrumbpage)
  - [`Breadcrumb.Separator`](#breadcrumbseparator)
  - [`Breadcrumb.Ellipsis`](#breadcrumbellipsis)

---

## Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add breadcrumb
```

> **Tip**: The CLI will automatically add the component files to your `$lib/components/ui/breadcrumb` folder.

### Manual

1. Copy the files from `registry.json` → `breadcrumb` into your project.
2. Import the component as shown in the **Usage** section.

---

## Usage

```svelte
<script lang="ts">
  import * as Breadcrumb from "$lib/components/ui/breadcrumb/index.js";
</script>

<Breadcrumb.Root>
  <Breadcrumb.List>
    <Breadcrumb.Item>
      <Breadcrumb.Link href="/">Home</Breadcrumb.Link>
    </Breadcrumb.Item>

    <Breadcrumb.Separator />

    <Breadcrumb.Item>
      <Breadcrumb.Link href="/components">Components</Breadcrumb.Link>
    </Breadcrumb.Item>

    <Breadcrumb.Separator />

    <Breadcrumb.Item>
      <Breadcrumb.Page>Breadcrumb</Breadcrumb.Page>
    </Breadcrumb.Item>
  </Breadcrumb.List>
</Breadcrumb.Root>
```

> The component is fully typed; the `href` prop on `Breadcrumb.Link` is required.

---

## Examples

### Custom Separator

Use a custom component inside the `<slot>` of `<Breadcrumb.Separator />` to change the separator icon.

```svelte
<Breadcrumb.Root>
  <Breadcrumb.List>
    <Breadcrumb.Item>
      <Breadcrumb.Link href="/">Home</Breadcrumb.Link>
    </Breadcrumb.Item>

    <Breadcrumb.Separator>
      <!-- Custom separator icon -->
      <svg class="size-4" viewBox="0 0 24 24"><path d="M..."/></svg>
    </Breadcrumb.Separator>

    <Breadcrumb.Item>
      <Breadcrumb.Link href="/components">Components</Breadcrumb.Link>
    </Breadcrumb.Item>

    <Breadcrumb.Separator />

    <Breadcrumb.Item>
      <Breadcrumb.Page>Breadcrumb</Breadcrumb.Page>
    </Breadcrumb.Item>
  </Breadcrumb.List>
</Breadcrumb.Root>
```

---

### Dropdown in Breadcrumb

Compose `<Breadcrumb.Item />` with a `<DropdownMenu />` to provide a dropdown for long paths.

```svelte
<script lang="ts">
  import * as Breadcrumb from "$lib/components/ui/breadcrumb/index.js";
  import * as DropdownMenu from "$lib/components/ui/dropdown-menu/index.js";
</script>

<Breadcrumb.Root>
  <Breadcrumb.List>
    <Breadcrumb.Item>
      <Breadcrumb.Link href="/">Home</Breadcrumb.Link>
    </Breadcrumb.Item>

    <Breadcrumb.Separator />

    <Breadcrumb.Item>
      <DropdownMenu.Root>
        <DropdownMenu.Trigger class="flex items-center gap-1">
          <Breadcrumb.Ellipsis class="size-4" />
          <span class="sr-only">Toggle menu</span>
        </DropdownMenu.Trigger>

        <DropdownMenu.Content align="start">
          <DropdownMenu.Item>Documentation</DropdownMenu.Item>
          <DropdownMenu.Item>Themes</DropdownMenu.Item>
          <DropdownMenu.Item>GitHub</DropdownMenu.Item>
        </DropdownMenu.Content>
      </DropdownMenu.Root>
    </Breadcrumb.Item>

    <Breadcrumb.Separator />

    <Breadcrumb.Item>
      <Breadcrumb.Link href="/docs/components">Components</Breadcrumb.Link>
    </Breadcrumb.Item>

    <Breadcrumb.Separator />

    <Breadcrumb.Item>
      <Breadcrumb.Page>Breadcrumb</Breadcrumb.Page>
    </Breadcrumb.Item>
  </Breadcrumb.List>
</Breadcrumb.Root>
```

---

### Collapsed Breadcrumb

Show a collapsed state when the breadcrumb is too long.

```svelte
<Breadcrumb.Root>
  <Breadcrumb.List>
    <Breadcrumb.Item>
      <Breadcrumb.Link href="/">Home</Breadcrumb.Link>
    </Breadcrumb.Item>

    <Breadcrumb.Separator />

    <Breadcrumb.Item>
      <Breadcrumb.Ellipsis />
    </Breadcrumb.Item>

    <Breadcrumb.Separator />

    <Breadcrumb.Item>
      <Breadcrumb.Link href="/components">Components</Breadcrumb.Link>
    </Breadcrumb.Item>

    <Breadcrumb.Separator />

    <Breadcrumb.Item>
      <Breadcrumb.Page>Breadcrumb</Breadcrumb.Page>
    </Breadcrumb.Item>
  </Breadcrumb.List>
</Breadcrumb.Root>
```

---

### Custom Link Component

If you use a routing library that provides a custom `<Link>` component, pass it via the `asChild` prop.

```svelte
<script lang="ts">
  import * as Breadcrumb from "$lib/components/ui/breadcrumb/index.js";
  import { Link } from "svelte-routing";
</script>

<Breadcrumb.Root>
  <Breadcrumb.List>
    <Breadcrumb.Item>
      <Breadcrumb.Link asChild href="/">
        <Link>Home</Link>
      </Breadcrumb.Link>
    </Breadcrumb.Item>

    <Breadcrumb.Separator />

    <Breadcrumb.Item>
      <Breadcrumb.Link asChild href="/components">
        <Link>Components</Link>
      </Breadcrumb.Link>
    </Breadcrumb.Item>

    <Breadcrumb.Separator />

    <Breadcrumb.Item>
      <Breadcrumb.Page>Breadcrumb</Breadcrumb.Page>
    </Breadcrumb.Item>
  </Breadcrumb.List>
</Breadcrumb.Root>
```

---

### Responsive Breadcrumb

Show a dropdown on desktop and a drawer on mobile.

```svelte
<script lang="ts">
  import * as Breadcrumb from "$lib/components/ui/breadcrumb/index.js";
  import * as DropdownMenu from "$lib/components/ui/dropdown-menu/index.js";
  import * as Drawer from "$lib/components/ui/drawer/index.js";
</script>

<Breadcrumb.Root>
  <Breadcrumb.List>
    <Breadcrumb.Item>
      <Breadcrumb.Link href="/">Home</Breadcrumb.Link>
    </Breadcrumb.Item>

    <Breadcrumb.Separator />

    <Breadcrumb.Item>
      <DropdownMenu.Root class="hidden md:flex">
        <DropdownMenu.Trigger class="flex items-center gap-1">
          <Breadcrumb.Ellipsis class="size-4" />
          <span class="sr-only">Toggle menu</span>
        </DropdownMenu.Trigger>

        <DropdownMenu.Content align="start">
          <DropdownMenu.Item>Documentation</DropdownMenu.Item>
          <DropdownMenu.Item>Themes</DropdownMenu.Item>
          <DropdownMenu.Item>GitHub</DropdownMenu.Item>
        </DropdownMenu.Content>
      </DropdownMenu.Root>

      <Drawer.Root class="md:hidden">
        <Drawer.Trigger class="flex items-center gap-1">
          <Breadcrumb.Ellipsis class="size-4" />
          <span class="sr-only">Open drawer</span>
        </Drawer.Trigger>

        <Drawer.Content>
          <Drawer.Header>
            <Drawer.Title>Navigation</Drawer.Title>
          </Drawer.Header>
          <Drawer.Body>
            <ul>
              <li><a href="/docs">Documentation</a></li>
              <li><a href="/themes">Themes</a></li>
              <li><a href="/github">GitHub</a></li>
            </ul>
          </Drawer.Body>
        </Drawer.Content>
      </Drawer.Root>
    </Breadcrumb.Item>

    <Breadcrumb.Separator />

    <Breadcrumb.Item>
      <Breadcrumb.Link href="/components">Components</Breadcrumb.Link>
    </Breadcrumb.Item>

    <Breadcrumb.Separator />

    <Breadcrumb.Item>
      <Breadcrumb.Page>Breadcrumb</Breadcrumb.Page>
    </Breadcrumb.Item>
  </Breadcrumb.List>
</Breadcrumb.Root>
```

---

## API Reference

### `Breadcrumb.Root`

Root wrapper for the breadcrumb.  
No props.

### `Breadcrumb.List`

Container for breadcrumb items.  
No props.

### `Breadcrumb.Item`

Wrapper for each breadcrumb segment.  
No props.

### `Breadcrumb.Link`

Link component.  
| Prop | Type | Description |
|------|------|-------------|
| `href` | `string` | Destination URL. |
| `asChild` | `boolean` | If `true`, renders the child component as the link element. |

### `Breadcrumb.Page`

Displays the current page (non‑link).  
No props.

### `Breadcrumb.Separator`

Separator between items.  
Can contain a custom component via `<slot>`.

### `Breadcrumb.Ellipsis`

Shows a collapsed state (e.g., `…`).  
No props.

---

## Credits

Built by **shadcn**.  
Ported to Svelte by **Huntabyte** & **CokaKoala**.