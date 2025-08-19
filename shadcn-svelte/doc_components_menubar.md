# Menubar – shadcn‑svelte

A fully‑featured, accessible menubar component for SvelteKit, Vite, Astro, or any Svelte project.  
It follows the shadcn design system and is fully Tailwind‑styled.

> **Tip** – The component is available as a part of the shadcn‑svelte registry.  
> Install it with the CLI or manually, then import it from `$lib/components/ui/menubar`.

---

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [API Reference](#api-reference)
  - [Root](#root)
  - [Menu](#menu)
  - [Trigger](#trigger)
  - [Content](#content)
  - [Item](#item)
  - [Shortcut](#shortcut)
  - [Separator](#separator)
  - [Sub](#sub)
  - [SubTrigger](#subtrigger)
  - [SubContent](#subcontent)
  - [CheckboxItem](#checkboxitem)
  - [RadioGroup](#radiogroup)
  - [RadioItem](#radioitem)
- [Examples](#examples)
  - [Full Example](#full-example)
  - [Simplified Example](#simplified-example)
- [Theming & Dark Mode](#theming--dark-mode)
- [Contributing](#contributing)

---

## Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add menubar
```

> Replace `pnpm` with `npm`, `yarn`, or `bun` if you prefer.

### Manual

1. Add the component files to your project (e.g. `src/lib/components/ui/menubar`).
2. Import the component in your Svelte file:

```svelte
<script lang="ts">
  import * as Menubar from "$lib/components/ui/menubar/index.js";
</script>
```

---

## Usage

```svelte
<script lang="ts">
  import * as Menubar from "$lib/components/ui/menubar/index.js";
</script>

<Menubar.Root>
  <Menubar.Menu>
    <Menubar.Trigger>File</Menubar.Trigger>
    <Menubar.Content>
      <Menubar.Item>
        New Tab <Menubar.Shortcut>⌘T</Menubar.Shortcut>
      </Menubar.Item>
      <Menubar.Item>New Window</Menubar.Item>
      <Menubar.Separator />
      <Menubar.Item>Share</Menubar.Item>
      <Menubar.Separator />
      <Menubar.Item>Print</Menubar.Item>
    </Menubar.Content>
  </Menubar.Menu>
</Menubar.Root>
```

---

## API Reference

### `Menubar.Root`

The top‑level wrapper. It manages the overall state of the menubar.

```svelte
<Menubar.Root>
  <!-- Menubar.Menu components -->
</Menubar.Root>
```

### `Menubar.Menu`

Defines a single menu (e.g., *File*, *Edit*). It contains a trigger and a content panel.

```svelte
<Menubar.Menu>
  <Menubar.Trigger>File</Menubar.Trigger>
  <Menubar.Content>
    <!-- Menubar.Item, Menubar.Sub, etc. -->
  </Menubar.Content>
</Menubar.Menu>
```

### `Menubar.Trigger`

The button that opens the menu. It can contain any markup.

```svelte
<Menubar.Trigger>File</Menubar.Trigger>
```

### `Menubar.Content`

The panel that appears when the menu is open. It should contain `Menubar.Item`, `Menubar.Sub`, etc.

```svelte
<Menubar.Content>
  <!-- Items -->
</Menubar.Content>
```

### `Menubar.Item`

A selectable menu entry. Supports `inset` prop to add left padding.

```svelte
<Menubar.Item>New Tab</Menubar.Item>
<Menubar.Item inset>Reload</Menubar.Item>
```

### `Menubar.Shortcut`

Displays a keyboard shortcut next to an item.

```svelte
<Menubar.Item>
  New Tab <Menubar.Shortcut>⌘T</Menubar.Shortcut>
</Menubar.Item>
```

### `Menubar.Separator`

A horizontal divider between groups of items.

```svelte
<Menubar.Separator />
```

### `Menubar.Sub`

Creates a nested submenu. Must contain a `SubTrigger` and `SubContent`.

```svelte
<Menubar.Sub>
  <Menubar.SubTrigger>Share</Menubar.SubTrigger>
  <Menubar.SubContent>
    <Menubar.Item>Email link</Menubar.Item>
    <Menubar.Item>Messages</Menubar.Item>
  </Menubar.SubContent>
</Menubar.Sub>
```

### `Menubar.SubTrigger`

The trigger for a submenu.

```svelte
<Menubar.SubTrigger>Share</Menubar.SubTrigger>
```

### `Menubar.SubContent`

The panel for a submenu.

```svelte
<Menubar.SubContent>
  <!-- Sub items -->
</Menubar.SubContent>
```

### `Menubar.CheckboxItem`

A menu item that can be toggled. Bind `checked` to a Svelte store or reactive variable.

```svelte
<script lang="ts">
  let bookmarks = $state(false);
</script>

<Menubar.CheckboxItem bind:checked={bookmarks}>
  Always Show Bookmarks Bar
</Menubar.CheckboxItem>
```

### `Menubar.RadioGroup`

Wraps a set of `RadioItem`s. Bind `value` to a variable.

```svelte
<script lang="ts">
  let profileRadioValue = $state("benoit");
</script>

<Menubar.RadioGroup bind:value={profileRadioValue}>
  <Menubar.RadioItem value="andy">Andy</Menubar.RadioItem>
  <Menubar.RadioItem value="benoit">Benoit</Menubar.RadioItem>
  <Menubar.RadioItem value="Luis">Luis</Menubar.RadioItem>
</Menubar.RadioGroup>
```

### `Menubar.RadioItem`

A selectable radio option inside a `RadioGroup`.

```svelte
<Menubar.RadioItem value="andy">Andy</Menubar.RadioItem>
```

---

## Examples

### Full Example

```svelte
<script lang="ts">
  import * as Menubar from "$lib/components/ui/menubar/index.js";

  let bookmarks = $state(false);
  let fullUrls = $state(true);
  let profileRadioValue = $state("benoit");
</script>

<Menubar.Root>
  <!-- File -->
  <Menubar.Menu>
    <Menubar.Trigger>File</Menubar.Trigger>
    <Menubar.Content>
      <Menubar.Item>
        New Tab <Menubar.Shortcut>⌘T</Menubar.Shortcut>
      </Menubar.Item>
      <Menubar.Item>
        New Window <Menubar.Shortcut>⌘N</Menubar.Shortcut>
      </Menubar.Item>
      <Menubar.Item>New Incognito Window</Menubar.Item>
      <Menubar.Separator />
      <Menubar.Sub>
        <Menubar.SubTrigger>Share</Menubar.SubTrigger>
        <Menubar.SubContent>
          <Menubar.Item>Email link</Menubar.Item>
          <Menubar.Item>Messages</Menubar.Item>
          <Menubar.Item>Notes</Menubar.Item>
        </Menubar.SubContent>
      </Menubar.Sub>
      <Menubar.Separator />
      <Menubar.Item>
        Print... <Menubar.Shortcut>⌘P</Menubar.Shortcut>
      </Menubar.Item>
    </Menubar.Content>
  </Menubar.Menu>

  <!-- Edit -->
  <Menubar.Menu>
    <Menubar.Trigger>Edit</Menubar.Trigger>
    <Menubar.Content>
      <Menubar.Item>
        Undo <Menubar.Shortcut>⌘Z</Menubar.Shortcut>
      </Menubar.Item>
      <Menubar.Item>
        Redo <Menubar.Shortcut>⇧⌘Z</Menubar.Shortcut>
      </Menubar.Item>
      <Menubar.Separator />
      <Menubar.Sub>
        <Menubar.SubTrigger>Find</Menubar.SubTrigger>
        <Menubar.SubContent>
          <Menubar.Item>Search the web</Menubar.Item>
          <Menubar.Separator />
          <Menubar.Item>Find...</Menubar.Item>
          <Menubar.Item>Find Next</Menubar.Item>
          <Menubar.Item>Find Previous</Menubar.Item>
        </Menubar.SubContent>
      </Menubar.Sub>
      <Menubar.Separator />
      <Menubar.Item>Cut</Menubar.Item>
      <Menubar.Item>Copy</Menubar.Item>
      <Menubar.Item>Paste</Menubar.Item>
    </Menubar.Content>
  </Menubar.Menu>

  <!-- View -->
  <Menubar.Menu>
    <Menubar.Trigger>View</Menubar.Trigger>
    <Menubar.Content>
      <Menubar.CheckboxItem bind:checked={bookmarks}>
        Always Show Bookmarks Bar
      </Menubar.CheckboxItem>
      <Menubar.CheckboxItem bind:checked={fullUrls}>
        Always Show Full URLs
      </Menubar.CheckboxItem>
      <Menubar.Separator />
      <Menubar.Item inset>
        Reload <Menubar.Shortcut>⌘R</Menubar.Shortcut>
      </Menubar.Item>
      <Menubar.Item inset>
        Force Reload <Menubar.Shortcut>⇧⌘R</Menubar.Shortcut>
      </Menubar.Item>
      <Menubar.Separator />
      <Menubar.Item inset>Toggle Fullscreen</Menubar.Item>
      <Menubar.Separator />
      <Menubar.Item inset>Hide Sidebar</Menubar.Item>
    </Menubar.Content>
  </Menubar.Menu>

  <!-- Profiles -->
  <Menubar.Menu>
    <Menubar.Trigger>Profiles</Menubar.Trigger>
    <Menubar.Content>
      <Menubar.RadioGroup bind:value={profileRadioValue}>
        <Menubar.RadioItem value="andy">Andy</Menubar.RadioItem>
        <Menubar.RadioItem value="benoit">Benoit</Menubar.RadioItem>
        <Menubar.RadioItem value="Luis">Luis</Menubar.RadioItem>
      </Menubar.RadioGroup>
      <Menubar.Separator />
      <Menubar.Item inset>Edit...</Menubar.Item>
      <Menubar.Separator />
      <Menubar.Item inset>Add Profile...</Menubar.Item>
    </Menubar.Content>
  </Menubar.Menu>
</Menubar.Root>
```

### Simplified Example

```svelte
<script lang="ts">
  import * as Menubar from "$lib/components/ui/menubar/index.js";
</script>

<Menubar.Root>
  <Menubar.Menu>
    <Menubar.Trigger>File</Menubar.Trigger>
    <Menubar.Content>
      <Menubar.Item>
        New Tab <Menubar.Shortcut>⌘T</Menubar.Shortcut>
      </Menubar.Item>
      <Menubar.Item>New Window</Menubar.Item>
      <Menubar.Separator />
      <Menubar.Item>Share</Menubar.Item>
      <Menubar.Separator />
      <Menubar.Item>Print</Menubar.Item>
    </Menubar.Content>
  </Menubar.Menu>
</Menubar.Root>
```

---

## Theming & Dark Mode

The component uses Tailwind CSS classes that automatically adapt to your global theme.  
If you enable dark mode in Tailwind (`dark:` variants), the menubar will switch styles accordingly.

```css
/* tailwind.config.js */
module.exports = {
  darkMode: 'class', // or 'media'
  // ...
}
```

Add `class="dark"` to a parent element or toggle via JavaScript to switch themes.

---

## Contributing

Feel free to open issues or pull requests on the shadcn‑svelte repository.  
All contributions that improve accessibility, documentation, or add new features are welcome.

---