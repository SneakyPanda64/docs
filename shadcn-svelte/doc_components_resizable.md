# Resizable – shadcn‑svelte

The **Resizable** component lets you create resizable panels or layouts with keyboard support.  
It is a thin wrapper around [PaneForge](https://github.com/huntabyte/paneforge) and exposes the same API.

> **Tip** – For a full list of props and advanced features, see the [PaneForge documentation](https://github.com/huntabyte/paneforge).

---

## Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add resizable
```

> The same command works with `npm`, `yarn`, or `bun`.

### Manual

```bash
# Add the component to your project
npm install @shadcn/svelte
```

Then import the component in your Svelte file:

```svelte
<script lang="ts">
  import * as Resizable from "$lib/components/ui/resizable/index.js";
</script>
```

---

## Basic Usage

```svelte
<script lang="ts">
  import * as Resizable from "$lib/components/ui/resizable/index.js";
</script>

<Resizable.PaneGroup direction="horizontal">
  <Resizable.Pane>One</Resizable.Pane>
  <Resizable.Handle />
  <Resizable.Pane>Two</Resizable.Pane>
</Resizable.PaneGroup>
```

- `PaneGroup` – container that defines the resizing direction (`horizontal` or `vertical`).
- `Pane` – a resizable panel.  
  Use `defaultSize` (number or string) to set the initial size.
- `Handle` – the draggable separator between panels.

---

## Props

| Component | Prop | Type | Default | Description |
|-----------|------|------|---------|-------------|
| `PaneGroup` | `direction` | `"horizontal" \| "vertical"` | `horizontal` | Layout direction. |
| `Pane` | `defaultSize` | `number \| string` | `undefined` | Initial size of the pane. |
| `Handle` | `withHandle` | `boolean` | `false` | Show a visible handle. |

> All other props are forwarded to the underlying PaneForge component.

---

## Examples

### 1. Vertical Layout

```svelte
<script lang="ts">
  import * as Resizable from "$lib/components/ui/resizable/index.js";
</script>

<Resizable.PaneGroup direction="vertical">
  <Resizable.Pane>One</Resizable.Pane>
  <Resizable.Handle />
  <Resizable.Pane>Two</Resizable.Pane>
</Resizable.PaneGroup>
```

### 2. Custom Handle

```svelte
<script lang="ts">
  import * as Resizable from "$lib/components/ui/resizable/index.js";
</script>

<Resizable.PaneGroup direction="vertical">
  <Resizable.Pane>One</Resizable.Pane>
  <Resizable.Handle withHandle />
  <Resizable.Pane>Two</Resizable.Pane>
</Resizable.PaneGroup>
```

### 3. Nested Panes

```svelte
<script lang="ts">
  import * as Resizable from "$lib/components/ui/resizable/index.js";
</script>

<Resizable.PaneGroup direction="horizontal" class="max-w-md rounded-lg border">
  <Resizable.Pane defaultSize={50}>
    <div class="flex h-[200px] items-center justify-center p-6">
      <span class="font-semibold">One</span>
    </div>
  </Resizable.Pane>

  <Resizable.Handle />

  <Resizable.Pane defaultSize={50}>
    <Resizable.PaneGroup direction="vertical">
      <Resizable.Pane defaultSize={25}>
        <div class="flex h-full items-center justify-center p-6">
          <span class="font-semibold">Two</span>
        </div>
      </Resizable.Pane>

      <Resizable.Handle />

      <Resizable.Pane defaultSize={75}>
        <div class="flex h-full items-center justify-center p-6">
          <span class="font-semibold">Three</span>
        </div>
      </Resizable.Pane>
    </Resizable.PaneGroup>
  </Resizable.Pane>
</Resizable.PaneGroup>
```

---

## Theming & Dark Mode

The component inherits Tailwind CSS utilities.  
Add your own classes or use the provided `class` prop to style the panes, handles, and group.

---

## FAQ

- **Can I use this component in a SvelteKit project?**  
  Yes – just import it as shown above.

- **How do I hide the handle?**  
  Omit the `withHandle` prop or set it to `false`.

- **Is keyboard navigation supported?**  
  Yes, PaneForge provides full keyboard support out of the box.

---

## Changelog

See the main shadcn‑svelte changelog for updates related to the Resizable component.

---