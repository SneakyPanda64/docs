# shadcn‑svelte – UI Component Library for Svelte

> A collection of fully‑styled, accessible UI components built with Tailwind CSS and Svelte.  
> Inspired by shadcn‑ui (React) and ported to Svelte by Huntabyte & CokaKoala.

---

## Table of Contents

- [Getting Started](#getting-started)
  - [Installation](#installation)
  - [Manual Installation](#manual-installation)
  - [CLI](#cli)
- [Theming & Dark Mode](#theming--dark-mode)
- [Component Registry](#component-registry)
- [Usage](#usage)
  - [Pagination Example](#pagination-example)
- [API Reference](#api-reference)
- [Contributing](#contributing)
- [License](#license)

---

## Getting Started

### Installation

```bash
# Using pnpm
pnpm add shadcn-svelte

# Using npm
npm install shadcn-svelte

# Using bun
bun add shadcn-svelte

# Using yarn
yarn add shadcn-svelte
```

> **Tip:** If you only need a single component, you can add it via the CLI:

```bash
pnpm dlx shadcn-svelte@latest add pagination
```

### Manual Installation

1. Install the package as shown above.  
2. Import the component(s) you need:

```svelte
<script lang="ts">
  import * as Pagination from "$lib/components/ui/pagination/index.js";
</script>
```

### CLI

The CLI can scaffold components, update the registry, and more.  
Run `pnpm dlx shadcn-svelte@latest --help` for a full list of commands.

---

## Theming & Dark Mode

- All components are styled with Tailwind CSS.  
- Dark mode is enabled by default via the `dark:` variant.  
- You can customize colors, spacing, and other Tailwind utilities in your `tailwind.config.js`.

```js
// tailwind.config.js
module.exports = {
  darkMode: 'class', // or 'media'
  theme: {
    extend: {
      colors: {
        primary: '#3b82f6',
        secondary: '#6b7280',
      },
    },
  },
};
```

---

## Component Registry

Below is a quick reference of the available components.  
Each component lives under `$lib/components/ui/<component-name>`.

| Component | Description |
|-----------|-------------|
| Accordion | Collapsible panels |
| Alert | Notification alerts |
| Avatar | User avatars |
| Badge | Small status indicators |
| Breadcrumb | Navigation breadcrumbs |
| Button | Primary action button |
| Calendar | Date picker |
| Card | Content container |
| Carousel | Image carousel |
| Chart | Data visualizations |
| Checkbox | Form checkbox |
| Collapsible | Expandable content |
| Combobox | Autocomplete input |
| Command | Command palette |
| Context Menu | Right‑click menu |
| Data Table | Tabular data |
| Date Picker | Date selection |
| Dialog | Modal dialog |
| Drawer | Side panel |
| Dropdown Menu | Dropdown list |
| Formsnap | Form snapshot |
| Hover Card | Hover‑activated card |
| Input | Text input |
| Label | Form label |
| Menubar | Application menubar |
| Navigation Menu | Navigation menu |
| Pagination | Page navigation |
| Popover | Popover panel |
| Progress | Progress bar |
| Radio Group | Radio buttons |
| Range Calendar | Date range picker |
| Resizable | Resizable panels |
| Scroll Area | Custom scroll area |
| Select | Dropdown select |
| Separator | Divider |
| Sheet | Bottom sheet |
| Sidebar | Sidebar navigation |
| Skeleton | Loading skeleton |
| Slider | Range slider |
| Sonner | Toast notifications |
| Switch | Toggle switch |
| Table | Data table |
| Tabs | Tabbed interface |
| Textarea | Multi‑line input |
| Toggle Group | Grouped toggles |
| Toggle | Single toggle |
| Tooltip | Hover tooltip |
| Typography | Text styles |

---

## Usage

### Pagination Example

Below is a complete example of a responsive pagination component that adapts to desktop and mobile viewports.

```svelte
<script lang="ts">
  import ChevronLeftIcon from "@lucide/svelte/icons/chevron-left";
  import ChevronRightIcon from "@lucide/svelte/icons/chevron-right";
  import { MediaQuery } from "svelte/reactivity";
  import * as Pagination from "$lib/components/ui/pagination/index.js";

  const isDesktop = new MediaQuery("(min-width: 768px)");

  const count = 20; // total number of items

  // Items per page: 3 on desktop, 8 on mobile
  const perPage = $derived(isDesktop.current ? 3 : 8);

  // Number of sibling pages to show: 1 on desktop, 0 on mobile
  const siblingCount = $derived(isDesktop.current ? 1 : 0);
</script>

<Pagination.Root {count} {perPage} {siblingCount}>
  {#snippet children({ pages, currentPage })}
    <Pagination.Content>
      <Pagination.Item>
        <Pagination.PrevButton>
          <ChevronLeftIcon class="size-4" />
          <span class="hidden sm:block">Previous</span>
        </Pagination.PrevButton>
      </Pagination.Item>

      {#each pages as page (page.key)}
        {#if page.type === "ellipsis"}
          <Pagination.Item>
            <Pagination.Ellipsis />
          </Pagination.Item>
        {:else}
          <Pagination.Item>
            <Pagination.Link {page} isActive={currentPage === page.value}>
              {page.value}
            </Pagination.Link>
          </Pagination.Item>
        {/if}
      {/each}

      <Pagination.Item>
        <Pagination.NextButton>
          <span class="hidden sm:block">Next</span>
          <ChevronRightIcon class="size-4" />
        </Pagination.NextButton>
      </Pagination.Item>
    </Pagination.Content>
  {/snippet}
</Pagination.Root>
```

#### Minimal Usage

If you prefer the default styling without custom icons:

```svelte
<script lang="ts">
  import * as Pagination from "$lib/components/ui/pagination/index.js";
</script>

<Pagination.Root count={100} perPage={10}>
  {#snippet children({ pages, currentPage })}
    <Pagination.Content>
      <Pagination.Item>
        <Pagination.PrevButton />
      </Pagination.Item>

      {#each pages as page (page.key)}
        {#if page.type === "ellipsis"}
          <Pagination.Item>
            <Pagination.Ellipsis />
          </Pagination.Item>
        {:else}
          <Pagination.Item>
            <Pagination.Link {page} isActive={currentPage === page.value}>
              {page.value}
            </Pagination.Link>
          </Pagination.Item>
        {/if}
      {/each}

      <Pagination.Item>
        <Pagination.NextButton />
      </Pagination.Item>
    </Pagination.Content>
  {/snippet}
</Pagination.Root>
```

---

## API Reference

Each component exposes a set of Svelte components and props.  
For a full API, refer to the source files under `$lib/components/ui/<component-name>`.

Example: **Pagination**

| Component | Props | Description |
|-----------|-------|-------------|
| `<Pagination.Root>` | `count`, `perPage`, `siblingCount` | Root wrapper that calculates pagination logic. |
| `<Pagination.Content>` | — | Container for pagination items. |
| `<Pagination.Item>` | — | Wrapper for individual pagination controls. |
| `<Pagination.PrevButton>` | — | Button to go to the previous page. |
| `<Pagination.NextButton>` | — | Button to go to the next page. |
| `<Pagination.Link>` | `page`, `isActive` | Link to a specific page. |
| `<Pagination.Ellipsis>` | — | Ellipsis indicator for truncated page ranges. |

---

## Contributing

Contributions are welcome!  
Please read the [CONTRIBUTING.md](CONTRIBUTING.md) guide for details on how to submit PRs, report bugs, or suggest new features.

---

## License

MIT © shadcn‑svelte contributors.  
See the [LICENSE](LICENSE) file for details.