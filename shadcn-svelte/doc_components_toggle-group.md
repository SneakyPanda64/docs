# Toggle Group – shadcn‑svelte

A set of two‑state buttons that can be toggled on or off.  
The component is fully accessible, supports single or multiple selection, and can be styled with the built‑in variants.

> **Tip** – All examples below use the default Tailwind CSS configuration that ships with shadcn‑svelte.  
> If you’re using a different styling system, adjust the class names accordingly.

---

## Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add toggle-group
```

> The command will add the component files to your `$lib/components/ui/toggle-group` folder and update your `components.json`.

### Manual

1. Copy the `toggle-group` folder from the repository into `$lib/components/ui/`.
2. Add the component to your `components.json`:

```json
{
  "name": "toggle-group",
  "path": "$lib/components/ui/toggle-group/index.js"
}
```

---

## Usage

```svelte
<script lang="ts">
  import * as ToggleGroup from "$lib/components/ui/toggle-group/index.js";
</script>

<ToggleGroup.Root type="single">
  <ToggleGroup.Item value="a">A</ToggleGroup.Item>
  <ToggleGroup.Item value="b">B</ToggleGroup.Item>
  <ToggleGroup.Item value="c">C</ToggleGroup.Item>
</ToggleGroup.Root>
```

### Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `type` | `"single" \| "multiple"` | `"single"` | Determines whether only one item can be selected or multiple items can be toggled. |
| `variant` | `"default" \| "outline" \| "ghost"` | `"default"` | Visual style of the toggle group. |
| `size` | `"default" \| "sm" \| "lg"` | `"default"` | Size of the toggle items. |
| `disabled` | `boolean` | `false` | Disables the entire group. |
| `value` | `string \| string[]` | `undefined` | Controlled value(s). |
| `onChange` | `(value: string | string[]) => void` | – | Callback when the selection changes. |

---

## API Reference

### `ToggleGroup.Root`

The container that manages the state of the group.

```svelte
<ToggleGroup.Root
  type="single"          // or "multiple"
  variant="outline"      // "default", "outline", or "ghost"
  size="sm"              // "default", "sm", or "lg"
  disabled={false}
  value={selectedValue}
  onChange={handleChange}
>
  <!-- ToggleGroup.Item children -->
</ToggleGroup.Root>
```

### `ToggleGroup.Item`

Individual toggle button.

```svelte
<ToggleGroup.Item value="bold" aria-label="Toggle bold">
  <BoldIcon class="h-4 w-4" />
</ToggleGroup.Item>
```

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `value` | `string` | – | The value that identifies this item. |
| `aria-label` | `string` | – | Accessible label for screen readers. |
| `disabled` | `boolean` | `false` | Disables this specific item. |

---

## Examples

Below are a collection of common use‑cases. Copy the code into your Svelte file to see the component in action.

### 1. Default

```svelte
<script lang="ts">
  import * as ToggleGroup from "$lib/components/ui/toggle-group/index.js";
</script>

<ToggleGroup.Root type="multiple">
  <ToggleGroup.Item value="bold" aria-label="Toggle bold">
    <BoldIcon class="h-4 w-4" />
  </ToggleGroup.Item>
  <ToggleGroup.Item value="italic" aria-label="Toggle italic">
    <ItalicIcon class="h-4 w-4" />
  </ToggleGroup.Item>
  <ToggleGroup.Item value="underline" aria-label="Toggle underline">
    <UnderlineIcon class="h-4 w-4" />
  </ToggleGroup.Item>
</ToggleGroup.Root>
```

### 2. Outline Variant

```svelte
<script lang="ts">
  import * as ToggleGroup from "$lib/components/ui/toggle-group/index.js";
</script>

<ToggleGroup.Root variant="outline" type="multiple">
  <ToggleGroup.Item value="bold" aria-label="Toggle bold">
    <BoldIcon class="h-4 w-4" />
  </ToggleGroup.Item>
  <ToggleGroup.Item value="italic" aria-label="Toggle italic">
    <ItalicIcon class="h-4 w-4" />
  </ToggleGroup.Item>
  <ToggleGroup.Item value="underline" aria-label="Toggle underline">
    <UnderlineIcon class="h-4 w-4" />
  </ToggleGroup.Item>
</ToggleGroup.Root>
```

### 3. Single‑Select

```svelte
<script lang="ts">
  import * as ToggleGroup from "$lib/components/ui/toggle-group/index.js";
</script>

<ToggleGroup.Root type="single">
  <ToggleGroup.Item value="a">A</ToggleGroup.Item>
  <ToggleGroup.Item value="b">B</ToggleGroup.Item>
  <ToggleGroup.Item value="c">C</ToggleGroup.Item>
</ToggleGroup.Root>
```

### 4. Small Size

```svelte
<script lang="ts">
  import * as ToggleGroup from "$lib/components/ui/toggle-group/index.js";
</script>

<ToggleGroup.Root size="sm" type="multiple">
  <ToggleGroup.Item value="bold" aria-label="Toggle bold">
    <BoldIcon class="h-3 w-3" />
  </ToggleGroup.Item>
  <ToggleGroup.Item value="italic" aria-label="Toggle italic">
    <ItalicIcon class="h-3 w-3" />
  </ToggleGroup.Item>
  <ToggleGroup.Item value="underline" aria-label="Toggle underline">
    <UnderlineIcon class="h-3 w-3" />
  </ToggleGroup.Item>
</ToggleGroup.Root>
```

### 5. Large Size

```svelte
<script lang="ts">
  import * as ToggleGroup from "$lib/components/ui/toggle-group/index.js";
</script>

<ToggleGroup.Root size="lg" type="multiple">
  <ToggleGroup.Item value="bold" aria-label="Toggle bold">
    <BoldIcon class="h-6 w-6" />
  </ToggleGroup.Item>
  <ToggleGroup.Item value="italic" aria-label="Toggle italic">
    <ItalicIcon class="h-6 w-6" />
  </ToggleGroup.Item>
  <ToggleGroup.Item value="underline" aria-label="Toggle underline">
    <UnderlineIcon class="h-6 w-6" />
  </ToggleGroup.Item>
</ToggleGroup.Root>
```

### 6. Disabled Group

```svelte
<script lang="ts">
  import * as ToggleGroup from "$lib/components/ui/toggle-group/index.js";
</script>

<ToggleGroup.Root disabled type="multiple">
  <ToggleGroup.Item value="bold" aria-label="Toggle bold">
    <BoldIcon class="h-4 w-4" />
  </ToggleGroup.Item>
  <ToggleGroup.Item value="italic" aria-label="Toggle italic">
    <ItalicIcon class="h-4 w-4" />
  </ToggleGroup.Item>
  <ToggleGroup.Item value="underline" aria-label="Toggle underline">
    <UnderlineIcon class="h-4 w-4" />
  </ToggleGroup.Item>
</ToggleGroup.Root>
```

> **Note** – The `Textarea` and `Toggle` examples referenced in the original documentation are omitted here because they were not provided. Feel free to add your own custom examples following the patterns above.

---

## Contributing

If you’d like to add new variants, sizes, or improve accessibility, feel free to open a pull request. All contributions are welcome!

---

## License

MIT © shadcn‑svelte (ported to Svelte by Huntabyte & CokaKoala)