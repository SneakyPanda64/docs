# Drawer – Svelte UI Component

The **Drawer** component is a lightweight, accessible panel that slides in from the side of the viewport.  
It is built on top of **Vaul Svelte** (a Svelte port of Vaul by Emil Kowalski) and follows the same API as the original Vaul component.

> **Tip** – The Drawer can be combined with the `Dialog` component to create a responsive dialog that behaves as a modal on desktop and as a drawer on mobile.

---

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [API](#api)
- [Examples](#examples)
  - [Simple Drawer](#simple-drawer)
  - [Drawer with a Chart](#drawer-with-a-chart)
  - [Responsive Dialog](#responsive-dialog)
- [Contributing](#contributing)
- [License](#license)

---

## Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add drawer
```

> The command will add the component files to your `$lib/components/ui/drawer` directory and update your `components.json`.

### Manual

1. Copy the `drawer` folder from the repository into `$lib/components/ui/`.
2. Add the following import to your `components.json`:

```json
{
  "drawer": {
    "path": "$lib/components/ui/drawer/index.js"
  }
}
```

---

## Usage

```svelte
<script lang="ts">
  import * as Drawer from "$lib/components/ui/drawer/index.js";
</script>

<Drawer.Root>
  <Drawer.Trigger>Open</Drawer.Trigger>

  <Drawer.Content>
    <Drawer.Header>
      <Drawer.Title>Are you sure absolutely sure?</Drawer.Title>
      <Drawer.Description>This action cannot be undone.</Drawer.Description>
    </Drawer.Header>

    <Drawer.Footer>
      <Button>Submit</Button>
      <Drawer.Close>Cancel</Drawer.Close>
    </Drawer.Footer>
  </Drawer.Content>
</Drawer.Root>
```

> **Note** – The `Drawer` component exposes the following sub‑components:
> - `Root`
> - `Trigger`
> - `Content`
> - `Header`
> - `Title`
> - `Description`
> - `Footer`
> - `Close`

---

## API

| Component | Props | Description |
|-----------|-------|-------------|
| `Drawer.Root` | `asChild?: boolean` | Root wrapper. |
| `Drawer.Trigger` | `asChild?: boolean` | Element that opens the drawer. |
| `Drawer.Content` | `asChild?: boolean` | The panel that slides in. |
| `Drawer.Header` | `asChild?: boolean` | Header container. |
| `Drawer.Title` | `asChild?: boolean` | Title text. |
| `Drawer.Description` | `asChild?: boolean` | Description text. |
| `Drawer.Footer` | `asChild?: boolean` | Footer container. |
| `Drawer.Close` | `asChild?: boolean` | Element that closes the drawer. |

All components accept the `asChild` prop to render a custom element while preserving the component’s behavior.

---

## Examples

### Simple Drawer

```svelte
<script lang="ts">
  import * as Drawer from "$lib/components/ui/drawer/index.js";
</script>

<Drawer.Root>
  <Drawer.Trigger>Open</Drawer.Trigger>

  <Drawer.Content>
    <Drawer.Header>
      <Drawer.Title>Are you sure absolutely sure?</Drawer.Title>
      <Drawer.Description>This action cannot be undone.</Drawer.Description>
    </Drawer.Header>

    <Drawer.Footer>
      <Button>Submit</Button>
      <Drawer.Close>Cancel</Drawer.Close>
    </Drawer.Footer>
  </Drawer.Content>
</Drawer.Root>
```

---

### Drawer with a Chart

```svelte
<script lang="ts">
  import MinusIcon from "@lucide/svelte/icons/minus";
  import PlusIcon from "@lucide/svelte/icons/plus";
  import * as Drawer from "$lib/components/ui/drawer/index.js";
  import { Button, buttonVariants } from "$lib/components/ui/button/index.js";
  import { BarChart, type ChartContextValue } from "layerchart";
  import { scaleBand } from "d3-scale";
  import { cubicInOut } from "svelte/easing";

  const data = [
    { goal: 400 },
    { goal: 300 },
    { goal: 200 },
    { goal: 300 },
    { goal: 200 },
    { goal: 278 },
    { goal: 189 },
    { goal: 239 },
    { goal: 300 },
    { goal: 200 },
    { goal: 278 },
    { goal: 189 },
    { goal: 349 }
  ];

  let goal = $state(350);

  function handleClick(adjustment: number) {
    goal = Math.max(200, Math.min(400, goal + adjustment));
  }

  let context = $state<ChartContextValue>();
</script>

<Drawer.Root>
  <Drawer.Trigger class={buttonVariants({ variant: "outline" })}>
    Open Drawer
  </Drawer.Trigger>

  <Drawer.Content>
    <div class="mx-auto w-full max-w-sm">
      <Drawer.Header>
        <Drawer.Title>Move Goal</Drawer.Title>
        <Drawer.Description>Set your daily activity goal.</Drawer.Description>
      </Drawer.Header>

      <div class="p-4 pb-0">
        <div class="flex items-center justify-center space-x-2">
          <Button
            variant="outline"
            size="icon"
            class="size-8 shrink-0 rounded-full"
            onclick={() => handleClick(-10)}
            disabled={goal <= 200}
          >
            <MinusIcon />
            <span class="sr-only">Decrease</span>
          </Button>

          <div class="flex-1 text-center">
            <div class="text-7xl font-bold tracking-tighter">{goal}</div>
            <div class="text-muted-foreground text-[0.70rem] uppercase">
              Calories/day
            </div>
          </div>

          <Button
            variant="outline"
            size="icon"
            class="size-8 shrink-0 rounded-full"
            onclick={() => handleClick(10)}
            disabled={goal >= 400}
          >
            <PlusIcon />
            <span class="sr-only">Increase</span>
          </Button>
        </div>

        <div class="mt-3 h-[120px]">
          <div class="h-full w-full">
            <BarChart
              bind:context
              data={data.map((d, i) => ({ goal: d.goal, index: i }))}
              y="goal"
              x="index"
              xScale={scaleBand().padding(0.25)}
              axis={false}
              tooltip={false}
              props={{
                bars: {
                  stroke: "none",
                  rounded: "all",
                  radius: 4,
                  initialY: context?.height,
                  initialHeight: 0,
                  motion: {
                    x: { type: "tween", duration: 500, easing: cubicInOut },
                    width: { type: "tween", duration: 500, easing: cubicInOut },
                    height: {
                      type: "tween",
                      duration: 500,
                      easing: cubicInOut
                    },
                    y: { type: "tween", duration: 500, easing: cubicInOut }
                  },
                  fill: "var(--color-foreground)",
                  fillOpacity: 0.9
                },
                highlight: { area: { fill: "none" } }
              }}
            />
          </div>
        </div>
      </div>

      <Drawer.Footer>
        <Button>Submit</Button>
        <Drawer.Close class={buttonVariants({ variant: "outline" })}>
          Cancel
        </Drawer.Close>
      </Drawer.Footer>
    </div>
  </Drawer.Content>
</Drawer.Root>
```

---

### Responsive Dialog

This example renders a `Dialog` on desktop and a `Drawer` on mobile.  
The `ResponsiveDialog` component is a thin wrapper that chooses the appropriate component based on the viewport width.

```svelte
<script lang="ts">
  import * as Dialog from "$lib/components/ui/dialog/index.js";
  import * as Drawer from "$lib/components/ui/drawer/index.js";
  import { useMediaQuery } from "$lib/hooks/useMediaQuery.js";
  import { Button } from "$lib/components/ui/button/index.js";

  const isMobile = useMediaQuery("(max-width: 768px)");
</script>

{#if isMobile}
  <Drawer.Root>
    <Drawer.Trigger>Open</Drawer.Trigger>
    <Drawer.Content>
      <Drawer.Header>
        <Drawer.Title>Edit Profile</Drawer.Title>
      </Drawer.Header>
      <!-- Drawer body -->
      <Drawer.Footer>
        <Button>Save</Button>
        <Drawer.Close>Cancel</Drawer.Close>
      </Drawer.Footer>
    </Drawer.Content>
  </Drawer.Root>
{:else}
  <Dialog.Root>
    <Dialog.Trigger>Open</Dialog.Trigger>
    <Dialog.Content>
      <Dialog.Header>
        <Dialog.Title>Edit Profile</Dialog.Title>
      </Dialog.Header>
      <!-- Dialog body -->
      <Dialog.Footer>
        <Button>Save</Button>
        <Dialog.Close>Cancel</Dialog.Close>
      </Dialog.Footer>
    </Dialog.Content>
  </Dialog.Root>
{/if}
```

---

## Contributing

Feel free to open issues or pull requests on the [GitHub repository](https://github.com/shadcn-svelte/shadcn-svelte).  
All contributions that follow the existing style guidelines are welcome.

---

## License

MIT © shadcn-svelte

---