# shadcn‑svelte – Charts

`shadcn‑svelte` ships a lightweight, composable chart system built on top of **LayerChart**.  
The goal is to give you a ready‑to‑use chart component that is fully themeable, supports custom tooltips, legends, and can be extended with your own UI.

> **Tip** – All examples below are fully copy‑paste ready.  
> They use the default `Chart` namespace (`$lib/components/ui/chart/index.js`) and the `layerchart` package.

---

## Table of Contents

| Section | Description |
|---------|-------------|
| [Installation](#installation) | Add the chart component to your project. |
| [Getting Started](#getting-started) | Build your first bar chart. |
| [Chart Config](#chart-config) | Define labels, icons, and colors. |
| [Theming](#theming) | Use CSS variables or raw colors. |
| [Tooltip](#tooltip) | Customise the built‑in tooltip. |
| [Examples](#examples) | Full code snippets for common use‑cases. |

---

## Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add chart
```

> **Supported package managers** – `pnpm`, `npm`, `bun`, `yarn`.

### Manual

```bash
# Install LayerChart
pnpm add layerchart

# Add the chart component
pnpm add @shadcn/svelte
```

---

## Getting Started

Below is a step‑by‑step guide to create a simple stacked bar chart.

### 1. Define your data

```svelte
<script lang="ts">
  const chartData = [
    { month: "January", desktop: 186, mobile: 80 },
    { month: "February", desktop: 305, mobile: 200 },
    { month: "March", desktop: 237, mobile: 120 },
    { month: "April", desktop: 73, mobile: 190 },
    { month: "May", desktop: 209, mobile: 130 },
    { month: "June", desktop: 214, mobile: 140 },
  ];
</script>
```

### 2. Create a chart config

```svelte
<script lang="ts">
  import * as Chart from "$lib/components/ui/chart/index.js";

  const chartConfig = {
    desktop: {
      label: "Desktop",
      color: "#2563eb",
    },
    mobile: {
      label: "Mobile",
      color: "#60a5fa",
    },
  } satisfies Chart.ChartConfig;
</script>
```

> `Chart.ChartConfig` is a TypeScript type that ensures the config matches the data keys.

### 3. Build the chart

```svelte
<script lang="ts">
  import * as Chart from "$lib/components/ui/chart/index.js";
  import { scaleBand } from "d3-scale";
  import { BarChart } from "layerchart";
</script>

<Chart.Container config={chartConfig} class="min-h-[200px] w-full">
  <BarChart
    data={chartData}
    xScale={scaleBand().padding(0.25)}
    x="month"
    axis="x"
    seriesLayout="group"
    series={[
      {
        key: "desktop",
        label: chartConfig.desktop.label,
        color: chartConfig.desktop.color,
      },
      {
        key: "mobile",
        label: chartConfig.mobile.label,
        color: chartConfig.mobile.color,
      },
    ]}
    props={{
      xAxis: { format: (d) => d.slice(0, 3) }, // Jan, Feb, …
    }}
  >
    <!-- Tooltip -->
    <Chart.Tooltip />
  </BarChart>
</Chart.Container>
```

> The `Chart.Container` injects the config into all child components.  
> `BarChart` is a LayerChart component; the rest is pure Svelte.

---

## Chart Config

The config is **decoupled** from the data.  
It lets you share labels, icons, and colors across multiple charts.

```svelte
<script lang="ts">
  import MonitorIcon from "@lucide/svelte/icons/monitor";
  import * as Chart from "$lib/components/ui/chart/index.js";

  const chartConfig = {
    desktop: {
      label: "Desktop",
      icon: MonitorIcon,
      color: "#2563eb",
      theme: { light: "#2563eb", dark: "#dc2626" },
    },
  } satisfies Chart.ChartConfig;
</script>
```

* `label` – Human‑readable name.  
* `icon` – Optional Svelte component.  
* `color` – Any CSS color string or CSS variable.  
* `theme` – Optional object with `light`/`dark` values.

---

## Theming

### CSS Variables (recommended)

```css
/* src/app.css */
:root {
  --chart-1: oklch(0.646 0.222 41.116);
  --chart-2: oklch(0.6 0.118 184.704);
}

.dark {
  --chart-1: oklch(0.488 0.243 264.376);
  --chart-2: oklch(0.696 0.17 162.48);
}
```

```svelte
<script lang="ts">
  const chartConfig = {
    desktop: { label: "Desktop", color: "var(--chart-1)" },
    mobile:  { label: "Mobile",  color: "var(--chart-2)" },
  } satisfies Chart.ChartConfig;
</script>
```

### Direct colors

```svelte
<script lang="ts">
  const chartConfig = {
    desktop: { label: "Desktop", color: "#2563eb" },
  } satisfies Chart.ChartConfig;
</script>
```

---

## Tooltip

`Chart.Tooltip` is a fully‑customisable component that can be dropped into any LayerChart.

### Props

| Prop | Type | Description |
|------|------|-------------|
| `labelKey` | `string` | Key to use for the tooltip label (defaults to config key). |
| `nameKey` | `string` | Key to use for the tooltip name (defaults to data key). |
| `indicator` | `"dot" | "line" | "dashed"` | Style of the indicator. |
| `hideLabel` | `boolean` | Hide the label. |
| `hideIndicator` | `boolean` | Hide the indicator. |
| `label` | `string` | Custom label text. |
| `labelFormatter` | `function` | Format the label. |
| `formatter` | `Snippet` | Render custom tooltip content. |

### Example – Custom keys

```svelte
<script lang="ts">
  const chartData = [
    { browser: "chrome", visitors: 187, color: "var(--color-chrome)" },
    { browser: "safari", visitors: 200, color: "var(--color-safari)" },
  ];

  const chartConfig = {
    visitors: { label: "Total Visitors" },
    chrome:   { label: "Chrome",   color: "var(--chart-1)" },
    safari:   { label: "Safari",   color: "var(--chart-2)" },
  } satisfies Chart.ChartConfig;
</script>

<Chart.Tooltip labelKey="visitors" nameKey="browser" />
```

> This will display “Total Visitors” as the label and “Chrome / Safari” as the names.

---

## Full Example – Stacked Bar Chart with Legend & Tooltip

```svelte
<script lang="ts">
  import * as Chart from "$lib/components/ui/chart/index.js";
  import { scaleBand } from "d3-scale";
  import { BarChart } from "layerchart";

  const chartData = [
    { month: "January", desktop: 186, mobile: 80 },
    { month: "February", desktop: 305, mobile: 200 },
    { month: "March", desktop: 237, mobile: 120 },
    { month: "April", desktop: 73, mobile: 190 },
    { month: "May", desktop: 209, mobile: 130 },
    { month: "June", desktop: 214, mobile: 140 },
  ];

  const chartConfig = {
    desktop: { label: "Desktop", color: "#2563eb" },
    mobile:  { label: "Mobile",  color: "#60a5fa" },
  } satisfies Chart.ChartConfig;
</script>

<Chart.Container config={chartConfig} class="min-h-[200px] w-full">
  <BarChart
    data={chartData}
    xScale={scaleBand().padding(0.25)}
    x="month"
    axis="x"
    seriesLayout="group"
    legend
    series={[
      { key: "desktop", label: chartConfig.desktop.label, color: chartConfig.desktop.color },
      { key: "mobile",  label: chartConfig.mobile.label,  color: chartConfig.mobile.color },
    ]}
    props={{
      xAxis: { format: (d) => d.slice(0, 3) },
    }}
  >
    <Chart.Tooltip />
  </BarChart>
</Chart.Container>
```

---

## FAQ

| Question | Answer |
|----------|--------|
| **Can I use other LayerChart types?** | Yes – replace `<BarChart>` with `<LineChart>`, `<AreaChart>`, etc. |
| **How do I add a custom legend?** | Use the `legend` prop and render your own component inside the chart. |
| **Is the chart responsive?** | The container can be styled with Tailwind or CSS; LayerChart handles resizing. |
| **Can I use Tailwind colors?** | Yes – reference them with `var(--color-…)` or directly in the config. |

---

## Resources

* **LayerChart** – The underlying charting library.  
* **shadcn‑svelte** – UI component library that ships the chart wrapper.  
* **Tailwind CSS** – Recommended for styling the container and layout.  

Happy charting!