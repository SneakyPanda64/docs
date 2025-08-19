# shadcn‑svelte – Building Blocks for the Web

> **Clean, modern UI components for Svelte**  
> Works with any Svelte project.  
> Open‑source, free forever.

---

## Table of Contents

1. [Getting Started](#getting-started)  
2. [Adding a Block](#adding-a-block)  
3. [Example: Dashboard‑01](#example-dashboard-01)  
4. [Other Available Blocks](#other-available-blocks)  
5. [License](#license)

---

## 1. Getting Started

```bash
# Install the CLI (if you haven't already)
pnpm dlx shadcn-svelte@latest

# Add a block to your project
pnpm dlx shadcn-svelte@latest add <block-name>
```

> **Tip** – The CLI will automatically copy the necessary files into your project and update your routing.

---

## 2. Adding a Block

The `shadcn-svelte` CLI lets you add pre‑built UI blocks with a single command.  
Example:

```bash
pnpm dlx shadcn-svelte@latest add dashboard-01
```

This command will:

- Create a new route (`routes/dashboard-01/+page.svelte`).
- Add the required component files under `lib/components`.
- Generate any supporting data or schema files.

---

## 3. Example: Dashboard‑01

Below is the full content of the generated `+page.svelte` for the **Dashboard‑01** block.  
Feel free to copy‑paste it into your own project or modify it as needed.

```svelte
<script lang="ts">
  import data from "./data.js";
  import * as Sidebar from "$lib/components/ui/sidebar/index.js";
  import AppSidebar from "$lib/components/app-sidebar.svelte";
  import SiteHeader from "$lib/components/site-header.svelte";
  import SectionCards from "$lib/components/section-cards.svelte";
  import ChartAreaInteractive from "$lib/components/chart-area-interactive.svelte";
  import DataTable from "$lib/components/data-table.svelte";
</script>

<Sidebar.Provider
  style="--sidebar-width: calc(var(--spacing) * 72); --header-height: calc(var(--spacing) * 12);"
>
  <AppSidebar variant="inset" />
  <Sidebar.Inset>
    <SiteHeader />
    <div class="flex flex-1 flex-col">
      <div class="@container/main flex flex-1 flex-col gap-2">
        <div class="flex flex-col gap-4 py-4 md:gap-6 md:py-6">
          <SectionCards />
          <div class="px-4 lg:px-6">
            <ChartAreaInteractive />
          </div>
          <DataTable {data} />
        </div>
      </div>
    </div>
  </Sidebar.Inset>
</Sidebar.Provider>
```

### What the Dashboard‑01 Block Includes

| Component | Purpose |
|-----------|---------|
| `AppSidebar` | Collapsible sidebar with navigation |
| `SiteHeader` | Top header bar |
| `SectionCards` | Overview cards (e.g., stats, quick links) |
| `ChartAreaInteractive` | Interactive area chart |
| `DataTable` | Paginated, sortable data table |

---

## 4. Other Available Blocks

You can add any of the following blocks with the same CLI command.  
All examples below are ready‑to‑copy code snippets.

### 4.1 Sidebar – Collapses to Icons

```bash
pnpm dlx shadcn-svelte@latest add sidebar-07
```

> *Preview* – A sidebar that collapses to icons for a minimal UI.

---

### 4.2 Sidebar – With Submenus

```bash
pnpm dlx shadcn-svelte@latest add sidebar-03
```

> *Preview* – A sidebar that supports nested submenus.

---

### 4.3 Login Page – Background Image

```bash
pnpm dlx shadcn-svelte@latest add login-03
```

> *Preview* – A login page featuring a full‑screen background image.

---

### 4.4 Login Page – Form & Image

```bash
pnpm dlx shadcn-svelte@latest add login-04
```

> *Preview* – A login page with a form and a side image.

---

## 5. License

`shadcn-svelte` is open‑source and free to use.  
All components are licensed under the MIT license.

---

**Enjoy building modern UIs with Svelte!**