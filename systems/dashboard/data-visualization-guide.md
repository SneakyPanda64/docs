# Data Visualization Guide for Dashboards

This comprehensive guide covers advanced data visualization techniques for building interactive, performant, and beautiful charts in dashboard applications. Based on production patterns using Layerchart and D3.js with SvelteKit.

## Table of Contents

1. [Overview](#overview)
2. [Technology Stack](#technology-stack)
3. [Chart Types](#chart-types)
4. [Advanced Area Charts](#advanced-area-charts)
5. [Time Series Visualization](#time-series-visualization)
6. [Real-time Updates](#real-time-updates)
7. [Interactive Features](#interactive-features)
8. [Performance Optimization](#performance-optimization)
9. [Responsive Design](#responsive-design)
10. [Theming & Customization](#theming--customization)
11. [Animation Patterns](#animation-patterns)
12. [Best Practices](#best-practices)

## Overview

Modern dashboards require sophisticated data visualization that is both informative and performant. This guide covers patterns for creating production-ready charts with:

- Multiple chart types (area, line, bar, scatter, pie)
- Time-based filtering and aggregation
- Interactive tooltips and legends
- Smooth animations and transitions
- Responsive design for all screen sizes
- Theme-aware color schemes
- Real-time data updates

## Technology Stack

### Core Dependencies

```json
{
	"layerchart": "^0.x",
	"d3-scale": "^4.x",
	"d3-shape": "^3.x",
	"d3-array": "^3.x",
	"d3-time": "^3.x",
	"d3-format": "^3.x",
	"dayjs": "^1.x"
}
```

### Why Layerchart?

- Built specifically for Svelte
- Declarative API
- Excellent performance
- Built-in animations
- Flexible and composable
- TypeScript support

## Chart Types

### Area Chart

Best for showing trends over time with volume emphasis.

```svelte
<script lang="ts">
	import { Area, AreaChart } from 'layerchart';
	import { scaleUtc, scaleLinear } from 'd3-scale';
	import { curveMonotoneX } from 'd3-shape';

	interface DataPoint {
		date: Date;
		value: number;
	}

	let { data }: { data: DataPoint[] } = $props();
</script>

<AreaChart
	{data}
	x="date"
	y="value"
	xScale={scaleUtc()}
	yScale={scaleLinear()}
	props={{
		area: {
			curve: curveMonotoneX,
			'fill-opacity': 0.3,
			fill: '#3b82f6',
			line: {
				stroke: '#3b82f6',
				'stroke-width': 2
			}
		}
	}}
/>
```

### Line Chart

Ideal for comparing multiple series or showing precise values.

```svelte
<script lang="ts">
	import { Line, LineChart } from 'layerchart';
	import { scaleUtc, scaleLinear } from 'd3-scale';

	interface DataPoint {
		date: Date;
		revenue: number;
		profit: number;
		expenses: number;
	}

	let { data }: { data: DataPoint[] } = $props();

	const series = [
		{ key: 'revenue', label: 'Revenue', color: '#3b82f6' },
		{ key: 'profit', label: 'Profit', color: '#10b981' },
		{ key: 'expenses', label: 'Expenses', color: '#ef4444' }
	];
</script>

<LineChart
	{data}
	x="date"
	xScale={scaleUtc()}
	yScale={scaleLinear()}
	{series}
	props={{
		line: {
			'stroke-width': 2,
			'stroke-linecap': 'round',
			'stroke-linejoin': 'round'
		}
	}}
/>
```

### Bar Chart

Perfect for categorical comparisons.

```svelte
<script lang="ts">
	import { Bar, BarChart } from 'layerchart';
	import { scaleBand, scaleLinear } from 'd3-scale';

	interface CategoryData {
		category: string;
		value: number;
	}

	let { data }: { data: CategoryData[] } = $props();
</script>

<BarChart
	{data}
	x="category"
	y="value"
	xScale={scaleBand().padding(0.1)}
	yScale={scaleLinear()}
	props={{
		bar: {
			fill: '#3b82f6',
			'fill-opacity': 0.8,
			rx: 4,
			ry: 4
		}
	}}
/>
```

### Scatter Plot

Excellent for showing correlations between variables.

```svelte
<script lang="ts">
	import { Circle, ScatterChart } from 'layerchart';
	import { scaleLinear } from 'd3-scale';

	interface DataPoint {
		x: number;
		y: number;
		size: number;
		category: string;
	}

	let { data }: { data: DataPoint[] } = $props();

	const colorScale = {
		A: '#3b82f6',
		B: '#10b981',
		C: '#f59e0b'
	};
</script>

<ScatterChart {data} x="x" y="y" xScale={scaleLinear()} yScale={scaleLinear()}>
	{#snippet marks({ data })}
		{#each data as d}
			<Circle
				cx={xScale(d.x)}
				cy={yScale(d.y)}
				r={Math.sqrt(d.size) * 2}
				fill={colorScale[d.category]}
				fill-opacity={0.6}
				stroke={colorScale[d.category]}
				stroke-width={2}
			/>
		{/each}
	{/snippet}
</ScatterChart>
```

## Advanced Area Charts

### Stacked Area Chart with Gradients

```svelte
<script lang="ts">
	import { Area, AreaChart } from 'layerchart';
	import { scaleUtc, scaleLinear } from 'd3-scale';
	import { curveMonotoneX } from 'd3-shape';
	import { stack } from 'd3-shape';

	interface TimeSeriesData {
		date: Date;
		series1: number;
		series2: number;
		series3: number;
		series4: number;
		series5: number;
	}

	let { data }: { data: TimeSeriesData[] } = $props();

	const chartConfig = {
		series1: { label: 'Series 1', color: '#3b82f6' },
		series2: { label: 'Series 2', color: '#10b981' },
		series3: { label: 'Series 3', color: '#f59e0b' },
		series4: { label: 'Series 4', color: '#ef4444' },
		series5: { label: 'Series 5', color: '#8b5cf6' }
	};

	const series = Object.entries(chartConfig).map(([key, config]) => ({
		key,
		...config
	}));
</script>

<AreaChart
	{data}
	x="date"
	xScale={scaleUtc()}
	yScale={scaleLinear()}
	{series}
	seriesLayout="stack"
	props={{
		area: {
			curve: curveMonotoneX,
			'fill-opacity': 0.6,
			line: { class: 'stroke-2' },
			motion: {
				initial: { opacity: 0, y: 20 },
				animate: { opacity: 1, y: 0 },
				transition: { duration: 0.5, ease: 'easeOut' }
			}
		}
	}}
>
	{#snippet marks({ series, getAreaProps })}
		<defs>
			{#each series as s}
				<linearGradient id="gradient-{s.key}" x1="0" y1="0" x2="0" y2="1">
					<stop offset="5%" stop-color={s.color} stop-opacity={0.9} />
					<stop offset="95%" stop-color={s.color} stop-opacity={0.3} />
				</linearGradient>
			{/each}
		</defs>

		{#each series as s, i}
			<Area {...getAreaProps(s, i)} fill="url(#gradient-{s.key})" />
		{/each}
	{/snippet}
</AreaChart>
```

### Streamgraph

For showing flow and proportion changes over time.

```svelte
<script lang="ts">
	import { Area, AreaChart } from 'layerchart';
	import { scaleUtc, scaleLinear } from 'd3-scale';
	import { curveMonotoneX, stackOffsetWiggle } from 'd3-shape';

	let { data, series }: { data: any[]; series: any[] } = $props();
</script>

<AreaChart
	{data}
	x="date"
	xScale={scaleUtc()}
	yScale={scaleLinear()}
	{series}
	seriesLayout="stack"
	stackOffset={stackOffsetWiggle}
	props={{
		area: {
			curve: curveMonotoneX,
			'fill-opacity': 0.8,
			motion: 'tween'
		}
	}}
/>
```

## Time Series Visualization

### Time Range Selector

```svelte
<script lang="ts">
	import * as ToggleGroup from '$lib/components/ui/toggle-group';
	import * as Select from '$lib/components/ui/select';
	import dayjs from 'dayjs';

	let timeRange = $state('30d');

	const timeRanges = [
		{ value: '1h', label: 'Last Hour', subtract: { hours: 1 } },
		{ value: '24h', label: 'Last 24 Hours', subtract: { hours: 24 } },
		{ value: '7d', label: 'Last 7 Days', subtract: { days: 7 } },
		{ value: '30d', label: 'Last 30 Days', subtract: { days: 30 } },
		{ value: '90d', label: 'Last 90 Days', subtract: { days: 90 } },
		{ value: '1y', label: 'Last Year', subtract: { years: 1 } },
		{ value: 'ytd', label: 'Year to Date', custom: true },
		{ value: 'all', label: 'All Time', custom: true }
	];

	const getDateRange = (range: string) => {
		const now = dayjs();
		const rangeConfig = timeRanges.find((r) => r.value === range);

		if (!rangeConfig) return { start: now.subtract(30, 'day'), end: now };

		if (rangeConfig.custom) {
			switch (range) {
				case 'ytd':
					return { start: now.startOf('year'), end: now };
				case 'all':
					return { start: null, end: now };
				default:
					return { start: now.subtract(30, 'day'), end: now };
			}
		}

		return {
			start: now.subtract(
				rangeConfig.subtract[Object.keys(rangeConfig.subtract)[0]],
				Object.keys(rangeConfig.subtract)[0]
			),
			end: now
		};
	};

	const dateRange = $derived(getDateRange(timeRange));
</script>

<!-- Desktop toggle group -->
<ToggleGroup.Root type="single" bind:value={timeRange} variant="outline" class="hidden lg:flex">
	{#each timeRanges.slice(0, 6) as range}
		<ToggleGroup.Item value={range.value}>{range.label}</ToggleGroup.Item>
	{/each}
</ToggleGroup.Root>

<!-- Mobile/Extended select -->
<Select.Root type="single" bind:value={timeRange}>
	<Select.Trigger class="w-40">
		{timeRanges.find((r) => r.value === timeRange)?.label}
	</Select.Trigger>
	<Select.Content>
		{#each timeRanges as range}
			<Select.Item value={range.value}>{range.label}</Select.Item>
		{/each}
	</Select.Content>
</Select.Root>
```

### Time Aggregation

```typescript
// utils/timeAggregation.ts
import { timeDay, timeWeek, timeMonth, timeYear } from 'd3-time';
import { group, rollup, sum, mean } from 'd3-array';

export function aggregateByTime(
	data: any[],
	dateField: string,
	valueField: string,
	interval: 'day' | 'week' | 'month' | 'year',
	aggregation: 'sum' | 'mean' | 'count' = 'sum'
) {
	const intervalFn = {
		day: timeDay,
		week: timeWeek,
		month: timeMonth,
		year: timeYear
	}[interval];

	const aggregateFn = {
		sum: (values: number[]) => sum(values),
		mean: (values: number[]) => mean(values),
		count: (values: any[]) => values.length
	}[aggregation];

	return rollup(
		data,
		(v) => aggregateFn(v.map((d) => d[valueField])),
		(d) => intervalFn(d[dateField])
	);
}
```

## Real-time Updates

### Live Data Chart

```svelte
<script lang="ts">
	import { onMount, onDestroy } from 'svelte';
	import { LineChart } from 'layerchart';
	import { scaleUtc, scaleLinear } from 'd3-scale';
	import { extent } from 'd3-array';

	interface DataPoint {
		timestamp: Date;
		value: number;
	}

	let data = $state<DataPoint[]>([]);
	let maxPoints = 100;
	let updateInterval: NodeJS.Timeout;

	// Simulate real-time data
	function addDataPoint() {
		const newPoint = {
			timestamp: new Date(),
			value: Math.random() * 100 + Math.sin(Date.now() / 1000) * 20
		};

		data = [...data.slice(-(maxPoints - 1)), newPoint];
	}

	onMount(() => {
		// Initial data
		const now = Date.now();
		data = Array.from({ length: 50 }, (_, i) => ({
			timestamp: new Date(now - (50 - i) * 1000),
			value: Math.random() * 100
		}));

		// Start updates
		updateInterval = setInterval(addDataPoint, 1000);
	});

	onDestroy(() => {
		clearInterval(updateInterval);
	});

	// Dynamic scales
	const xDomain = $derived(extent(data, (d) => d.timestamp));
	const yDomain = $derived([0, Math.max(...data.map((d) => d.value)) * 1.1]);
</script>

<LineChart
	{data}
	x="timestamp"
	y="value"
	xScale={scaleUtc().domain(xDomain)}
	yScale={scaleLinear().domain(yDomain)}
	props={{
		line: {
			stroke: '#3b82f6',
			'stroke-width': 2,
			motion: {
				initial: false,
				animate: { transition: { duration: 1000 } }
			}
		},
		xAxis: {
			format: (d) => d.toLocaleTimeString()
		}
	}}
/>
```

### WebSocket Integration

```svelte
<script lang="ts">
	import { onMount, onDestroy } from 'svelte';
	import { browser } from '$app/environment';

	let ws: WebSocket;
	let data = $state<any[]>([]);
	let status = $state<'connecting' | 'connected' | 'disconnected'>('connecting');

	onMount(() => {
		if (!browser) return;

		ws = new WebSocket('wss://api.example.com/live-data');

		ws.onopen = () => {
			status = 'connected';
		};

		ws.onmessage = (event) => {
			const newData = JSON.parse(event.data);
			data = [...data.slice(-99), newData];
		};

		ws.onerror = (error) => {
			console.error('WebSocket error:', error);
			status = 'disconnected';
		};

		ws.onclose = () => {
			status = 'disconnected';
		};
	});

	onDestroy(() => {
		ws?.close();
	});
</script>

<div class="relative">
	{#if status !== 'connected'}
		<div class="absolute inset-0 z-10 flex items-center justify-center bg-background/80">
			<div class="text-center">
				<div
					class="mb-2 h-8 w-8 animate-spin rounded-full border-4 border-muted border-t-primary"
				/>
				<p class="text-sm text-muted-foreground">{status}...</p>
			</div>
		</div>
	{/if}

	<!-- Chart component here -->
</div>
```

## Interactive Features

### Advanced Tooltips

```svelte
<script lang="ts">
  import { Chart } from '$lib/components/ui/chart';
  import { format } from 'd3-format';
  import { timeFormat } from 'd3-time-format';

  const currencyFormat = format('$,.0f');
  const percentFormat = format('.1%');
  const dateFormat = timeFormat('%b %d, %Y');
</script>

<Chart.Container>
  <AreaChart {data} {series}>
    {#snippet tooltip()}
      <Chart.Tooltip
        labelFormatter={(value: Date) => dateFormat(value)}
        indicator="line"
        content={({ label, payload }) => (
          <div class="rounded-lg border bg-popover p-3 shadow-lg">
            <p class="mb-2 font-medium">{label}</p>
            <div class="space-y-1">
              {#each payload as item}
                <div class="flex items-center justify-between gap-4">
                  <div class="flex items-center gap-2">
                    <div
                      class="h-3 w-3 rounded-full"
                      style="background-color: {item.color}"
                    />
                    <span class="text-sm">{item.name}</span>
                  </div>
                  <span class="font-mono text-sm font-medium">
                    {currencyFormat(item.value)}
                  </span>
                </div>
              {/each}
            </div>
            {#if payload.length > 1}
              <div class="mt-2 border-t pt-2">
                <div class="flex items-center justify-between">
                  <span class="text-sm font-medium">Total</span>
                  <span class="font-mono text-sm font-bold">
                    {currencyFormat(payload.reduce((sum, item) => sum + item.value, 0))}
                  </span>
                </div>
              </div>
            {/if}
          </div>
        )}
      />
    {/snippet}
  </AreaChart>
</Chart.Container>
```

### Interactive Legend

```svelte
<script lang="ts">
	let visibleSeries = $state<Set<string>>(new Set(series.map((s) => s.key)));

	function toggleSeries(key: string) {
		if (visibleSeries.has(key)) {
			visibleSeries.delete(key);
		} else {
			visibleSeries.add(key);
		}
		visibleSeries = new Set(visibleSeries);
	}

	const filteredData = $derived(
		data.map((d) => {
			const filtered = { ...d };
			series.forEach((s) => {
				if (!visibleSeries.has(s.key)) {
					filtered[s.key] = 0;
				}
			});
			return filtered;
		})
	);
</script>

<div class="space-y-4">
	<div class="flex flex-wrap gap-2">
		{#each series as s}
			<button
				class="flex items-center gap-2 rounded-md border px-3 py-1 text-sm transition-all"
				class:opacity-50={!visibleSeries.has(s.key)}
				onclick={() => toggleSeries(s.key)}
			>
				<div class="h-3 w-3 rounded-full" style="background-color: {s.color}" />
				{s.label}
			</button>
		{/each}
	</div>

	<AreaChart data={filteredData} {series} />
</div>
```

### Zoom and Pan

```svelte
<script lang="ts">
	import { zoom } from 'd3-zoom';
	import { select } from 'd3-selection';

	let svgElement: SVGElement;
	let transform = $state({ x: 0, y: 0, k: 1 });

	onMount(() => {
		const svg = select(svgElement);
		const zoomBehavior = zoom()
			.scaleExtent([0.5, 5])
			.on('zoom', (event) => {
				transform = event.transform;
			});

		svg.call(zoomBehavior);
	});
</script>

<svg bind:this={svgElement} class="w-full h-full">
	<g transform="translate({transform.x},{transform.y}) scale({transform.k})">
		<!-- Chart content -->
	</g>
</svg>

<div class="mt-2 flex gap-2">
	<Button size="sm" onclick={() => resetZoom()}>Reset</Button>
	<Button size="sm" onclick={() => zoomIn()}>Zoom In</Button>
	<Button size="sm" onclick={() => zoomOut()}>Zoom Out</Button>
</div>
```

## Performance Optimization

### Data Sampling for Large Datasets

```typescript
// utils/dataSampling.ts
export function sampleData(
	data: any[],
	maxPoints: number,
	method: 'nth' | 'lttb' | 'average' = 'nth'
) {
	if (data.length <= maxPoints) return data;

	switch (method) {
		case 'nth':
			// Simple nth-point sampling
			const step = Math.ceil(data.length / maxPoints);
			return data.filter((_, i) => i % step === 0);

		case 'lttb':
			// Largest Triangle Three Buckets algorithm
			return largestTriangleThreeBuckets(data, maxPoints);

		case 'average':
			// Average bucketing
			return averageBuckets(data, maxPoints);
	}
}

function largestTriangleThreeBuckets(data: any[], threshold: number) {
	// LTTB implementation
	// ... (implementation details)
}

function averageBuckets(data: any[], buckets: number) {
	const bucketSize = Math.floor(data.length / buckets);
	const sampled = [];

	for (let i = 0; i < buckets; i++) {
		const start = i * bucketSize;
		const end = Math.min((i + 1) * bucketSize, data.length);
		const bucket = data.slice(start, end);

		if (bucket.length > 0) {
			const avg = bucket.reduce((sum, d) => sum + d.value, 0) / bucket.length;
			sampled.push({
				...bucket[Math.floor(bucket.length / 2)],
				value: avg
			});
		}
	}

	return sampled;
}
```

### Virtualized Rendering

```svelte
<script lang="ts">
	import { VirtualList } from '@tanstack/svelte-virtual';

	let { data }: { data: any[] } = $props();

	const virtualizer = new VirtualList({
		count: data.length,
		estimateSize: () => 50,
		overscan: 5
	});
</script>

<div class="h-[400px] overflow-auto">
	<div style="height: {virtualizer.getTotalSize()}px; position: relative;">
		{#each virtualizer.getVirtualItems() as item}
			<div
				style="
          position: absolute;
          top: {item.start}px;
          height: {item.size}px;
          width: 100%;
        "
			>
				<!-- Render chart for visible item -->
				<MiniChart data={data[item.index]} />
			</div>
		{/each}
	</div>
</div>
```

## Responsive Design

### Container Queries

```svelte
<script lang="ts">
	let containerWidth = $state(0);
	let containerElement: HTMLElement;

	onMount(() => {
		const observer = new ResizeObserver((entries) => {
			containerWidth = entries[0].contentRect.width;
		});

		observer.observe(containerElement);

		return () => observer.disconnect();
	});

	const chartHeight = $derived(() => {
		if (containerWidth < 400) return 200;
		if (containerWidth < 600) return 250;
		return 300;
	});

	const showLegend = $derived(containerWidth > 500);
	const showAxis = $derived(containerWidth > 300);
</script>

<div bind:this={containerElement} class="w-full">
	<Chart.Container height={chartHeight}>
		<AreaChart {data} {series} showXAxis={showAxis} showYAxis={showAxis} {showLegend} />
	</Chart.Container>
</div>
```

### Adaptive Chart Types

```svelte
<script lang="ts">
	import { browser } from '$app/environment';

	let screenWidth = $state(browser ? window.innerWidth : 1024);

	if (browser) {
		window.addEventListener('resize', () => {
			screenWidth = window.innerWidth;
		});
	}

	const chartType = $derived(() => {
		if (screenWidth < 640) return 'bar'; // Better for mobile
		if (screenWidth < 1024) return 'line'; // Good for tablets
		return 'area'; // Best for desktop
	});
</script>

{#if chartType === 'area'}
	<AreaChart {data} {series} />
{:else if chartType === 'line'}
	<LineChart {data} {series} />
{:else}
	<BarChart {data} {series} />
{/if}
```

## Theming & Customization

### Theme-aware Colors

```typescript
// utils/chartTheme.ts
export function getChartTheme(isDark: boolean) {
	return {
		colors: {
			primary: isDark ? '#60a5fa' : '#3b82f6',
			secondary: isDark ? '#34d399' : '#10b981',
			tertiary: isDark ? '#fbbf24' : '#f59e0b',
			danger: isDark ? '#f87171' : '#ef4444',
			neutral: isDark ? '#9ca3af' : '#6b7280'
		},
		grid: {
			color: isDark ? 'rgba(255, 255, 255, 0.1)' : 'rgba(0, 0, 0, 0.1)',
			strokeDasharray: '3 3'
		},
		axis: {
			color: isDark ? '#9ca3af' : '#6b7280',
			fontSize: 12
		},
		tooltip: {
			background: isDark ? '#1f2937' : '#ffffff',
			border: isDark ? '#374151' : '#e5e7eb',
			text: isDark ? '#f3f4f6' : '#111827'
		}
	};
}
```

### Custom Color Scales

```svelte
<script lang="ts">
	import { scaleOrdinal, scaleSequential } from 'd3-scale';
	import { interpolatePlasma, interpolateViridis } from 'd3-scale-chromatic';

	// Categorical colors
	const categoricalScale = scaleOrdinal()
		.domain(['A', 'B', 'C', 'D', 'E'])
		.range(['#3b82f6', '#10b981', '#f59e0b', '#ef4444', '#8b5cf6']);

	// Sequential colors
	const sequentialScale = scaleSequential().domain([0, 100]).interpolator(interpolateViridis);

	// Diverging colors
	const divergingScale = scaleSequential()
		.domain([-50, 0, 50])
		.interpolator((d) => (d < 0.5 ? interpolatePlasma(d * 2) : interpolateViridis((d - 0.5) * 2)));
</script>
```

## Animation Patterns

### Smooth Transitions

```svelte
<script lang="ts">
	import { tweened } from 'svelte/motion';
	import { cubicOut } from 'svelte/easing';

	let data = $state([]);
	const animatedData = tweened([], {
		duration: 750,
		easing: cubicOut,
		interpolate: (a, b) => {
			// Custom interpolation for data arrays
			return (t) => {
				return b.map((item, i) => {
					const prev = a[i] || { value: 0 };
					return {
						...item,
						value: prev.value + (item.value - prev.value) * t
					};
				});
			};
		}
	});

	$effect(() => {
		animatedData.set(data);
	});
</script>

<AreaChart data={$animatedData} />
```

### Staggered Animations

```svelte
<script lang="ts">
	import { fly, fade } from 'svelte/transition';
	import { quintOut } from 'svelte/easing';

	let visible = $state(false);

	onMount(() => {
		visible = true;
	});
</script>

{#if visible}
	<div class="space-y-4">
		{#each charts as chart, i}
			<div
				in:fly={{
					y: 20,
					duration: 500,
					delay: i * 100,
					easing: quintOut
				}}
			>
				<Chart data={chart.data} />
			</div>
		{/each}
	</div>
{/if}
```

## Best Practices

### 1. Data Preparation

```typescript
// Prepare data on the server when possible
export const load: PageServerLoad = async () => {
	const rawData = await fetchData();

	// Transform and aggregate
	const chartData = prepareChartData(rawData);

	// Pre-calculate domains
	const xDomain = extent(chartData, (d) => d.date);
	const yDomain = [0, max(chartData, (d) => d.value) * 1.1];

	return {
		chartData,
		xDomain,
		yDomain
	};
};
```

### 2. Error Handling

```svelte
<script lang="ts">
	import { Alert } from '$lib/components/ui/alert';

	let { data }: { data: any[] } = $props();

	const hasData = $derived(data && data.length > 0);
	const hasValidData = $derived(
		hasData &&
			data.every((d) => d.date instanceof Date && typeof d.value === 'number' && !isNaN(d.value))
	);
</script>

{#if !hasData}
	<Alert>
		<AlertDescription>No data available</AlertDescription>
	</Alert>
{:else if !hasValidData}
	<Alert variant="destructive">
		<AlertDescription>Invalid data format</AlertDescription>
	</Alert>
{:else}
	<Chart {data} />
{/if}
```

### 3. Accessibility

```svelte
<script lang="ts">
	// Provide text alternatives
	const chartDescription = $derived(
		`Chart showing ${series.map((s) => s.label).join(', ')} ` +
			`from ${format(xDomain[0])} to ${format(xDomain[1])}`
	);
</script>

<div role="img" aria-label={chartDescription}>
	<Chart {data} {series} />
</div>

<!-- Provide data table alternative -->
<details class="mt-4">
	<summary class="cursor-pointer text-sm text-muted-foreground"> View data table </summary>
	<Table>
		<!-- Render data as table -->
	</Table>
</details>
```

### 4. Export Functionality

```svelte
<script lang="ts">
	import { toPng, toSvg } from 'html-to-image';
	import { download } from '$lib/utils/download';

	let chartElement: HTMLElement;

	async function exportChart(format: 'png' | 'svg') {
		const dataUrl = format === 'png' ? await toPng(chartElement) : await toSvg(chartElement);

		download(dataUrl, `chart.${format}`);
	}
</script>

<div bind:this={chartElement}>
	<Chart {data} />
</div>

<DropdownMenu.Root>
	<DropdownMenu.Trigger>
		<Button size="sm" variant="outline">Export</Button>
	</DropdownMenu.Trigger>
	<DropdownMenu.Content>
		<DropdownMenu.Item onclick={() => exportChart('png')}>Export as PNG</DropdownMenu.Item>
		<DropdownMenu.Item onclick={() => exportChart('svg')}>Export as SVG</DropdownMenu.Item>
	</DropdownMenu.Content>
</DropdownMenu.Root>
```

## Conclusion

This guide covers comprehensive patterns for building production-ready data visualizations. Key principles:

- **Performance First**: Optimize for large datasets
- **Responsive Design**: Adapt to all screen sizes
- **Interactivity**: Enhance user engagement
- **Accessibility**: Ensure charts are usable by everyone
- **Maintainability**: Use TypeScript and consistent patterns

Remember to:

- Test with real data volumes
- Profile performance on low-end devices
- Provide fallbacks for complex visualizations
- Document your chart configurations
- Consider print styles for reports

For specific visualization needs, combine these patterns while maintaining consistency across your dashboard.
