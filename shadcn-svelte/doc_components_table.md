# Table Component – shadcn‑svelte

The **Table** component is a fully‑responsive, accessible table implementation that follows the shadcn‑svelte design system.  
It is built with Svelte and Tailwind CSS and exposes a small, composable API that mirrors the native `<table>` element.

> **Note** – All examples below use the default export from `$lib/components/ui/table/index.js`.  
> The component names are namespaced (`Table.Root`, `Table.Caption`, …) to keep the global namespace clean.

---

## 1. Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add table
```

> The CLI will add the component files to your project and update the registry.

### Manual

If you prefer to add the component manually, copy the files from the `table` component folder into your `$lib/components/ui/table` directory and import them as shown in the usage section.

---

## 2. Usage

```svelte
<script lang="ts">
  import * as Table from "$lib/components/ui/table/index.js";
</script>

<Table.Root>
  <Table.Caption>A list of your recent invoices.</Table.Caption>

  <Table.Header>
    <Table.Row>
      <Table.Head class="w-[100px]">Invoice</Table.Head>
      <Table.Head>Status</Table.Head>
      <Table.Head>Method</Table.Head>
      <Table.Head class="text-right">Amount</Table.Head>
    </Table.Row>
  </Table.Header>

  <Table.Body>
    <Table.Row>
      <Table.Cell class="font-medium">INV001</Table.Cell>
      <Table.Cell>Paid</Table.Cell>
      <Table.Cell>Credit Card</Table.Cell>
      <Table.Cell class="text-right">$250.00</Table.Cell>
    </Table.Row>
  </Table.Body>
</Table.Root>
```

### Component API

| Component | Purpose | Example Tailwind |
|-----------|---------|------------------|
| `Table.Root` | `<table>` wrapper | – |
| `Table.Caption` | `<caption>` | – |
| `Table.Header` | `<thead>` | – |
| `Table.Row` | `<tr>` | – |
| `Table.Head` | `<th>` | `class="w-[100px]"` |
| `Table.Body` | `<tbody>` | – |
| `Table.Cell` | `<td>` | `class="text-right"` |
| `Table.Footer` | `<tfoot>` | – |

All components accept standard HTML attributes and Tailwind classes.

---

## 3. Full Example – Invoices Table

```svelte
<script lang="ts">
  import * as Table from "$lib/components/ui/table/index.js";

  const invoices = [
    {
      invoice: "INV001",
      paymentStatus: "Paid",
      totalAmount: "$250.00",
      paymentMethod: "Credit Card"
    },
    {
      invoice: "INV002",
      paymentStatus: "Pending",
      totalAmount: "$150.00",
      paymentMethod: "PayPal"
    },
    {
      invoice: "INV003",
      paymentStatus: "Unpaid",
      totalAmount: "$350.00",
      paymentMethod: "Bank Transfer"
    },
    {
      invoice: "INV004",
      paymentStatus: "Paid",
      totalAmount: "$450.00",
      paymentMethod: "Credit Card"
    },
    {
      invoice: "INV005",
      paymentStatus: "Paid",
      totalAmount: "$550.00",
      paymentMethod: "PayPal"
    },
    {
      invoice: "INV006",
      paymentStatus: "Pending",
      totalAmount: "$200.00",
      paymentMethod: "Bank Transfer"
    },
    {
      invoice: "INV007",
      paymentStatus: "Unpaid",
      totalAmount: "$300.00",
      paymentMethod: "Credit Card"
    }
  ];
</script>

<Table.Root>
  <Table.Caption>A list of your recent invoices.</Table.Caption>

  <Table.Header>
    <Table.Row>
      <Table.Head class="w-[100px]">Invoice</Table.Head>
      <Table.Head>Status</Table.Head>
      <Table.Head>Method</Table.Head>
      <Table.Head class="text-right">Amount</Table.Head>
    </Table.Row>
  </Table.Header>

  <Table.Body>
    {#each invoices as invoice (invoice)}
      <Table.Row>
        <Table.Cell class="font-medium">{invoice.invoice}</Table.Cell>
        <Table.Cell>{invoice.paymentStatus}</Table.Cell>
        <Table.Cell>{invoice.paymentMethod}</Table.Cell>
        <Table.Cell class="text-right">{invoice.totalAmount}</Table.Cell>
      </Table.Row>
    {/each}
  </Table.Body>

  <Table.Footer>
    <Table.Row>
      <Table.Cell colspan={3}>Total</Table.Cell>
      <Table.Cell class="text-right">$2,500.00</Table.Cell>
    </Table.Row>
  </Table.Footer>
</Table.Root>
```

### What the example shows

- **Dynamic data** – The table rows are generated with `{#each}`.
- **Semantic structure** – Header, body, and footer are clearly separated.
- **Styling** – Tailwind classes (`w-[100px]`, `text-right`, `font-medium`) are applied directly to the component slots.
- **Accessibility** – The component renders native `<table>` semantics, ensuring screen‑reader friendliness.

---

## 4. Customization

- **Classes** – Pass any Tailwind or custom classes to the component slots.
- **Props** – The underlying components expose the same props as their native HTML counterparts (e.g., `colspan`, `rowspan`).
- **Slots** – You can use Svelte slots inside any component if you need to inject custom markup.

---

## 5. Related Resources

- **Registry** – View all available UI components in the shadcn‑svelte registry.
- **Theming** – Learn how to switch between light/dark themes.
- **CLI Docs** – `pnpm dlx shadcn-svelte@latest add <component>` for quick scaffolding.

---

**Built by shadcn. Ported to Svelte by Huntabyte & CokaKoala.**