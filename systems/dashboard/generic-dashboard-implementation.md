# Generic Dashboard Implementation Guide

This guide provides a comprehensive approach to building feature-rich dashboards using modern web technologies. Based on proven patterns from production implementations, this guide covers all aspects of dashboard development from layout to advanced features like pagination and real-time updates.

## Table of Contents

1. [Architecture Overview](#architecture-overview)
2. [Technology Stack](#technology-stack)
3. [Layout Structure](#layout-structure)
4. [Statistics Cards](#statistics-cards)
5. [Data Visualization](#data-visualization)
6. [Data Tables](#data-tables)
7. [Pagination System](#pagination-system)
8. [Sorting & Filtering](#sorting--filtering)
9. [State Management](#state-management)
10. [Best Practices](#best-practices)

## Architecture Overview

### Component Hierarchy

```text
+page.svelte (Main Dashboard Page)
├── HeaderSection
│   ├── Title & Description
│   └── Action Buttons/Guides
├── StatsSection
│   └── StatsCard (4x)
├── ChartSection
│   └── Chart Component
└── TableSection
    ├── Table Header
    ├── Table Body
    └── Pagination Controls
```

### Data Flow

```text
Server (+page.server.ts)
    ↓ (load function)
PageData → +page.svelte
    ↓ (props)
Components (Stats, Chart, Table)
    ↓ (events/actions)
URL Updates → Server Reload
```

## Technology Stack

### Core Technologies

- **Framework**: SvelteKit 5 with TypeScript
- **UI Components**: shadcn/ui (Bits UI based)
- **Table Management**: TanStack Table v8
- **Charts**: Layerchart with D3
- **Styling**: Tailwind CSS
- **Icons**: Lucide Icons / Tabler Icons
- **State Management**: Svelte 5 Runes

### Key Dependencies

```json
{
	"@tanstack/table-core": "^8.x",
	"layerchart": "^0.x",
	"d3-scale": "^4.x",
	"d3-shape": "^3.x",
	"bits-ui": "^0.x",
	"tailwindcss": "^3.x"
}
```

## Layout Structure

### Main Page Layout

```svelte
<!-- +page.svelte -->
<script lang="ts">
	import type { PageData } from './$types';
	import StatsGrid from './components/StatsGrid.svelte';
	import DataChart from './components/DataChart.svelte';
	import DataTable from './components/DataTable.svelte';
	import * as Card from '$lib/components/ui/card';

	let { data }: { data: PageData } = $props();
</script>

<div class="min-h-screen p-6 space-y-6">
	<!-- Header Section -->
	<Card.Root>
		<Card.Header>
			<div class="flex items-center justify-between">
				<div>
					<Card.Title class="text-3xl font-bold">Dashboard Title</Card.Title>
					<Card.Description class="mt-2">Dashboard description and purpose</Card.Description>
				</div>
				<div class="flex gap-2">
					<!-- Action buttons -->
				</div>
			</div>
		</Card.Header>
	</Card.Root>

	<!-- Stats and Chart Grid -->
	<div class="grid gap-6 lg:grid-cols-2">
		<StatsGrid stats={data.stats} />
		<DataChart chartData={data.chartData} />
	</div>

	<!-- Table Section -->
	<DataTable {data} />
</div>
```

### Responsive Grid Patterns

```css
/* Mobile First Approach */
.grid {
	display: grid;
	gap: 1.5rem; /* 24px */
}

/* Tablet and up */
@media (min-width: 768px) {
	.stats-grid {
		grid-template-columns: repeat(2, 1fr);
	}
}

/* Desktop */
@media (min-width: 1024px) {
	.main-grid {
		grid-template-columns: repeat(2, 1fr);
	}

	.stats-grid {
		grid-template-columns: repeat(2, 1fr);
	}
}
```

## Statistics Cards

### Generic Stats Card Component

```svelte
<!-- StatsCard.svelte -->
<script lang="ts">
	import * as Card from '$lib/components/ui/card';
	import { Badge } from '$lib/components/ui/badge';
	import { IconTrendingUp, IconTrendingDown } from '@tabler/icons-svelte';

	interface StatConfig {
		title: string;
		value: number | string;
		description: string;
		trend?: {
			value: string;
			isPositive: boolean;
		};
		formatter?: (value: any) => string;
		icon?: any;
	}

	let { config }: { config: StatConfig } = $props();

	const displayValue = $derived(config.formatter ? config.formatter(config.value) : config.value);
</script>

<Card.Root class="relative overflow-hidden">
	<div class="absolute inset-0 bg-gradient-to-br from-muted/50 via-transparent to-transparent" />
	<Card.Header class="relative pb-2">
		<div class="flex items-center justify-between">
			<Card.Description class="text-sm font-medium">{config.title}</Card.Description>
			{#if config.icon}
				<svelte:component this={config.icon} class="h-4 w-4 text-muted-foreground" />
			{/if}
		</div>
	</Card.Header>
	<Card.Content class="relative">
		<div class="flex items-baseline justify-between">
			<div class="text-2xl font-bold tracking-tight sm:text-3xl">
				{displayValue}
			</div>
			{#if config.trend}
				<Badge
					variant={config.trend.isPositive ? 'secondary' : 'outline'}
					class="gap-1 font-medium"
				>
					{#if config.trend.isPositive}
						<IconTrendingUp class="h-3 w-3" />
					{:else}
						<IconTrendingDown class="h-3 w-3" />
					{/if}
					{config.trend.value}
				</Badge>
			{/if}
		</div>
		<p class="mt-1 text-xs text-muted-foreground">
			{config.description}
		</p>
	</Card.Content>
</Card.Root>
```

### Common Formatters

```typescript
// utils/formatters.ts
export const formatters = {
	currency: (value: number, currency = 'USD') => {
		return new Intl.NumberFormat('en-US', {
			style: 'currency',
			currency,
			minimumFractionDigits: 0,
			maximumFractionDigits: 0
		}).format(value);
	},

	percentage: (value: number) => {
		return `${value.toFixed(1)}%`;
	},

	number: (value: number) => {
		return new Intl.NumberFormat('en-US').format(value);
	},

	compact: (value: number) => {
		return new Intl.NumberFormat('en-US', {
			notation: 'compact',
			compactDisplay: 'short'
		}).format(value);
	}
};
```

### Stats Grid Implementation

```svelte
<!-- StatsGrid.svelte -->
<script lang="ts">
	import StatsCard from './StatsCard.svelte';
	import { formatters } from '$lib/utils/formatters';

	interface Stats {
		totalUsers: number;
		activeUsers: number;
		revenue: number;
		growthRate: number;
	}

	let { stats }: { stats: Stats } = $props();

	const statsConfigs = [
		{
			title: 'Total Users',
			value: stats.totalUsers,
			description: 'All registered users',
			trend: { value: '+12%', isPositive: true },
			formatter: formatters.number
		},
		{
			title: 'Active Users',
			value: stats.activeUsers,
			description: 'Active in last 30 days',
			trend: { value: '+5%', isPositive: true },
			formatter: formatters.number
		},
		{
			title: 'Revenue',
			value: stats.revenue,
			description: 'Monthly recurring revenue',
			trend: { value: '+23%', isPositive: true },
			formatter: formatters.currency
		},
		{
			title: 'Growth Rate',
			value: stats.growthRate,
			description: 'Month over month',
			trend: { value: `${stats.growthRate}%`, isPositive: stats.growthRate > 0 },
			formatter: formatters.percentage
		}
	];
</script>

<div class="grid grid-cols-1 gap-4 sm:grid-cols-2">
	{#each statsConfigs as config}
		<StatsCard {config} />
	{/each}
</div>
```

## Data Visualization

### Chart Component with Time Range Filtering

```svelte
<!-- DataChart.svelte -->
<script lang="ts">
	import * as Chart from '$lib/components/ui/chart/index.js';
	import * as Card from '$lib/components/ui/card/index.js';
	import * as Select from '$lib/components/ui/select/index.js';
	import * as ToggleGroup from '$lib/components/ui/toggle-group/index.js';
	import { scaleUtc, scaleLinear } from 'd3-scale';
	import { Area, AreaChart, Line, LineChart, Bar, BarChart } from 'layerchart';
	import { curveMonotoneX } from 'd3-shape';
	import dayjs from 'dayjs';

	interface ChartData {
		date: Date;
		[key: string]: number | Date;
	}

	interface ChartConfig {
		[key: string]: {
			label: string;
			color: string;
		};
	}

	let {
		data,
		config,
		type = 'area'
	}: {
		data: ChartData[];
		config: ChartConfig;
		type?: 'area' | 'line' | 'bar';
	} = $props();

	let timeRange = $state('30d');

	const timeRanges = [
		{ value: '7d', label: 'Last 7 days' },
		{ value: '30d', label: 'Last 30 days' },
		{ value: '90d', label: 'Last 90 days' },
		{ value: '1y', label: 'Last year' }
	];

	const filteredData = $derived(() => {
		const now = new Date();
		let startDate: Date;

		switch (timeRange) {
			case '7d':
				startDate = dayjs(now).subtract(7, 'day').toDate();
				break;
			case '30d':
				startDate = dayjs(now).subtract(30, 'day').toDate();
				break;
			case '90d':
				startDate = dayjs(now).subtract(90, 'day').toDate();
				break;
			case '1y':
				startDate = dayjs(now).subtract(1, 'year').toDate();
				break;
			default:
				startDate = dayjs(now).subtract(30, 'day').toDate();
		}

		return data.filter((item) => item.date >= startDate);
	});

	const series = Object.entries(config).map(([key, cfg]) => ({
		key,
		label: cfg.label,
		color: cfg.color
	}));
</script>

<Card.Root class="@container/chart">
	<Card.Header>
		<Card.Title>Data Trends</Card.Title>
		<Card.Description>
			<span class="@[540px]/chart:block hidden">Historical data visualization</span>
			<span class="@[540px]/chart:hidden">Data trends</span>
		</Card.Description>
		<Card.Action>
			<!-- Desktop toggle group -->
			<ToggleGroup.Root
				type="single"
				bind:value={timeRange}
				variant="outline"
				class="@[767px]/chart:flex hidden"
			>
				{#each timeRanges as range}
					<ToggleGroup.Item value={range.value}>{range.label}</ToggleGroup.Item>
				{/each}
			</ToggleGroup.Root>

			<!-- Mobile select -->
			<Select.Root type="single" bind:value={timeRange}>
				<Select.Trigger
					size="sm"
					class="@[767px]/chart:hidden flex w-40"
					aria-label="Select time range"
				>
					{timeRanges.find((r) => r.value === timeRange)?.label}
				</Select.Trigger>
				<Select.Content>
					{#each timeRanges as range}
						<Select.Item value={range.value}>{range.label}</Select.Item>
					{/each}
				</Select.Content>
			</Select.Root>
		</Card.Action>
	</Card.Header>
	<Card.Content class="px-2 pt-4 sm:px-6 sm:pt-6">
		<Chart.Container {config} class="aspect-auto h-[250px] w-full">
			{#if type === 'area'}
				<AreaChart
					data={filteredData}
					x="date"
					xScale={scaleUtc()}
					yScale={scaleLinear()}
					{series}
					props={{
						area: {
							curve: curveMonotoneX,
							'fill-opacity': 0.3,
							line: { class: 'stroke-2' }
						}
					}}
				/>
			{:else if type === 'line'}
				<LineChart data={filteredData} x="date" xScale={scaleUtc()} {series} />
			{:else if type === 'bar'}
				<BarChart data={filteredData} x="date" xScale={scaleUtc()} {series} />
			{/if}
		</Chart.Container>
	</Card.Content>
</Card.Root>
```

### Chart Configuration Examples

```typescript
// Chart configurations for different use cases

// Revenue Chart
const revenueConfig = {
	revenue: { label: 'Revenue', color: '#3b82f6' },
	profit: { label: 'Profit', color: '#10b981' },
	expenses: { label: 'Expenses', color: '#ef4444' }
};

// User Activity Chart
const activityConfig = {
	signups: { label: 'New Signups', color: '#8b5cf6' },
	activeUsers: { label: 'Active Users', color: '#3b82f6' },
	churned: { label: 'Churned Users', color: '#ef4444' }
};

// Performance Metrics
const performanceConfig = {
	responseTime: { label: 'Response Time (ms)', color: '#f59e0b' },
	errorRate: { label: 'Error Rate (%)', color: '#ef4444' },
	uptime: { label: 'Uptime (%)', color: '#10b981' }
};
```

## Data Tables

### Table Setup with TanStack Table

```svelte
<!-- DataTable.svelte -->
<script lang="ts">
	import {
		getCoreRowModel,
		getPaginationRowModel,
		getSortedRowModel,
		getFilteredRowModel,
		type PaginationState,
		type SortingState,
		type ColumnFiltersState,
		type Updater
	} from '@tanstack/table-core';
	import { createSvelteTable } from '$lib/components/ui/data-table';
	import FlexRender from '$lib/components/ui/data-table/flex-render.svelte';
	import * as Table from '$lib/components/ui/table';
	import { table_state as _ } from './table-state.svelte';
	import { updateUrl } from './updateUrl';
	import Pagination from './Pagination.svelte';
	import { onMount } from 'svelte';

	interface Props {
		data: any;
	}

	let { data }: Props = $props();

	// Initialize state from URL on mount
	onMount(() => {
		_.pagination = {
			pageIndex: data.pagination.page,
			pageSize: data.pagination.pageSize
		};

		if (data.sorting.sortBy) {
			_.sorting = [
				{
					id: data.sorting.sortBy,
					desc: data.sorting.sortOrder === 'desc'
				}
			];
		}
	});

	// State update handlers
	const setPagination = (updater: Updater<PaginationState>) => {
		_.pagination = updater instanceof Function ? updater(_.pagination) : updater;
		updateUrl(table);
	};

	const setSorting = (updater: Updater<SortingState>) => {
		_.sorting = updater instanceof Function ? updater(_.sorting) : updater;
		updateUrl(table);
	};

	const setColumnFilters = (updater: Updater<ColumnFiltersState>) => {
		_.columnFilters = updater instanceof Function ? updater(_.columnFilters) : updater;
		updateUrl(table);
	};

	// Create table instance
	const table = createSvelteTable({
		get data() {
			return data.items;
		},
		columns: columnDefs,
		getCoreRowModel: getCoreRowModel(),
		getPaginationRowModel: getPaginationRowModel(),
		getSortedRowModel: getSortedRowModel(),
		getFilteredRowModel: getFilteredRowModel(),
		manualPagination: true,
		manualSorting: true,
		manualFiltering: true,
		rowCount: data.rowCount,
		onPaginationChange: setPagination,
		onSortingChange: setSorting,
		onColumnFiltersChange: setColumnFilters,
		state: {
			get pagination() {
				return _.pagination;
			},
			get sorting() {
				return _.sorting;
			},
			get columnFilters() {
				return _.columnFilters;
			}
		}
	});
</script>

<div class="space-y-4">
	<!-- Filters -->
	<div class="flex items-center justify-between">
		<Input
			placeholder="Search..."
			value={_.globalFilter}
			oninput={(e) => (_.globalFilter = e.currentTarget.value)}
			class="max-w-xs"
		/>
		<div class="flex gap-2">
			<!-- Additional filters/actions -->
		</div>
	</div>

	<!-- Table -->
	<div class="rounded-md border">
		<Table.Root>
			<Table.Header>
				{#each table.getHeaderGroups() as headerGroup}
					<Table.Row>
						{#each headerGroup.headers as header}
							<Table.Head>
								{#if !header.isPlaceholder}
									<div class="flex items-center gap-2">
										<FlexRender
											content={header.column.columnDef.header}
											context={header.getContext()}
										/>
										{#if header.column.getCanSort()}
											<SortButton column={header.column} />
										{/if}
									</div>
								{/if}
							</Table.Head>
						{/each}
					</Table.Row>
				{/each}
			</Table.Header>
			<Table.Body>
				{#if table.getRowModel().rows?.length}
					{#each table.getRowModel().rows as row}
						<Table.Row>
							{#each row.getVisibleCells() as cell}
								<Table.Cell>
									<FlexRender content={cell.column.columnDef.cell} context={cell.getContext()} />
								</Table.Cell>
							{/each}
						</Table.Row>
					{/each}
				{:else}
					<Table.Row>
						<Table.Cell colspan={columns.length} class="h-24 text-center">
							No results found.
						</Table.Cell>
					</Table.Row>
				{/if}
			</Table.Body>
		</Table.Root>
	</div>

	<!-- Pagination -->
	<Pagination {table} />
</div>
```

### Column Definitions Pattern

```typescript
// columnDefs.ts
import type { ColumnDef } from '@tanstack/table-core';
import { renderComponent } from '$lib/components/ui/data-table/render-component';
import StatusBadge from './StatusBadge.svelte';
import ActionMenu from './ActionMenu.svelte';

interface DataRow {
	id: string;
	name: string;
	status: 'active' | 'inactive' | 'pending';
	value: number;
	date: Date;
	tags: string[];
}

export const columnDefs: ColumnDef<DataRow>[] = [
	{
		id: 'select',
		header: ({ table }) =>
			renderComponent(Checkbox, {
				checked: table.getIsAllPageRowsSelected(),
				onCheckedChange: (value) => table.toggleAllPageRowsSelected(!!value),
				'aria-label': 'Select all'
			}),
		cell: ({ row }) =>
			renderComponent(Checkbox, {
				checked: row.getIsSelected(),
				onCheckedChange: (value) => row.toggleSelected(!!value),
				'aria-label': 'Select row'
			}),
		enableSorting: false,
		enableHiding: false
	},
	{
		accessorKey: 'name',
		header: 'Name',
		cell: ({ row }) => {
			const name = row.getValue('name') as string;
			return `<div class="font-medium">${name}</div>`;
		}
	},
	{
		accessorKey: 'status',
		header: 'Status',
		cell: ({ row }) => {
			const status = row.getValue('status') as string;
			return renderComponent(StatusBadge, { status });
		},
		filterFn: (row, id, value) => {
			return value.includes(row.getValue(id));
		}
	},
	{
		accessorKey: 'value',
		header: ({ column }) => {
			return renderComponent(Button, {
				variant: 'ghost',
				onclick: () => column.toggleSorting(column.getIsSorted() === 'asc'),
				children: 'Value'
			});
		},
		cell: ({ row }) => {
			const value = parseFloat(row.getValue('value'));
			const formatted = new Intl.NumberFormat('en-US', {
				style: 'currency',
				currency: 'USD'
			}).format(value);
			return `<div class="text-right font-medium">${formatted}</div>`;
		}
	},
	{
		accessorKey: 'date',
		header: 'Date',
		cell: ({ row }) => {
			const date = row.getValue('date') as Date;
			return new Intl.DateTimeFormat('en-US').format(date);
		}
	},
	{
		id: 'actions',
		enableHiding: false,
		cell: ({ row }) => {
			return renderComponent(ActionMenu, { row });
		}
	}
];
```

### Custom Cell Components

```svelte
<!-- StatusBadge.svelte -->
<script lang="ts">
	import { Badge } from '$lib/components/ui/badge';
	import { cn } from '$lib/utils';

	let { status }: { status: string } = $props();

	const statusConfig = {
		active: {
			label: 'Active',
			variant: 'default',
			class: 'bg-green-500/20 text-green-700 border-green-500/30'
		},
		inactive: {
			label: 'Inactive',
			variant: 'secondary',
			class: 'bg-gray-500/20 text-gray-700 border-gray-500/30'
		},
		pending: {
			label: 'Pending',
			variant: 'outline',
			class: 'bg-yellow-500/20 text-yellow-700 border-yellow-500/30'
		}
	} as const;

	const config = statusConfig[status as keyof typeof statusConfig] || statusConfig.inactive;
</script>

<Badge variant={config.variant} class={cn('capitalize', config.class)}>
	{config.label}
</Badge>
```

## Pagination System

### Server-Side Pagination

```typescript
// +page.server.ts
import type { PageServerLoad } from './$types';
import { db } from '$lib/server/db';

export const load: PageServerLoad = async ({ url }) => {
	// Parse pagination parameters
	const page = Number(url.searchParams.get('page')) || 0;
	const pageSize = Number(url.searchParams.get('pageSize')) || 10;

	// Parse sorting parameters
	const sortBy = url.searchParams.get('sortBy') || 'id';
	const sortOrder = url.searchParams.get('sortOrder') || 'asc';

	// Parse filter parameters
	const search = url.searchParams.get('search') || '';
	const status = url.searchParams.get('status') || 'all';

	// Validate parameters
	const safePage = Math.max(0, page);
	const safePageSize = Math.min(Math.max(1, pageSize), 100);

	// Build query
	const where = {
		...(search && {
			OR: [
				{ name: { contains: search, mode: 'insensitive' } },
				{ email: { contains: search, mode: 'insensitive' } }
			]
		}),
		...(status !== 'all' && { status })
	};

	// Execute queries
	const [items, totalCount] = await Promise.all([
		db.items.findMany({
			where,
			orderBy: { [sortBy]: sortOrder },
			skip: safePage * safePageSize,
			take: safePageSize
		}),
		db.items.count({ where })
	]);

	return {
		items,
		rowCount: totalCount,
		pagination: {
			page: safePage,
			pageSize: safePageSize,
			totalPages: Math.ceil(totalCount / safePageSize)
		},
		sorting: {
			sortBy,
			sortOrder
		},
		filters: {
			search,
			status
		}
	};
};
```

### Pagination Component

```svelte
<!-- Pagination.svelte -->
<script lang="ts" generics="TData">
	import { Button } from '$lib/components/ui/button';
	import * as Select from '$lib/components/ui/select';
	import { ChevronLeft, ChevronRight, ChevronsLeft, ChevronsRight } from 'lucide-svelte';
	import type { Table } from '@tanstack/table-core';

	let { table }: { table: Table<TData> } = $props();

	const pageSizes = [10, 20, 30, 40, 50, 100];
</script>

<div class="flex items-center justify-between px-2">
	<div class="flex-1 text-sm text-muted-foreground">
		{table.getFilteredRowModel().rows.length} of{' '}
		{table.getRowCount()} row(s) total
	</div>
	<div class="flex items-center space-x-6 lg:space-x-8">
		<div class="flex items-center space-x-2">
			<p class="text-sm font-medium">Rows per page</p>
			<Select.Root
				value={`${table.getState().pagination.pageSize}`}
				onValueChange={(value) => {
					table.setPageSize(Number(value));
				}}
			>
				<Select.Trigger class="h-8 w-[70px]">
					<Select.Value>{table.getState().pagination.pageSize}</Select.Value>
				</Select.Trigger>
				<Select.Content side="top">
					{#each pageSizes as pageSize}
						<Select.Item value={`${pageSize}`}>{pageSize}</Select.Item>
					{/each}
				</Select.Content>
			</Select.Root>
		</div>
		<div class="flex w-[100px] items-center justify-center text-sm font-medium">
			Page {table.getState().pagination.pageIndex + 1} of{' '}
			{table.getPageCount()}
		</div>
		<div class="flex items-center space-x-2">
			<Button
				variant="outline"
				class="hidden h-8 w-8 p-0 lg:flex"
				onclick={() => table.setPageIndex(0)}
				disabled={!table.getCanPreviousPage()}
			>
				<span class="sr-only">Go to first page</span>
				<ChevronsLeft class="h-4 w-4" />
			</Button>
			<Button
				variant="outline"
				class="h-8 w-8 p-0"
				onclick={() => table.previousPage()}
				disabled={!table.getCanPreviousPage()}
			>
				<span class="sr-only">Go to previous page</span>
				<ChevronLeft class="h-4 w-4" />
			</Button>
			<Button
				variant="outline"
				class="h-8 w-8 p-0"
				onclick={() => table.nextPage()}
				disabled={!table.getCanNextPage()}
			>
				<span class="sr-only">Go to next page</span>
				<ChevronRight class="h-4 w-4" />
			</Button>
			<Button
				variant="outline"
				class="hidden h-8 w-8 p-0 lg:flex"
				onclick={() => table.setPageIndex(table.getPageCount() - 1)}
				disabled={!table.getCanNextPage()}
			>
				<span class="sr-only">Go to last page</span>
				<ChevronsRight class="h-4 w-4" />
			</Button>
		</div>
	</div>
</div>
```

### URL State Management

```typescript
// updateUrl.ts
import { goto } from '$app/navigation';
import type { Table } from '@tanstack/table-core';
import { table_state as _ } from './table-state.svelte';

export const updateUrl = (table: Table<any>) => {
	// Ensure pagination is within valid range
	const validPagination = ensureValidPagination(_.pagination, table.getPageCount());

	if (validPagination !== _.pagination) {
		_.pagination = validPagination;
	}

	const params = new URLSearchParams();

	// Pagination params
	params.set('page', _.pagination.pageIndex.toString());
	params.set('pageSize', _.pagination.pageSize.toString());

	// Sorting params
	if (_.sorting.length > 0) {
		params.set('sortBy', _.sorting[0].id);
		params.set('sortOrder', _.sorting[0].desc ? 'desc' : 'asc');
	}

	// Filter params
	if (_.globalFilter) {
		params.set('search', _.globalFilter);
	}

	_.columnFilters.forEach((filter) => {
		params.set(filter.id, String(filter.value));
	});

	goto(`?${params.toString()}`, {
		noScroll: true,
		replaceState: true,
		keepFocus: true
	});
};

function ensureValidPagination(pagination: PaginationState, totalPages: number): PaginationState {
	if (pagination.pageIndex >= totalPages && totalPages > 0) {
		return {
			...pagination,
			pageIndex: totalPages - 1
		};
	}
	return pagination;
}
```

## Sorting & Filtering

### Sort Button Component

```svelte
<!-- SortButton.svelte -->
<script lang="ts">
	import { Button } from '$lib/components/ui/button';
	import { ArrowUpDown, ArrowUp, ArrowDown } from 'lucide-svelte';
	import type { Column } from '@tanstack/table-core';

	let { column }: { column: Column<any, any> } = $props();

	const sortState = $derived(column.getIsSorted());
</script>

<Button
	variant="ghost"
	size="sm"
	class="-ml-3 h-8 data-[state=open]:bg-accent"
	onclick={() => column.toggleSorting(column.getIsSorted() === 'asc')}
>
	<span>{column.columnDef.header}</span>
	{#if sortState === 'asc'}
		<ArrowUp class="ml-2 h-4 w-4" />
	{:else if sortState === 'desc'}
		<ArrowDown class="ml-2 h-4 w-4" />
	{:else}
		<ArrowUpDown class="ml-2 h-4 w-4" />
	{/if}
</Button>
```

### Filter Components

```svelte
<!-- TableFilters.svelte -->
<script lang="ts">
	import { Input } from '$lib/components/ui/input';
	import * as Select from '$lib/components/ui/select';
	import { Button } from '$lib/components/ui/button';
	import { X } from 'lucide-svelte';
	import type { Table } from '@tanstack/table-core';

	let { table }: { table: Table<any> } = $props();

	const statusOptions = [
		{ value: 'all', label: 'All Status' },
		{ value: 'active', label: 'Active' },
		{ value: 'inactive', label: 'Inactive' },
		{ value: 'pending', label: 'Pending' }
	];

	let searchValue = $state('');
	let statusFilter = $state('all');

	function applyFilters() {
		table.getColumn('name')?.setFilterValue(searchValue);
		table.getColumn('status')?.setFilterValue(statusFilter === 'all' ? undefined : statusFilter);
	}

	function clearFilters() {
		searchValue = '';
		statusFilter = 'all';
		table.resetColumnFilters();
	}

	const hasActiveFilters = $derived(table.getState().columnFilters.length > 0);
</script>

<div class="flex items-center gap-2">
	<Input
		placeholder="Search by name..."
		bind:value={searchValue}
		onchange={applyFilters}
		class="h-8 w-[250px]"
	/>

	<Select.Root bind:value={statusFilter} onValueChange={applyFilters}>
		<Select.Trigger class="h-8 w-[180px]">
			<Select.Value placeholder="Select status" />
		</Select.Trigger>
		<Select.Content>
			{#each statusOptions as option}
				<Select.Item value={option.value}>{option.label}</Select.Item>
			{/each}
		</Select.Content>
	</Select.Root>

	{#if hasActiveFilters}
		<Button variant="ghost" size="sm" onclick={clearFilters} class="h-8 px-2 lg:px-3">
			Reset
			<X class="ml-2 h-4 w-4" />
		</Button>
	{/if}
</div>
```

### Advanced Filtering

```typescript
// Advanced filter functions
export const filterFunctions = {
	// Text contains (case insensitive)
	contains: (row: any, columnId: string, filterValue: string) => {
		const value = row.getValue(columnId);
		return String(value).toLowerCase().includes(filterValue.toLowerCase());
	},

	// Exact match
	equals: (row: any, columnId: string, filterValue: any) => {
		return row.getValue(columnId) === filterValue;
	},

	// Number range
	between: (row: any, columnId: string, filterValue: [number, number]) => {
		const value = row.getValue(columnId) as number;
		return value >= filterValue[0] && value <= filterValue[1];
	},

	// Date range
	dateRange: (row: any, columnId: string, filterValue: [Date, Date]) => {
		const date = row.getValue(columnId) as Date;
		return date >= filterValue[0] && date <= filterValue[1];
	},

	// Array includes
	includesAny: (row: any, columnId: string, filterValue: any[]) => {
		const value = row.getValue(columnId);
		return Array.isArray(value)
			? value.some((v) => filterValue.includes(v))
			: filterValue.includes(value);
	}
};
```

## State Management

### Table State with Svelte 5 Runes

```typescript
// table-state.svelte.ts
import type {
	PaginationState,
	SortingState,
	ColumnFiltersState,
	RowSelectionState,
	VisibilityState
} from '@tanstack/table-core';

// Global table state using runes
export const table_state = $state({
	pagination: {
		pageIndex: 0,
		pageSize: 10
	} as PaginationState,
	sorting: [] as SortingState,
	columnFilters: [] as ColumnFiltersState,
	globalFilter: '',
	rowSelection: {} as RowSelectionState,
	columnVisibility: {} as VisibilityState
});

// Derived states
export const selectedRows = $derived(
	Object.keys(table_state.rowSelection).filter((key) => table_state.rowSelection[key])
);

export const hasFilters = $derived(
	table_state.columnFilters.length > 0 || table_state.globalFilter !== ''
);
```

### Loading States

```svelte
<!-- LoadingOverlay.svelte -->
<script lang="ts">
	import { navigating } from '$app/stores';

	let showLoading = $state(false);
	let loadingTimeout: NodeJS.Timeout | null = null;

	$effect(() => {
		if ($navigating !== null) {
			// Show loading after 100ms to avoid flash
			loadingTimeout = setTimeout(() => {
				showLoading = true;
			}, 100);
		} else {
			if (loadingTimeout) {
				clearTimeout(loadingTimeout);
				loadingTimeout = null;
			}
			showLoading = false;
		}

		return () => {
			if (loadingTimeout) {
				clearTimeout(loadingTimeout);
			}
		};
	});
</script>

{#if showLoading}
	<div class="absolute inset-0 z-50 bg-background/80 backdrop-blur-sm">
		<div class="flex h-full items-center justify-center">
			<div class="flex flex-col items-center gap-2">
				<div class="h-8 w-8 animate-spin rounded-full border-4 border-muted border-t-primary" />
				<p class="text-sm text-muted-foreground">Loading...</p>
			</div>
		</div>
	</div>
{/if}
```

## Best Practices

### Performance Optimization

1. **Server-Side Operations**

   - Implement pagination, sorting, and filtering on the server
   - Use database indexes for commonly sorted/filtered columns
   - Limit page size to prevent large data transfers

2. **Data Fetching**

   ```typescript
   // Parallel data fetching
   const [data, stats, chartData] = await Promise.all([
   	fetchTableData(params),
   	fetchStatistics(),
   	fetchChartData()
   ]);
   ```

3. **Memoization**

   ```svelte
   <script>
   	// Use $derived for computed values
   	const expensiveComputation = $derived(() => {
   		return data.items.reduce((acc, item) => {
   			// Complex calculation
   			return acc + item.value;
   		}, 0);
   	});
   </script>
   ```

### TypeScript Patterns

1. **Type-Safe Column Definitions**

   ```typescript
   import type { ColumnDef } from '@tanstack/table-core';

   // Define your data type
   interface User {
   	id: string;
   	name: string;
   	email: string;
   	role: 'admin' | 'user';
   	createdAt: Date;
   }

   // Type-safe columns
   export const columns: ColumnDef<User>[] = [
   	{
   		accessorKey: 'name',
   		header: 'Name',
   		// Cell type is inferred as User
   		cell: ({ row }) => row.getValue('name')
   	}
   ];
   ```

2. **Generic Components**

   ```svelte
   <script lang="ts" generics="T extends Record<string, any>">
   	import type { ColumnDef } from '@tanstack/table-core';

   	interface Props {
   		data: T[];
   		columns: ColumnDef<T>[];
   	}

   	let { data, columns }: Props = $props();
   </script>
   ```

### Accessibility

1. **Keyboard Navigation**

   ```svelte
   <button
     onclick={handleSort}
     onkeydown={(e) => {
       if (e.key === 'Enter' || e.key === ' ') {
         e.preventDefault();
         handleSort();
       }
     }}
     aria-label={`Sort by ${column.name}`}
     aria-sort={sortDirection}
   >
   ```

2. **Screen Reader Support**

   ```svelte
   <div role="status" aria-live="polite" class="sr-only">
   	{#if loading}
   		Loading data...
   	{:else}
   		Showing {results.length} results
   	{/if}
   </div>
   ```

3. **Focus Management**

   ```typescript
   import { afterNavigate } from '$app/navigation';

   afterNavigate(() => {
   	// Restore focus after navigation
   	const mainContent = document.getElementById('main-content');
   	mainContent?.focus();
   });
   ```

### Error Handling

```svelte
<!-- ErrorBoundary.svelte -->
<script lang="ts">
	import { Alert, AlertDescription } from '$lib/components/ui/alert';
	import { AlertCircle } from 'lucide-svelte';

	let { error }: { error: Error | null } = $props();
</script>

{#if error}
	<Alert variant="destructive">
		<AlertCircle class="h-4 w-4" />
		<AlertDescription>
			{error.message || 'An unexpected error occurred'}
		</AlertDescription>
	</Alert>
{:else}
	<slot />
{/if}
```

### Testing Approach

```typescript
// Example test for dashboard component
import { render, screen } from '@testing-library/svelte';
import { vi } from 'vitest';
import Dashboard from './Dashboard.svelte';

describe('Dashboard', () => {
	const mockData = {
		stats: {
			totalUsers: 1000,
			activeUsers: 750,
			revenue: 50000,
			growthRate: 15
		},
		items: [],
		rowCount: 0
	};

	it('renders stats cards', () => {
		render(Dashboard, { props: { data: mockData } });

		expect(screen.getByText('1,000')).toBeInTheDocument();
		expect(screen.getByText('750')).toBeInTheDocument();
		expect(screen.getByText('$50,000')).toBeInTheDocument();
		expect(screen.getByText('15%')).toBeInTheDocument();
	});

	it('handles empty data gracefully', () => {
		render(Dashboard, {
			props: {
				data: { ...mockData, items: [] }
			}
		});

		expect(screen.getByText('No results found.')).toBeInTheDocument();
	});
});
```

## Conclusion

This guide provides a comprehensive foundation for building modern, feature-rich dashboards. The patterns and components described here can be adapted for various use cases while maintaining consistency and best practices.

Key takeaways:

- Use server-side operations for better performance
- Implement URL-based state for shareable views
- Create reusable, typed components
- Focus on accessibility and user experience
- Test critical functionality

For specific implementations, adapt these patterns to your domain requirements while maintaining the core architectural principles.
