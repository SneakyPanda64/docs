# shadcn‑Svelte – Component Library Documentation

> **shadcn‑Svelte** is a collection of reusable, accessible UI components for Svelte, inspired by the original shadcn‑React library.  
> Built by shadcn, ported to Svelte by Huntabyte & CokaKoala.

---

## Table of Contents

| Section | Description |
|---------|-------------|
| [Getting Started](#getting-started) | Installation & basic setup |
| [Components](#components) | Core UI building blocks |
| [Blocks](#blocks) | Layout helpers |
| [Charts](#charts) | Data‑visualisation components |
| [Themes & Colors](#themes--colors) | Customisable colour palettes |
| [Examples](#examples) | Live‑code snippets from the UI mock‑up |
| [Contributing](#contributing) | How to help |
| [License](#license) | Licensing information |

---

## Getting Started

```bash
# Install via npm
npm i shadcn-svelte

# Or via yarn
yarn add shadcn-svelte
```

Add the global styles to your `app.html` (or equivalent):

```html
<link rel="stylesheet" href="/node_modules/shadcn-svelte/dist/index.css" />
```

Import the components you need:

```svelte
<script>
  import { Button, Card, Input } from 'shadcn-svelte';
</script>
```

---

## Components

| Component | Description | Example |
|-----------|-------------|---------|
| **Button** | Primary action button | ` <Button>Click me</Button> ` |
| **Card** | Content container with optional header/footer | ` <Card title="Total Revenue">$15,231.89</Card> ` |
| **Input** | Text input field | ` <Input placeholder="Email" /> ` |
| **Select** | Dropdown selector | ` <Select options={['Starter', 'Pro']} /> ` |
| **Table** | Data table with pagination | ` <Table data={rows} /> ` |
| **Modal** | Overlay dialog | ` <Modal open={isOpen}>...</Modal> ` |
| **Toast** | Brief notification | ` <Toast message="Upgrade successful!" /> ` |

> **Tip:** All components accept the same `class` and `style` props for custom styling.

---

## Blocks

Blocks are layout helpers that help you structure pages.

| Block | Usage | Example |
|-------|-------|---------|
| **Container** | Wraps content with responsive padding | ` <Container>...</Container> ` |
| **Grid** | CSS‑grid layout | ` <Grid cols={3}>...</Grid> ` |
| **Flex** | Flexbox layout | ` <Flex direction="row" gap="4">...</Flex> ` |

---

## Charts

Charts are powered by **Chart.js** under the hood.

| Chart | Props | Example |
|-------|-------|---------|
| **BarChart** | `data`, `options` | ` <BarChart data={chartData} /> ` |
| **LineChart** | `data`, `options` | ` <LineChart data={chartData} /> ` |
| **PieChart** | `data`, `options` | ` <PieChart data={chartData} /> ` |

```svelte
<script>
  import { BarChart } from 'shadcn-svelte';
  const chartData = {
    labels: ['Jan', 'Feb', 'Mar'],
    datasets: [{ label: 'Revenue', data: [1200, 1900, 3000] }]
  };
</script>

<BarChart {chartData} />
```

---

## Themes & Colors

The library ships with a set of pre‑defined themes that can be swapped at runtime.

```svelte
<script>
  import { ThemeProvider, useTheme } from 'shadcn-svelte';
  const { setTheme } = useTheme();
</script>

<ThemeProvider>
  <Button on:click={() => setTheme('red')}>Red Theme</Button>
  <Button on:click={() => setTheme('green')}>Green Theme</Button>
</ThemeProvider>
```

### Available Themes

| Theme | Description |
|-------|-------------|
| Default | Light mode |
| Red | Vibrant red palette |
| Rose | Soft rose tones |
| Orange | Warm orange hues |
| Green | Fresh green shades |
| Blue | Cool blue tones |
| Yellow | Bright yellow accents |
| Violet | Deep violet hues |

---

## Examples

Below are the exact UI snippets from the original mock‑up, now rendered as Svelte code.

### 1. Dashboard Header

```svelte
<Card class="flex justify-between items-center p-4">
  <div>
    <h1 class="text-2xl font-bold">Total Revenue</h1>
    <p class="text-xl">$15,231.89</p>
    <span class="text-green-600">+20.1% from last month</span>
  </div>
  <Button variant="outline">View More</Button>
</Card>
```

### 2. Subscription Upgrade Prompt

```svelte
<Card class="p-6">
  <h2 class="text-xl font-semibold mb-4">Upgrade your subscription</h2>
  <p class="mb-6">You are currently on the free plan. Upgrade to the pro plan to get access to all features.</p>

  <form class="space-y-4">
    <Input name="name" placeholder="Name" />
    <Input name="email" placeholder="Email" type="email" />
    <Input name="card" placeholder="Card Number" />

    <Select name="plan" options={['Starter Plan', 'Pro Plan']} />

    <div class="flex items-center">
      <Checkbox name="terms" />
      <label class="ml-2">I agree to the terms and conditions</label>
    </div>

    <div class="flex items-center">
      <Checkbox name="email" />
      <label class="ml-2">Allow us to send you emails</label>
    </div>

    <div class="flex justify-end gap-2">
      <Button variant="ghost">Cancel</Button>
      <Button type="submit">Upgrade Plan</Button>
    </div>
  </form>
</Card>
```

### 3. Team Members List

```svelte
<Card class="p-6">
  <h2 class="text-xl font-semibold mb-4">Team Members</h2>
  <p class="mb-6">Invite your team members to collaborate.</p>

  <Table data={teamMembers} columns={[
    { key: 'name', label: 'Name' },
    { key: 'email', label: 'Email' },
    { key: 'role', label: 'Role' }
  ]} />

  <Button variant="outline" class="mt-4">Invite Member</Button>
</Card>
```

### 4. Cookie Settings

```svelte
<Card class="p-6">
  <h2 class="text-xl font-semibold mb-4">Cookie Settings</h2>
  <p class="mb-6">Manage your cookie settings here.</p>

  <div class="space-y-4">
    <div class="flex items-center">
      <Checkbox name="necessary" checked />
      <label class="ml-2">Strictly Necessary</label>
    </div>
    <p class="ml-6 text-sm text-gray-600">These cookies are essential in order to use the website and use its features.</p>

    <div class="flex items-center">
      <Checkbox name="functional" />
      <label class="ml-2">Functional Cookies</label>
    </div>
    <p class="ml-6 text-sm text-gray-600">These cookies allow the website to provide personalized functionality.</p>
  </div>

  <Button variant="primary" class="mt-4">Save preferences</Button>
</Card>
```

### 5. Calendar Widget

```svelte
<Card class="p-6">
  <h2 class="text-xl font-semibold mb-4">Calendar</h2>

  <Calendar month="May" year="2023" />

  <div class="flex justify-end mt-4">
    <Button variant="ghost">Previous</Button>
    <Button variant="ghost">Next</Button>
  </div>
</Card>
```

### 6. Payments Table

```svelte
<Card class="p-6">
  <h2 class="text-xl font-semibold mb-4">Payments</h2>

  <Table data={payments} columns={[
    { key: 'status', label: 'Status' },
    { key: 'email', label: 'Email' },
    { key: 'amount', label: 'Amount' }
  ]} />

  <Button variant="outline" class="mt-4">Add Payment</Button>
</Card>
```

---

## Contributing

1. Fork the repo.  
2. Create a feature branch (`git checkout -b feature/foo`).  
3. Commit your changes (`git commit -m "Add foo"`).  
4. Push to your fork (`git push origin feature/foo`).  
5. Open a Pull Request.

All contributions must include tests and documentation updates.

---

## License

MIT © shadcn & ported by Huntabyte & CokaKoala

---