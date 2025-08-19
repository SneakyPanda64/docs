# shadcn‑svelte Data Table Documentation

> This guide walks you through building a fully‑featured data table in Svelte 5 using **TanStack Table** and the shadcn‑svelte UI components.  
> All examples are kept verbatim from the original source so you can copy‑paste and run them immediately.

---

## Table of Contents

1. [Introduction](#introduction)
2. [Installation](#installation)
3. [Prerequisites](#prerequisites)
4. [Project Structure](#project-structure)
5. [Basic Table](#basic-table)
   - [Column Definitions](#column-definitions)
   - [`<DataTable />` Component](#datatable-component)
   - [Render the Table](#render-the-table)
6. [Cell Formatting](#cell-formatting)
7. [Row Actions](#row-actions)
   - [Actions Component](#actions-component)
   - [Update Column Definition](#update-column-definition)
8. [Pagination](#pagination)
   - [Add Pagination Controls](#add-pagination-controls)
9. [Sorting](#sorting)
   - [Sortable Email Header](#sortable-email-header)
   - [Update Column Definition](#update-column-definition-1)
10. [Filtering](#filtering)
11. [Visibility](#visibility)
12. [Row Selection](#row-selection)
    - [Checkbox Component](#checkbox-component)
    - [Update Column Definition](#update-column-definition-2)
    - [Show Selected Rows](#show-selected-rows)
13. [Reusable Components](#reusable-components)
14. [Context Menu & Date Picker](#context-menu--date-picker)
15. [Built By](#built-by)

---

## Introduction

Data tables are notoriously hard to componentize because of the wide variety of features they support and the uniqueness of every data set.  
Instead of a one‑size‑fits‑all solution, this guide shows you how to build your own data tables step‑by‑step, starting from a basic table and adding features such as pagination, sorting, filtering, visibility, and row selection.

---

## Installation

Add the **Table** and **Data Table** components to your project:

```bash
# pnpm
pnpm dlx shadcn-svelte@latest add table data-table

# npm
npm i shadcn-svelte@latest
npx shadcn-svelte add table data-table

# bun
bun x shadcn-svelte@latest add table data-table

# yarn
yarn dlx shadcn-svelte@latest add table data-table
```

Add **TanStack Table** as a dependency:

```bash
# pnpm
pnpm i @tanstack/table-core

# npm
npm i @tanstack/table-core

# bun
bun i @tanstack/table-core

# yarn
yarn add @tanstack/table-core
```

---

## Prerequisites

We’ll build a table to show recent payments.  
Here’s the data shape:

```ts
type Payment = {
  id: string;
  amount: number;
  status: "pending" | "processing" | "success" | "failed";
  email: string;
};

export const data: Payment[] = [
  { id: "728ed52f", amount: 100, status: "pending", email: "m@example.com" },
  { id: "489e1d42", amount: 125, status: "processing", email: "example@gmail.com" },
  // …
];
```

---

## Project Structure

Create a route where your data table will live (e.g. `payments`) and add the following files:

```
routes
└── payments
    ├── columns.ts
    ├── data-table.svelte
    ├── data-table-actions.svelte
    ├── data-table-checkbox.svelte
    ├── data-table-email-button.svelte
    └── +page.svelte
```

- **`columns.ts`** – column definitions  
- **`data-table.svelte`** – `<Table />` + `<DataTable />` component  
- **`data-table-actions.svelte`** – actions menu for each row  
- **`data-table-checkbox.svelte`** – checkbox for each row  
- **`data-table-email-button.svelte`** – sortable email header button  
- **`+page.svelte`** – where you render `<DataTable />`

---

## Basic Table

### Column Definitions

```ts
// routes/payments/columns.ts
import type { ColumnDef } from "@tanstack/table-core";

export type Payment = {
  id: string;
  amount: number;
  status: "pending" | "processing" | "success" | "failed";
  email: string;
};

export const columns: ColumnDef<Payment>[] = [
  { accessorKey: "status", header: "Status" },
  { accessorKey: "email", header: "Email" },
  { accessorKey: "amount", header: "Amount" },
];
```

### `<DataTable />` Component

```svelte
<!-- routes/payments/data-table.svelte -->
<script lang="ts" generics="TData, TValue">
  import { type ColumnDef, getCoreRowModel } from "@tanstack/table-core";
  import { createSvelteTable, FlexRender } from "$lib/components/ui/data-table/index.js";
  import * as Table from "$lib/components/ui/table/index.js";

  type DataTableProps<TData, TValue> = {
    columns: ColumnDef<TData, TValue>[];
    data: TData[];
  };

  let { data, columns }: DataTableProps<TData, TValue> = $props();

  const table = createSvelteTable({
    get data() { return data; },
    columns,
    getCoreRowModel: getCoreRowModel(),
  });
</script>

<div class="rounded-md border">
  <Table.Root>
    <Table.Header>
      {#each table.getHeaderGroups() as headerGroup (headerGroup.id)}
        <Table.Row>
          {#each headerGroup.headers as header (header.id)}
            <Table.Head colspan={header.colSpan}>
              {#if !header.isPlaceholder}
                <FlexRender
                  content={header.column.columnDef.header}
                  context={header.getContext()}
                />
              {/if}
            </Table.Head>
          {/each}
        </Table.Row>
      {/each}
    </Table.Header>

    <Table.Body>
      {#each table.getRowModel().rows as row (row.id)}
        <Table.Row data-state={row.getIsSelected() && "selected"}>
          {#each row.getVisibleCells() as cell (cell.id)}
            <Table.Cell>
              <FlexRender
                content={cell.column.columnDef.cell}
                context={cell.getContext()}
              />
            </Table.Cell>
          {/each}
        </Table.Row>
      {:else}
        <Table.Row>
          <Table.Cell colspan={columns.length} class="h-24 text-center">
            No results.
          </Table.Cell>
        </Table.Row>
      {/each}
    </Table.Body>
  </Table.Root>
</div>
```

> **Tip** – If you use `<DataTable />` in multiple places, extract it into a reusable component (`components/ui/data-table.svelte`).

### Render the Table

```svelte
<!-- routes/payments/+page.svelte -->
<script lang="ts">
  import DataTable from "./data-table.svelte";
  import { columns } from "./columns.js";

  let { data } = $props();
</script>

<DataTable data={data.payments} {columns} />
```

---

## Cell Formatting

Format the amount cell to display a dollar amount and align it to the right.

```ts
// routes/payments/columns.ts
import type { ColumnDef } from "@tanstack/table-core";
import { createRawSnippet } from "svelte";
import { renderSnippet } from "$lib/components/ui/data-table/index.js";

export const columns: ColumnDef<Payment>[] = [
  {
    accessorKey: "amount",
    header: () => {
      const amountHeaderSnippet = createRawSnippet(() => ({
        render: () => `<div class="text-right">Amount</div>`,
      }));
      return renderSnippet(amountHeaderSnippet, "");
    },
    cell: ({ row }) => {
      const formatter = new Intl.NumberFormat("en-US", {
        style: "currency",
        currency: "USD",
      });

      const amountCellSnippet = createRawSnippet<[string]>((getAmount) => {
        const amount = getAmount();
        return {
          render: () => `<div class="text-right font-medium">${amount}</div>`,
        };
      });

      return renderSnippet(
        amountCellSnippet,
        formatter.format(parseFloat(row.getValue("amount")))
      );
    },
  },
];
```

> We use `createRawSnippet` to render simple HTML without a full component lifecycle.

---

## Row Actions

### Actions Component

```svelte
<!-- routes/payments/data-table-actions.svelte -->
<script lang="ts">
  import EllipsisIcon from "@lucide/svelte/icons/ellipsis";
  import { Button } from "$lib/components/ui/button/index.js";
  import * as DropdownMenu from "$lib/components/ui/dropdown-menu/index.js";

  let { id }: { id: string } = $props();
</script>

<DropdownMenu.Root>
  <DropdownMenu.Trigger>
    {#snippet child({ props })}
      <Button
        {...props}
        variant="ghost"
        size="icon"
        class="relative size-8 p-0"
      >
        <span class="sr-only">Open menu</span>
        <EllipsisIcon />
      </Button>
    {/snippet}
  </DropdownMenu.Trigger>

  <DropdownMenu.Content>
    <DropdownMenu.Group>
      <DropdownMenu.Label>Actions</DropdownMenu.Label>
      <DropdownMenu.Item onclick={() => navigator.clipboard.writeText(id)}>
        Copy payment ID
      </DropdownMenu.Item>
    </DropdownMenu.Group>

    <DropdownMenu.Separator />

    <DropdownMenu.Item>View customer</DropdownMenu.Item>
    <DropdownMenu.Item>View payment details</DropdownMenu.Item>
  </DropdownMenu.Content>
</DropdownMenu.Root>
```

### Update Column Definition

```ts
// routes/payments/columns.ts
import type { ColumnDef } from "@tanstack/table-core";
import { renderComponent } from "$lib/components/ui/data-table/index.js";
import DataTableActions from "./data-table-actions.svelte";

export const columns: ColumnDef<Payment>[] = [
  // …
  {
    id: "actions",
    cell: ({ row }) => {
      return renderComponent(DataTableActions, { id: row.original.id });
    },
  },
];
```

---

## Pagination

### Add Pagination State

```svelte
<!-- routes/payments/data-table.svelte -->
<script lang="ts" generics="TData, TValue">
  import {
    type ColumnDef,
    type PaginationState,
    getCoreRowModel,
    getPaginationRowModel,
  } from "@tanstack/table-core";
  import { createSvelteTable, FlexRender } from "$lib/components/ui/data-table/index.js";
  import * as Table from "$lib/components/ui/table/index.js";
  import { Button } from "$lib/components/ui/button/index.js";

  type DataTableProps<TData, TValue> = {
    data: TData[];
    columns: ColumnDef<TData, TValue>[];
  };

  let { data, columns }: DataTableProps<TData, TValue> = $props();

  let pagination = $state<PaginationState>({ pageIndex: 0, pageSize: 10 });

  const table = createSvelteTable({
    get data() { return data; },
    columns,
    state: { get pagination() { return pagination; } },
    onPaginationChange: (updater) => {
      pagination = typeof updater === "function" ? updater(pagination) : updater;
    },
    getCoreRowModel: getCoreRowModel(),
    getPaginationRowModel: getPaginationRowModel(),
  });
</script>

<div>
  <div class="rounded-md border">
    <Table.Root>
      <!-- table implementation -->
    </Table.Root>
  </div>

  <div class="flex items-center justify-end space-x-2 py-4">
    <Button
      variant="outline"
      size="sm"
      onclick={() => table.previousPage()}
      disabled={!table.getCanPreviousPage()}
    >
      Previous
    </Button>
    <Button
      variant="outline"
      size="sm"
      onclick={() => table.nextPage()}
      disabled={!table.getCanNextPage()}
    >
      Next
    </Button>
  </div>
</div>
```

---

## Sorting

### Sortable Email Header

```svelte
<!-- routes/payments/data-table-email-button.svelte -->
<script lang="ts">
  import type { ComponentProps } from "svelte";
  import ArrowUpDownIcon from "@lucide/svelte/icons/arrow-up-down";
  import { Button } from "$lib/components/ui/button/index.js";

  let { variant = "