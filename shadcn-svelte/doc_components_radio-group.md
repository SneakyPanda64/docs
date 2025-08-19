# Radio Group – shadcn‑svelte

A **Radio Group** is a set of checkable buttons (radio buttons) where only one button can be selected at a time.  
This component is part of the shadcn‑svelte UI library, a Svelte port of the popular shadcn‑ui React components.

> **Tip** – All examples below use the latest version of shadcn‑svelte (`v0.5.x`).  
> If you’re using an older version, the API is identical.

---

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [API Reference](#api-reference)
- [Examples](#examples)
- [Related Components](#related-components)

---

## Installation

### CLI

```bash
# Add the radio‑group component to your project
pnpm dlx shadcn-svelte@latest add radio-group
```

> The CLI will automatically install the component and add the necessary Tailwind CSS utilities.

### Manual

If you prefer to add the component manually:

```bash
# Install the package
pnpm add @shadcn-svelte/radio-group
```

Then import the component in your Svelte file:

```svelte
<script lang="ts">
  import * as RadioGroup from "$lib/components/ui/radio-group/index.js";
  import { Label } from "$lib/components/ui/label/index.js";
</script>
```

---

## Usage

```svelte
<script lang="ts">
  import * as RadioGroup from "$lib/components/ui/radio-group/index.js";
  import { Label } from "$lib/components/ui/label/index.js";
</script>

<RadioGroup.Root value="option-one">
  <div class="flex items-center space-x-2">
    <RadioGroup.Item value="option-one" id="option-one" />
    <Label for="option-one">Option One</Label>
  </div>

  <div class="flex items-center space-x-2">
    <RadioGroup.Item value="option-two" id="option-two" />
    <Label for="option-two">Option Two</Label>
  </div>
</RadioGroup.Root>
```

### Props

| Component | Prop | Type | Default | Description |
|-----------|------|------|---------|-------------|
| `RadioGroup.Root` | `value` | `string` | – | The currently selected value. |
| | `on:change` | `Event` | – | Fired when the selected value changes. |
| `RadioGroup.Item` | `value` | `string` | – | The value represented by this radio button. |
| | `id` | `string` | – | The DOM id used for the `<label for="…">`. |
| | `disabled` | `boolean` | `false` | Disables the radio button. |

> **Note** – The component uses Tailwind CSS for styling. Ensure Tailwind is configured in your project.

---

## API Reference

### `RadioGroup.Root`

A container that manages the state of the radio group.

```svelte
<RadioGroup.Root value="selectedValue" on:change={handleChange}>
  <!-- RadioGroup.Item components -->
</RadioGroup.Root>
```

- **`value`** – The value of the currently selected radio button.
- **`on:change`** – Event fired when the selection changes. The event detail contains `{ value: string }`.

### `RadioGroup.Item`

Represents an individual radio button.

```svelte
<RadioGroup.Item value="someValue" id="some-id" />
```

- **`value`** – The value this radio button represents.
- **`id`** – The DOM id used for the `<label for="…">`.
- **`disabled`** – Optional boolean to disable the button.

---

## Examples

### 1. Comfort Settings

```svelte
<script lang="ts">
  import * as RadioGroup from "$lib/components/ui/radio-group/index.js";
  import { Label } from "$lib/components/ui/label/index.js";
</script>

<RadioGroup.Root value="comfortable">
  <div class="flex items-center space-x-2">
    <RadioGroup.Item value="default" id="r1" />
    <Label for="r1">Default</Label>
  </div>

  <div class="flex items-center space-x-2">
    <RadioGroup.Item value="comfortable" id="r2" />
    <Label for="r2">Comfortable</Label>
  </div>

  <div class="flex items-center space-x-2">
    <RadioGroup.Item value="compact" id="r3" />
    <Label for="r3">Compact</Label>
  </div>
</RadioGroup.Root>
```

### 2. Simple Two‑Option Group

```svelte
<script lang="ts">
  import * as RadioGroup from "$lib/components/ui/radio-group/index.js";
  import { Label } from "$lib/components/ui/label/index.js";
</script>

<RadioGroup.Root value="option-one">
  <div class="flex items-center space-x-2">
    <RadioGroup.Item value="option-one" id="option-one" />
    <Label for="option-one">Option One</Label>
  </div>

  <div class="flex items-center space-x-2">
    <RadioGroup.Item value="option-two" id="option-two" />
    <Label for="option-two">Option Two</Label>
  </div>
</RadioGroup.Root>
```

### 3. Disabled Radio Button

```svelte
<script lang="ts">
  import * as RadioGroup from "$lib/components/ui/radio-group/index.js";
  import { Label } from "$lib/components/ui/label/index.js";
</script>

<RadioGroup.Root value="enabled">
  <div class="flex items-center space-x-2">
    <RadioGroup.Item value="enabled" id="enabled" />
    <Label for="enabled">Enabled</Label>
  </div>

  <div class="flex items-center space-x-2">
    <RadioGroup.Item value="disabled" id="disabled" disabled />
    <Label for="disabled">Disabled</Label>
  </div>
</RadioGroup.Root>
```

---

## Related Components

- **`<Label>`** – Accessible label component used with radio items.
- **`<RadioGroup.Root>`** – The container that manages state.
- **`<RadioGroup.Item>`** – Individual radio button.

---

### Quick Reference

```svelte
<script lang="ts">
  import * as RadioGroup from "$lib/components/ui/radio-group/index.js";
  import { Label } from "$lib/components/ui/label/index.js";
</script>

<RadioGroup.Root value="selected">
  <div class="flex items-center space-x-2">
    <RadioGroup.Item value="selected" id="selected" />
    <Label for="selected">Selected</Label>
  </div>
  <!-- Add more items as needed -->
</RadioGroup.Root>
```

> **Tip** – Wrap each `RadioGroup.Item` and its `Label` in a flex container to align them horizontally.

---

**Happy coding!** 🚀