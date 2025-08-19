# shadcn‑Svelte – Charts

> A collection of ready‑to‑use chart components built with **LayerChart**.  
> From basic charts to rich data displays, copy and paste into your Svelte apps.

> Built by **shadcn** – ported to Svelte by **Huntabyte** & **CokaKoala**.

---

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Chart Components](#chart-components)
  - [Area Charts](#area-charts)
  - [Bar Charts](#bar-charts)
  - [Line Charts](#line-charts)
  - [Pie Charts](#pie-charts)
  - [Radar Charts](#radar-charts)
  - [Radial Charts](#radial-charts)
  - [Tooltips](#tooltips)
- [Theming](#theming)
- [Colors](#colors)
- [Examples](#examples)

---

## Installation

```bash
npm i shadcn-svelte
# or
yarn add shadcn-svelte
```

Import the global styles in your `app.html` or root component:

```svelte
<script>
  import 'shadcn-svelte/styles.css';
</script>
```

---

## Usage

All chart components are Svelte wrappers around LayerChart.  
They accept the same props as LayerChart, plus a few convenience props.

```svelte
<script>
  import { AreaChart } from 'shadcn-svelte';
  const data = [
    { month: 'Jan', visitors: 1200 },
    { month: 'Feb', visitors: 1500 },
    // …
  ];
</script>

<AreaChart
  data={data}
  xKey="month"
  yKey="visitors"
  title="Monthly Visitors"
  subtitle="January – June 2024"
/>
```

---

## Chart Components

| Chart Type | Description | Example |
|------------|-------------|---------|
| **Area Chart – Interactive** | Shows total visitors for the last 3 months with hover tooltips. | `<AreaChartInteractive data={data} />` |
| **Area Chart – Linear** | Linear area chart for 6‑month data. | `<AreaChartLinear data={data} />` |
| **Area Chart – Step** | Step‑style area chart. | `<AreaChartStep data={data} />` |
| **Area Chart – Legend** | Area chart with a legend. | `<AreaChartLegend data={data} />` |
| **Area Chart – Stacked** | Stacked area chart. | `<AreaChartStacked data={data} />` |
| **Area Chart – Stacked Expanded** | Expanded stacked area chart. | `<AreaChartStackedExpanded data={data} />` |
| **Area Chart – Icons** | Area chart with icons on data points. | `<AreaChartIcons data={data} />` |
| **Area Chart – Gradient** | Gradient‑filled area chart. | `<AreaChartGradient data={data} />` |
| **Area Chart – Axes** | Area chart with custom Y‑axis ticks. | `<AreaChartAxes data={data} />` |
| **Bar Chart** | Standard bar chart. | `<BarChart data={data} />` |
| **Line Chart** | Standard line chart. | `<LineChart data={data} />` |
| **Pie Chart** | Pie chart. | `<PieChart data={data} />` |
| **Radar Chart** | Radar chart. | `<RadarChart data={data} />` |
| **Radial Chart** | Radial chart. | `<RadialChart data={data} />` |
| **Tooltips** | Custom tooltip component. | `<Tooltip content={...} />` |

> **Note:** Replace `data` with your own dataset. The component names above are illustrative; the actual exported names may differ (e.g., `AreaChartInteractive`, `BarChart`, etc.). Check the package documentation for the exact API.

---

## Theming

The library ships with a default theme that follows the shadcn design system.  
You can override colors, fonts, and spacing via CSS variables or by providing a custom theme object.

```svelte
<script>
  import { ThemeProvider } from 'shadcn-svelte';
  const customTheme = {
    colors: {
      primary: '#1e40af',
      secondary: '#64748b',
      // …
    },
    // other theme overrides
  };
</script>

<ThemeProvider theme={customTheme}>
  <!-- Your app -->
</ThemeProvider>
```

---

## Colors

All charts use the same color palette as the rest of the shadcn‑Svelte UI.  
You can customize the palette via the theme provider or by passing a `colors` prop to individual charts.

```svelte
<AreaChart
  data={data}
  colors={['#3b82f6', '#10b981', '#f59e0b']}
/>
```

---

## Examples

Below are minimal, copy‑and‑paste examples for each chart type.  
Replace the placeholder data with your own.

```svelte
<!-- Area Chart – Interactive -->
<script>
  import { AreaChartInteractive } from 'shadcn-svelte';
  const data = [
    { date: '2024-04-07', visitors: 1200 },
    { date: '2024-04-14', visitors: 1500 },
    // …
  ];
</script>

<AreaChartInteractive
  data={data}
  xKey="date"
  yKey="visitors"
  title="Total Visitors (Last 3 Months)"
  subtitle="Trending up by 5.2% this month"
/>
```

```svelte
<!-- Area Chart – Linear -->
<script>
  import { AreaChartLinear } from 'shadcn-svelte';
  const data = [
    { month: 'Jan', visitors: 1200 },
    { month: 'Feb', visitors: 1500 },
    // …
  ];
</script>

<AreaChartLinear
  data={data}
  xKey="month"
  yKey="visitors"
  title="Monthly Visitors (Jan–Jun 2024)"
  subtitle="Trending up by 5.2% this month"
/>
```

```svelte
<!-- Bar Chart -->
<script>
  import { BarChart } from 'shadcn-svelte';
  const data = [
    { category: 'A', value: 30 },
    { category: 'B', value: 45 },
    // …
  ];
</script>

<BarChart
  data={data}
  xKey="category"
  yKey="value"
  title="Category Distribution"
/>
```

```svelte
<!-- Line Chart -->
<script>
  import { LineChart } from 'shadcn-svelte';
  const data = [
    { day: 'Mon', sales: 200 },
    { day: 'Tue', sales: 250 },
    // …
  ];
</script>

<LineChart
  data={data}
  xKey="day"
  yKey="sales"
  title="Daily Sales"
/>
```

```svelte
<!-- Pie Chart -->
<script>
  import { PieChart } from 'shadcn-svelte';
  const data = [
    { label: 'Chrome', value: 55 },
    { label: 'Firefox', value: 25 },
    // …
  ];
</script>

<PieChart
  data={data}
  labelKey="label"
  valueKey="value"
  title="Browser Share"
/>
```

```svelte
<!-- Radar Chart -->
<script>
  import { RadarChart } from 'shadcn-svelte';
  const data = [
    { skill: 'JavaScript', score: 80 },
    { skill: 'Svelte', score: 70 },
    // …
  ];
</script>

<RadarChart
  data={data}
  labelKey="skill"
  valueKey="score"
  title="Skill Radar"
/>
```

```svelte
<!-- Radial Chart -->
<script>
  import { RadialChart } from 'shadcn-svelte';
  const data = [
    { segment: 'A', percent: 40 },
    { segment: 'B', percent: 60 },
    // …
  ];
</script>

<RadialChart
  data={data}
  labelKey="segment"
  valueKey="percent"
  title="Segment Distribution"
/>
```

```svelte
<!-- Tooltips -->
<script>
  import { Tooltip } from 'shadcn-svelte';
</script>

<Tooltip content="This is a custom tooltip" />
```

---

## Contributing

Feel free to open issues or pull requests on the GitHub repository.  
All contributions that improve the documentation, add new chart types, or fix bugs are welcome.

---

## License

MIT © shadcn / Huntabyte / CokaKoala

---