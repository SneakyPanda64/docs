# Badge Component – shadcn‑svelte

The **Badge** component is a small, highly‑customizable UI element that can be used to display status, tags, or counts. It supports several visual variants and can be styled with Tailwind classes.

---

## 📦 Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add badge
```

> **Tip:** The same command works with `npm`, `yarn`, or `bun`.

### Manual

```bash
# Add the component files to your project
# (copy the files from the registry or run the CLI above)
```

---

## 🔧 Usage

```svelte
<script lang="ts">
  import { Badge } from "$lib/components/ui/badge/index.js";
</script>

<Badge variant="outline">Badge</Badge>
```

> **Variants**  
> `default` | `secondary` | `destructive` | `outline`

---

## 🔗 Badge as a Link

You can use the `badgeVariants` helper to style an `<a>` element like a badge.

```svelte
<script lang="ts">
  import { badgeVariants } from "$lib/components/ui/badge/index.js";
</script>

<a href="/dashboard" class={badgeVariants({ variant: "outline" })}>
  Badge
</a>
```

---

## 🎨 Preview

```svelte
<script lang="ts">
  import { Badge } from "$lib/components/ui/badge/index.js";
  import BadgeCheckIcon from "@lucide/svelte/icons/badge-check";
</script>

<div class="flex flex-col items-center gap-2">
  <!-- Basic badges -->
  <div class="flex w-full flex-wrap gap-2">
    <Badge>Badge</Badge>
    <Badge variant="secondary">Secondary</Badge>
    <Badge variant="destructive">Destructive</Badge>
    <Badge variant="outline">Outline</Badge>
  </div>

  <!-- Badges with icons and custom styles -->
  <div class="flex w-full flex-wrap gap-2">
    <Badge variant="secondary" class="bg-blue-500 text-white dark:bg-blue-600">
      <BadgeCheckIcon />
      Verified
    </Badge>

    <Badge class="h-5 min-w-5 rounded-full px-1 font-mono tabular-nums">8</Badge>

    <Badge
      class="h-5 min-w-5 rounded-full px-1 font-mono tabular-nums"
      variant="destructive"
    >
      99
    </Badge>

    <Badge
      class="h-5 min-w-5 rounded-full px-1 font-mono tabular-nums"
      variant="outline"
    >
      20+
    </Badge>
  </div>
</div>
```

---

## 📄 Component Source

```svelte
<script lang="ts" context="module">
  export { default as Badge } from "./badge.svelte";
  export { default as badgeVariants } from "./badge-variants.ts";
</script>
```

---

## 📚 Further Reading

- [Installation Guide](#installation)
- [Theming & Dark Mode](#)
- [Component Registry](#)

---

*Built by shadcn. Ported to Svelte by Huntabyte & CokaKoala.*