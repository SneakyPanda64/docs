# shadcn‑svelte – Tailwind Color Palette

The **shadcn‑svelte** component library ships with the full Tailwind CSS color palette.  
All colors are available in HEX, RGB, HSL, CSS‑variable, and Tailwind‑class form, so you can copy‑paste them directly into your Svelte project.

> **Built by shadcn** – ported to Svelte by Huntabyte & CokaKoala.

---

## Color Groups

Below is a quick reference of every color group and its available shades.  
(Each shade is available as a Tailwind class, a CSS variable, and in HEX/RGB/HSL.)

| Group | Shades |
|-------|--------|
| **Neutral** | 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 |
| **Stone** | 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 |
| **Zinc** | 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 |
| **Slate** | 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 |
| **Gray** | 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 |
| **Red** | 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 |
| **Orange** | 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 |
| **Amber** | 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 |
| **Yellow** | 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 |
| **Lime** | 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 |
| **Green** | 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 |
| **Emerald** | 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 |
| **Teal** | 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 |
| **Cyan** | 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 |
| **Sky** | 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 |
| **Blue** | 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 |
| **Indigo** | 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 |
| **Violet** | 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 |
| **Purple** | 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 |
| **Fuchsia** | 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 |
| **Pink** | 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 |
| **Rose** | 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 |

> **Note**: Each shade can be referenced in Tailwind as `bg-{color}-{shade}` (e.g., `bg-blue-500`) or as a CSS variable `--color-{color}-{shade}` (e.g., `--color-blue-500`).

---

## Quick Usage Example

```svelte
<script>
  // No special imports needed – Tailwind is already configured
</script>

<div class="bg-blue-500 text-white p-4 rounded">
  Hello, world!
</div>
```

The above snippet renders a blue background (`blue-500`) with white text.  
You can replace `blue-500` with any other shade from the palette (e.g., `red-700`, `emerald-200`, etc.).

---

## Adding the Palette to Your Project

If you’re using Tailwind with shadcn‑svelte, the palette is already included.  
If you need to extend or customize it, edit your `tailwind.config.cjs`:

```js
module.exports = {
  theme: {
    extend: {
      colors: {
        // Add custom colors here
      }
    }
  }
}
```

---

### Summary

- **All Tailwind colors** are available out‑of‑the‑box.
- Use the **class** (`bg-{color}-{shade}`) or **CSS variable** (`--color-{color}-{shade}`) forms.
- The palette is fully documented in the shadcn‑svelte docs and can be copied directly into your Svelte components.

Happy styling!