# shadcn‑Svelte – Component Library Documentation

> **shadcn‑Svelte** is a collection of beautifully‑designed, accessible UI components for Svelte, ported from the original shadcn‑React library.  
> Built by shadcn, ported to Svelte by **Huntabyte** & **CokaKoala**.

---

## Table of Contents

| Section                               | Description                                   |
| ------------------------------------- | --------------------------------------------- |
| [Getting Started](#getting-started)   | Installation & basic setup                    |
| [Components](#components)             | Overview of all components with live examples |
| - [Dashboard](#dashboard)             | Total revenue & subscription cards            |
| - [Upgrade Plan](#upgrade-plan)       | Subscription upgrade form                     |
| - [Team Members](#team-members)       | Table of collaborators                        |
| - [Cookie Settings](#cookie-settings) | Toggleable cookie preferences                 |
| - [Create Account](#create-account)   | Sign‑up form with social logins               |
| - [Chat](#chat)                       | Simple messaging UI                           |
| - [Report Issue](#report-issue)       | Issue reporting form                          |
| - [Move Goal](#move-goal)             | Goal‑setting slider                           |
| - [Payments](#payments)               | Payment status table                          |
| [Blocks](#blocks)                     | Re‑usable UI blocks                           |
| [Charts](#charts)                     | Data‑visualisation components                 |
| [Themes & Colors](#themes--colors)    | Theme switching & colour palette              |
| [Examples](#examples)                 | Full Svelte files for each component          |
| [Contributing](#contributing)         | How to help                                   |
| [License](#license)                   | MIT                                           |

---

## Getting Started

```bash
# Install via npm
npm i shadcn-svelte

# Or via yarn
yarn add shadcn-svelte
```

Add the global styles to your `app.html` or `main.svelte`:

```svelte
<script>
  import 'shadcn-svelte/styles.css';
</script>
```

---

## Components

Below you’ll find a quick‑start example for each component.  
All examples use the default theme; you can switch themes via the `ThemeProvider` component (see _Themes & Colors_).

### Dashboard

A simple card layout showing revenue and subscription stats.

```svelte
<script>
  import { Card, CardHeader, CardTitle, CardContent } from 'shadcn-svelte';
</script>

<div class="grid grid-cols-1 md:grid-cols-2 gap-4">
  <Card>
    <CardHeader>
      <CardTitle>Total Revenue</CardTitle>
    </CardHeader>
    <CardContent class="text-4xl font-bold">$15,231.89</CardContent>
    <CardContent class="text-green-600">+20.1% from last month</CardContent>
  </Card>

  <Card>
    <CardHeader>
      <CardTitle>Subscriptions</CardTitle>
    </CardHeader>
    <CardContent class="text-4xl font-bold">+2,350</CardContent>
    <CardContent class="text-green-600">+180.1% from last month</CardContent>
  </Card>
</div>
```

---

### Upgrade Plan

A subscription upgrade form with validation hints.

```svelte
<script>
  import { Input, Select, Button, Checkbox, Label, Form, FormField } from 'shadcn-svelte';
  import { onMount } from 'svelte';

  let plan = 'starter';
  const plans = [
    { value: 'starter', label: 'Starter Plan – Perfect for small businesses.' },
    { value: 'pro', label: 'Pro Plan – More features and storage.' }
  ];
</script>

<Form>
  <FormField>
    <Label for="name">Name</Label>
    <Input id="name" placeholder="Your full name" required />
  </FormField>

  <FormField>
    <Label for="email">Email</Label>
    <Input id="email" type="email" placeholder="you@example.com" required />
  </FormField>

  <FormField>
    <Label for="card">Card Number</Label>
    <Input id="card" placeholder="•••• •••• •••• ••••" required />
  </FormField>

  <FormField>
    <Label for="plan">Plan</Label>
    <Select id="plan" bind:value={plan}>
      {#each plans as p}
        <option value={p.value}>{p.label}</option>
      {/each}
    </Select>
  </FormField>

  <FormField>
    <Checkbox id="terms" required />
    <Label for="terms">I agree to the terms and conditions</Label>
  </FormField>

  <FormField>
    <Checkbox id="email" />
    <Label for="email">Allow us to send you emails</Label>
  </FormField>

  <div class="flex justify-end gap-2 mt-4">
    <Button variant="outline">Cancel</Button>
    <Button type="submit">Upgrade Plan</Button>
  </div>
</Form>
```

---

### Team Members

A table listing collaborators with roles.

```svelte
<script>
  import { Table, TableHeader, TableRow, TableHead, TableBody, TableCell } from 'shadcn-svelte';
</script>

<Table>
  <TableHeader>
    <TableRow>
      <TableHead>Name</TableHead>
      <TableHead>Email</TableHead>
      <TableHead>Role</TableHead>
    </TableRow>
  </TableHeader>

  <TableBody>
    <TableRow>
      <TableCell>Sofia Davis</TableCell>
      <TableCell>m@example.com</TableCell>
      <TableCell>Owner</TableCell>
    </TableRow>
    <TableRow>
      <TableCell>Jackson Lee</TableCell>
      <TableCell>p@example.com</TableCell>
      <TableCell>Member</TableCell>
    </TableRow>
    <TableRow>
      <TableCell>Isabella Nguyen</TableCell>
      <TableCell>i@example.com</TableCell>
      <TableCell>Member</TableCell>
    </TableRow>
  </TableBody>
</Table>
```

---

### Cookie Settings

Toggle switches for cookie preferences.

```svelte
<script>
  import { Switch, Label } from 'shadcn-svelte';
  let strictlyNecessary = true;
  let functionalCookies = false;
</script>

<div class="space-y-4">
  <div class="flex items-center space-x-2">
    <Switch bind:checked={strictlyNecessary} />
    <Label>Strictly Necessary</Label>
  </div>
  <p class="text-sm text-muted-foreground">
    These cookies are essential in order to use the website and use its features.
  </p>

  <div class="flex items-center space-x-2">
    <Switch bind:checked={functionalCookies} />
    <Label>Functional Cookies</Label>
  </div>
  <p class="text-sm text-muted-foreground">
    These cookies allow the website to provide personalized functionality.
  </p>

  <Button variant="outline">Save preferences</Button>
</div>
```

---

### Create Account

Sign‑up form with social login options.

```svelte
<script>
  import { Input, Button, Separator } from 'shadcn-svelte';
</script>

<div class="max-w-sm mx-auto space-y-4">
  <h2 class="text-2xl font-semibold">Create an account</h2>
  <p class="text-sm text-muted-foreground">
    Enter your email below to create your account
  </p>

  <Button variant="outline" class="w-full flex items-center justify-center gap-2">
    <svg class="h-4 w-4" viewBox="0 0 24 24"><!-- GitHub icon --></svg>
    GitHub
  </Button>

  <Button variant="outline" class="w-full flex items-center justify-center gap-2">
    <svg class="h-4 w-4" viewBox="0 0 24 24"><!-- Google icon --></svg>
    Google
  </Button>

  <Separator>OR CONTINUE WITH</Separator>

  <Input placeholder="Email" type="email" required />
  <Input placeholder="Password" type="password" required />

  <Button class="w-full mt-4">Create account</Button>
</div>
```

---

### Chat

A minimal chat UI.

```svelte
<script>
  import { Avatar, AvatarImage, AvatarFallback } from 'shadcn-svelte';
  let messages = [
    { id: 1, text: "Hi, how can I help you today?", from: "bot" },
    { id: 2, text: "Hey, I'm having trouble with my account.", from: "user" },
    { id: 3, text: "What seems to be the problem?", from: "bot" },
    { id: 4, text: "I can't log in.", from: "user" }
  ];
  let newMessage = '';
</script>

<div class="flex flex-col h-96 border rounded-lg p-4 space-y-4 overflow-y-auto">
  {#each messages as msg}
    <div class="flex {msg.from === 'user' ? 'justify-end' : 'justify-start'}">
      {#if msg.from === 'bot'}
        <Avatar class="mr-2">
          <AvatarImage src="/bot.png" alt="Bot" />
          <AvatarFallback>🤖</AvatarFallback>
        </Avatar>
      {/if}
      <div class="max-w-xs p-2 rounded bg-muted">
        {msg.text}
      </div>
      {#if msg.from === 'user'}
        <Avatar class="ml-2">
          <AvatarImage src="/user.png" alt="You" />
          <AvatarFallback>👤</AvatarFallback>
        </Avatar>
      {/if}
    </div>
  {/each}
</div>

<div class="flex space-x-2 mt-2">
  <Input bind:value={newMessage} placeholder="Type a message..." />
  <Button on:click={() => { messages = [...messages, { id: messages.length + 1, text: newMessage, from: 'user' }]; newMessage = ''; }}>Send</Button>
</div>
```

---

### Report Issue

Form for submitting support tickets.

```svelte
<script>
  import { Select, Input, Textarea, Button, Label, Form, FormField } from 'shadcn-svelte';
  let area = 'billing';
  let severity = '2';
  let subject = '';
  let description = '';
</script>

<Form>
  <FormField>
    <Label for="area">Area</Label>
    <Select id="area" bind:value={area}>
      <option value="billing">Billing</option>
      <option value="security">Security</option>
      <option value="feature">Feature Request</option>
    </Select>
  </FormField>

  <FormField>
    <Label for="severity">Security Level</Label>
    <Select id="severity" bind:value={severity}>
      <option value="1">Severity 1</option>
      <option value="2">Severity 2</option>
      <option value="3">Severity 3</option>
    </Select>
  </FormField>

  <FormField>
    <Label for="subject">Subject</Label>
    <Input id="subject" bind:value={subject} required />
  </FormField>

  <FormField>
    <Label for="description">Description</Label>
    <Textarea id="description" bind:value={description} rows="4" required />
  </FormField>

  <div class="flex justify-end gap-2 mt-4">
    <Button variant="outline">Cancel</Button>
    <Button type="submit">Submit</Button>
  </div>
</Form>
```

---

### Move Goal

Goal‑setting slider for daily activity.

```svelte
<script>
  import { Slider, Label } from 'shadcn-svelte';
  let calories = 350;
  let minutes = 30;
</script>

<div class="space-y-6">
  <div>
    <Label>Set your daily activity goal</Label>
    <div class="flex items-center justify-between mt-2">
      <span>Calories/day</span>
      <span>{calories}</span>
    </div>
    <Slider min={100} max={1000} step={50} bind:value={calories} />
  </div>

  <div>
    <Label>Exercise Minutes</Label>
    <div class="flex items-center justify-between mt-2">
      <span>Minutes</span>
      <span>{minutes}</span>
    </div>
    <Slider min={0} max={120} step={5} bind:value={minutes} />
  </div>
</div>
```

---

### Payments

Table of payment status.

```svelte
<script>
  import { Table, TableHeader, TableRow, TableHead, TableBody, TableCell, Badge } from 'shadcn-svelte';
  const payments = [
    { id: 1, email: 'ken99@example.com', amount: '$316.00', status: 'Success' },
    { id: 2, email: 'abe45@example.com', amount: '$242.00', status: 'Success' },
    { id: 3, email: 'monserrat44@example.com', amount: '$837.00', status: 'Processing' },
    { id: 4, email: 'carmella@example.com', amount: '$721.00', status: 'Failed' },
    { id: 5, email: 'jason78@example.com', amount: '$450.00', status: 'Pending' },
    { id: 6, email: 'sarah23@example.com', amount: '$1,280.00', status: 'Success' }
  ];
</script>

<Table>
  <TableHeader>
    <TableRow>
      <TableHead>Status</TableHead>
      <TableHead>Email</TableHead>
      <TableHead>Amount</TableHead>
    </TableRow>
  </TableHeader>

  <TableBody>
    {#each payments as p}
      <TableRow>
        <TableCell>
          <Badge variant={p.status === 'Success' ? 'success' : p.status === 'Processing' ? 'warning' : 'destructive'}>
            {p.status}
          </Badge>
        </TableCell>
```
