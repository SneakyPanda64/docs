# shadcn‑svelte – Carousel Component

The **Carousel** component is a lightweight, fully‑featured carousel built on top of the [Embla Carousel](https://www.embla-carousel.com/) library.  
It is part of the shadcn‑svelte UI kit and can be used in any Svelte project that uses Tailwind CSS.

> **Tip** – All examples below use the default Tailwind configuration.  
> If you’re using a custom config, replace the utility classes accordingly.

---

## Table of Contents

- [Installation](#installation)
- [Basic Usage](#basic-usage)
- [Examples](#examples)
  - [Sizes](#sizes)
  - [Spacing](#spacing)
  - [Orientation](#orientation)
  - [Options](#options)
  - [API](#api)
  - [Events](#events)
  - [Plugins](#plugins)
- [API Reference](#api-reference)
- [License](#license)

---

## Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add carousel
```

> The same command works with `npm`, `yarn`, or `bun`.

### Manual

1. Install the component package:

   ```bash
   pnpm add @shadcn-svelte/carousel
   ```

2. Import the component in your Svelte file:

   ```svelte
   <script lang="ts">
     import * as Carousel from "$lib/components/ui/carousel/index.js";
   </script>
   ```

---

## Basic Usage

```svelte
<script lang="ts">
  import * as Carousel from "$lib/components/ui/carousel/index.js";
</script>

<Carousel.Root>
  <Carousel.Content>
    <Carousel.Item>Slide 1</Carousel.Item>
    <Carousel.Item>Slide 2</Carousel.Item>
    <Carousel.Item>Slide 3</Carousel.Item>
  </Carousel.Content>

  <Carousel.Previous />
  <Carousel.Next />
</Carousel.Root>
```

> **Note** – The `<Carousel.Previous />` and `<Carousel.Next />` components are optional.  
> If omitted, the carousel will still be swipeable but will not have navigation buttons.

---

## Examples

### Sizes

Set the width of each slide using Tailwind’s `basis-` utilities.

```svelte
<!-- 33 % of the carousel width -->
<Carousel.Root>
  <Carousel.Content>
    <Carousel.Item class="basis-1/3">Slide 1</Carousel.Item>
    <Carousel.Item class="basis-1/3">Slide 2</Carousel.Item>
    <Carousel.Item class="basis-1/3">Slide 3</Carousel.Item>
  </Carousel.Content>
</Carousel.Root>
```

#### Responsive

```svelte
<!-- 50 % on small screens, 33 % on larger screens -->
<Carousel.Root>
  <Carousel.Content>
    <Carousel.Item class="md:basis-1/2 lg:basis-1/3">Slide 1</Carousel.Item>
    <Carousel.Item class="md:basis-1/2 lg:basis-1/3">Slide 2</Carousel.Item>
    <Carousel.Item class="md:basis-1/2 lg:basis-1/3">Slide 3</Carousel.Item>
  </Carousel.Content>
</Carousel.Root>
```

---

### Spacing

Use `pl-` on each `<Carousel.Item>` and a negative `-ml-` on `<Carousel.Content>` to create gaps.

```svelte
<Carousel.Root>
  <Carousel.Content class="-ml-4">
    <Carousel.Item class="pl-4">Slide 1</Carousel.Item>
    <Carousel.Item class="pl-4">Slide 2</Carousel.Item>
    <Carousel.Item class="pl-4">Slide 3</Carousel.Item>
  </Carousel.Content>
</Carousel.Root>
```

#### Responsive

```svelte
<Carousel.Root>
  <Carousel.Content class="-ml-2 md:-ml-4">
    <Carousel.Item class="pl-2 md:pl-4">Slide 1</Carousel.Item>
    <Carousel.Item class="pl-2 md:pl-4">Slide 2</Carousel.Item>
    <Carousel.Item class="pl-2 md:pl-4">Slide 3</Carousel.Item>
  </Carousel.Content>
</Carousel.Root>
```

---

### Orientation

Set the carousel’s orientation via the `orientation` prop.

```svelte
<Carousel.Root orientation="vertical">
  <Carousel.Content>
    <Carousel.Item>Slide 1</Carousel.Item>
    <Carousel.Item>Slide 2</Carousel.Item>
    <Carousel.Item>Slide 3</Carousel.Item>
  </Carousel.Content>
</Carousel.Root>
```

---

### Options

Pass Embla options through the `opts` prop.

```svelte
<Carousel.Root
  opts={{
    align: "start",
    loop: true,
  }}
>
  <Carousel.Content>
    <Carousel.Item>Slide 1</Carousel.Item>
    <Carousel.Item>Slide 2</Carousel.Item>
    <Carousel.Item>Slide 3</Carousel.Item>
  </Carousel.Content>
</Carousel.Root>
```

> **Tip** – Refer to the [Embla Carousel docs](https://www.embla-carousel.com/docs/options/) for a full list of options.

---

### API

Expose the Embla API instance with `setApi` and use Svelte’s reactive state to track the current slide.

```svelte
<script lang="ts">
  import { type CarouselAPI } from "$lib/components/ui/carousel/context.js";
  import * as Carousel from "$lib/components/ui/carousel/index.js";

  let api = $state<CarouselAPI>();
  let current = $state(0);
  const count = $derived(api ? api.scrollSnapList().length : 0);

  $effect(() => {
    if (api) {
      current = api.selectedScrollSnap() + 1;
      api.on("select", () => {
        current = api!.selectedScrollSnap() + 1;
      });
    }
  });
</script>

<Carousel.Root setApi={(emblaApi) => (api = emblaApi)}>
  <Carousel.Content>
    <Carousel.Item>Slide 1</Carousel.Item>
    <Carousel.Item>Slide 2</Carousel.Item>
    <Carousel.Item>Slide 3</Carousel.Item>
  </Carousel.Content>
</Carousel.Root>

<p>Slide {current} of {count}</p>
```

---

### Events

Listen to Embla events via the API instance.

```svelte
<script lang="ts">
  import { type CarouselAPI } from "$lib/components/ui/carousel/context.js";
  import * as Carousel from "$lib/components/ui/carousel/index.js";

  let api = $state<CarouselAPI>();

  $effect(() => {
    if (api) {
      api.on("select", () => {
        console.log("Slide changed");
      });
    }
  });
</script>

<Carousel.Root setApi={(emblaApi) => (api = emblaApi)}>
  <Carousel.Content>
    <Carousel.Item>Slide 1</Carousel.Item>
    <Carousel.Item>Slide 2</Carousel.Item>
    <Carousel.Item>Slide 3</Carousel.Item>
  </Carousel.Content>
</Carousel.Root>
```

---

### Plugins

Add Embla plugins via the `plugins` prop.

```svelte
<script lang="ts">
  import Autoplay from "embla-carousel-autoplay";
  import * as Carousel from "$lib/components/ui/carousel/index.js";
</script>

<Carousel.Root
  plugins={[
    Autoplay({
      delay: 2000,
    }),
  ]}
>
  <Carousel.Content>
    <Carousel.Item>Slide 1</Carousel.Item>
    <Carousel.Item>Slide 2</Carousel.Item>
    <Carousel.Item>Slide 3</Carousel.Item>
  </Carousel.Content>
</Carousel.Root>
```

> **Note** – For a full list of available plugins, see the [Embla Carousel plugin docs](https://www.embla-carousel.com/docs/plugins/).

---

## API Reference

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `orientation` | `"horizontal" \| "vertical"` | `"horizontal"` | Sets the carousel’s scroll direction. |
| `opts` | `EmblaOptionsType` | `{}` | Passes options directly to Embla. |
| `plugins` | `EmblaPlugin[]` | `[]` | Adds Embla plugins. |
| `setApi` | `(api: CarouselAPI) => void` | `undefined` | Callback that receives the Embla API instance. |

### Child Components

| Component | Description |
|-----------|-------------|
| `<Carousel.Root>` | Wrapper that initializes the carousel. |
| `<Carousel.Content>` | Container for all slides. |
| `<Carousel.Item>` | Individual slide. |
| `<Carousel.Previous>` | Button that moves to the previous slide. |
| `<Carousel.Next>` | Button that moves to the next slide. |

---

## License

This component is part of the shadcn‑svelte UI kit, licensed under the MIT License.  
See the repository’s `LICENSE` file for details.