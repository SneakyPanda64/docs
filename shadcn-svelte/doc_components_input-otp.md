# shadcn‑svelte – Input OTP Component

> Accessible one‑time‑password component with copy‑paste functionality.  
> Built on top of **Bits UI**’s `PinInput` and inspired by @guilherme_rodz’s original component.

---

## 📦 Installation

### CLI

```bash
pnpm dlx shadcn-svelte@latest add input-otp
```

> The same command works with `npm`, `yarn`, or `bun`.

### Manual

```bash
# Add the component to your project
npm install shadcn-svelte
```

Then import the component in your Svelte file:

```svelte
<script lang="ts">
  import * as InputOTP from "$lib/components/ui/input-otp/index.js";
</script>
```

---

## 🔧 Usage

```svelte
<script lang="ts">
  import * as InputOTP from "$lib/components/ui/input-otp/index.js";
</script>

<InputOTP.Root maxlength={6}>
  {#snippet children({ cells })}
    <InputOTP.Group>
      {#each cells.slice(0, 3) as cell}
        <InputOTP.Slot {cell} />
      {/each}
    </InputOTP.Group>

    <InputOTP.Separator />

    <InputOTP.Group>
      {#each cells.slice(3, 6) as cell}
        <InputOTP.Slot {cell} />
      {/each}
    </InputOTP.Group>
  {/snippet}
</InputOTP.Root>
```

> *`maxlength`* – Maximum number of characters the OTP can contain.  
> *`pattern`* – Optional regular expression to validate each cell.

---

## 📚 API Reference

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `maxlength` | `number` | `6` | Maximum length of the OTP. |
| `pattern` | `RegExp` | `undefined` | Regex to validate each cell. |
| `value` | `string` | `""` | Current OTP value. |
| `on:input` | `Event` | – | Fires when the OTP changes. |
| `on:blur` | `Event` | – | Fires when the component loses focus. |

### Slots

| Slot | Description |
|------|-------------|
| `Root` | The outermost wrapper. |
| `Group` | Wraps a group of `Slot` cells. |
| `Slot` | Individual OTP cell. |
| `Separator` | Optional visual separator between groups. |

---

## 📦 Examples

### 1. Custom Pattern

```svelte
<script lang="ts">
  import * as InputOTP from "$lib/components/ui/input-otp/index.js";
  import { REGEXP_ONLY_DIGITS_AND_CHARS } from "bits-ui";
</script>

<InputOTP.Root maxlength={6} pattern={REGEXP_ONLY_DIGITS_AND_CHARS}>
  <!-- ... -->
</InputOTP.Root>
```

> `REGEXP_ONLY_DIGITS_AND_CHARS` allows only alphanumeric characters.

---

### 2. Separator Between Groups

```svelte
<script lang="ts">
  import * as InputOTP from "$lib/components/ui/input-otp/index.js";
</script>

<InputOTP.Root maxlength={4}>
  {#snippet children({ cells })}
    <InputOTP.Group>
      {#each cells.slice(0, 2) as cell}
        <InputOTP.Slot {cell} />
      {/each}
    </InputOTP.Group>

    <InputOTP.Separator />

    <InputOTP.Group>
      {#each cells.slice(2, 4) as cell}
        <InputOTP.Slot {cell} />
      {/each}
    </InputOTP.Group>
  {/snippet}
</InputOTP.Root>
```

---

### 3. Invalid State

```svelte
<script lang="ts">
  import * as InputOTP from "$lib/components/ui/input-otp/index.js";
</script>

<InputOTP.Root maxlength={6} class="invalid">
  {#snippet children({ cells })}
    <InputOTP.Group>
      {#each cells as cell}
        <InputOTP.Slot {cell} />
      {/each}
    </InputOTP.Group>
  {/snippet}
</InputOTP.Root>
```

> Add a CSS class (e.g., `invalid`) to style the component when the OTP is incorrect.

---

### 4. Form Integration

```svelte
<script lang="ts">
  import * as InputOTP from "$lib/components/ui/input-otp/index.js";
  let otp = "";
</script>

<form on:submit|preventDefault={() => console.log(otp)}>
  <label for="otp">Please enter the one‑time password sent to your phone.</label>

  <InputOTP.Root id="otp" bind:value={otp} maxlength={6}>
    {#snippet children({ cells })}
      <InputOTP.Group>
        {#each cells as cell}
          <InputOTP.Slot {cell} />
        {/each}
      </InputOTP.Group>
    {/snippet}
  </InputOTP.Root>

  <button type="submit">Submit</button>
</form>
```

---

## 🎨 Theming & Dark Mode

The component respects Tailwind CSS utilities.  
Add `dark:` variants or custom classes to style it for dark mode.

```html
<InputOTP.Root class="dark:bg-gray-800 dark:text-white" ...>
```

---

## 📦 Component Source

The source code is available in the repository under:

```
src/lib/components/ui/input-otp/
```

Feel free to inspect or contribute.

---

## 📖 Further Reading

- [Bits UI – PinInput](https://github.com/bitsofco/developer/tree/main/bits-ui)  
- [Original Input OTP by @guilherme_rodz](https://github.com/guilherme-rodz/input-otp)

---

Happy coding! 🚀