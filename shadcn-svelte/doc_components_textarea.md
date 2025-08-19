# Textarea Component – shadcn‑svelte

A lightweight, fully‑styled `<Textarea>` component that follows the shadcn‑svelte design system.  
It supports all the same props and variants as the original shadcn‑react component, but is written in Svelte and uses Tailwind CSS for styling.

> **Tip** – The component is available in the `ui/textarea` package of the shadcn‑svelte registry.  
> Import it from `$lib/components/ui/textarea/index.js` (or the path you configured in your project).

---

## Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add textarea
```

> The CLI will add the component files, Tailwind utilities, and update your `components.json` automatically.

### Manual

```bash
# npm
npm i @shadcn/svelte

# yarn
yarn add @shadcn/svelte

# bun
bun add @shadcn/svelte
```

Then copy the component files from the registry into your project or import directly from the package.

---

## Usage

```svelte
<script lang="ts">
  import { Textarea } from "$lib/components/ui/textarea/index.js";
</script>

<Textarea placeholder="Type your message here." />
```

The component accepts all standard `<textarea>` attributes (`value`, `name`, `id`, `rows`, `cols`, etc.) and the following shadcn‑specific props:

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `variant` | `"default" | "outline" | "filled"` | Visual style of the textarea. |
| `size` | `"sm" | "md" | "lg"` | Padding and font size. |
| `disabled` | `boolean` | `false` | Disables the textarea. |
| `label` | `string` | – | Adds a floating label. |
| `helpText` | `string` | – | Adds helper text below the textarea. |
| `error` | `string` | – | Shows an error state with message. |
| `class` | `string` | – | Custom Tailwind classes. |

---

## Examples

Below are a collection of common use‑cases. Copy the code into your Svelte file to see the component in action.

### 1. Default

```svelte
<script lang="ts">
  import { Textarea } from "$lib/components/ui/textarea/index.js";
</script>

<Textarea placeholder="Type your message here." />
```

### 2. Disabled

```svelte
<script lang="ts">
  import { Textarea } from "$lib/components/ui/textarea/index.js";
</script>

<Textarea placeholder="Disabled textarea" disabled />
```

### 3. With Label

```svelte
<script lang="ts">
  import { Textarea } from "$lib/components/ui/textarea/index.js";
</script>

<Textarea label="Your message" placeholder="Type your message here." />
```

### 4. With Text (Helper)

```svelte
<script lang="ts">
  import { Textarea } from "$lib/components/ui/textarea/index.js";
</script>

<Textarea
  label="Your Message"
  placeholder="Type your message here."
  helpText="Your message will be copied to the support team."
/>
```

### 5. With Button (Form)

```svelte
<script lang="ts">
  import { Textarea } from "$lib/components/ui/textarea/index.js";
  import { Button } from "$lib/components/ui/button/index.js";
</script>

<form class="space-y-4">
  <Textarea
    label="Bio"
    placeholder="You can @mention other users and organizations."
  />
  <Button type="submit">Submit</Button>
</form>
```

### 6. Tabs & Toggle Group (Advanced)

> *This example demonstrates how the `<Textarea>` can be used inside a tabbed interface or a toggle group.*

```svelte
<script lang="ts">
  import { Tabs, TabList, Tab, TabPanel } from "$lib/components/ui/tabs/index.js";
  import { ToggleGroup, ToggleGroupItem } from "$lib/components/ui/toggle-group/index.js";
  import { Textarea } from "$lib/components/ui/textarea/index.js";
</script>

<Tabs defaultValue="description">
  <TabList>
    <Tab value="description">Description</Tab>
    <Tab value="details">Details</Tab>
  </TabList>

  <TabPanel value="description">
    <Textarea placeholder="Describe your project..." />
  </TabPanel>

  <TabPanel value="details">
    <ToggleGroup type="single" defaultValue="public">
      <ToggleGroupItem value="public">Public</ToggleGroupItem>
      <ToggleGroupItem value="private">Private</ToggleGroupItem>
    </ToggleGroup>
    <Textarea placeholder="Add more details..." />
  </TabPanel>
</Tabs>
```

---

## Customization

The component is fully Tailwind‑based, so you can override any style by passing a `class` prop or by extending the Tailwind config.

```svelte
<Textarea
  placeholder="Custom styled"
  class="bg-gray-50 dark:bg-gray-800 border-2 border-blue-500"
/>
```

---

## Accessibility

- The component automatically forwards `aria-*` attributes to the underlying `<textarea>`.
- When a `label` prop is provided, it is rendered as a `<label>` element linked to the textarea via `id`/`for`.
- Error states are announced via `aria-invalid` and `aria-describedby`.

---

## Contributing

If you’d like to add new variants, improve the styling, or fix bugs, feel free to open a PR in the shadcn‑svelte repository. All contributions are welcome!

---