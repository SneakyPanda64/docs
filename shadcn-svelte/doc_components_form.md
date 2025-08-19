# shadcn‑svelte – Forms Documentation

> **shadcn‑svelte** is a collection of UI components for SvelteKit that are built on top of
> **Formsnap** and **SvelteKit‑Superforms**.  
> This guide shows how to create fully‑accessible, type‑safe forms with client‑ and server‑side
> validation using **Zod**.

---

## Table of Contents

1. [Features](#features)
2. [Anatomy](#anatomy)
3. [Example](#example)
4. [Installation](#installation)
5. [Usage](#usage)
   - [Create a form schema](#create-a-form-schema)
   - [Setup the load function](#setup-the-load-function)
   - [Create the form component](#create-the-form-component)
   - [Use the component](#use-the-component)
   - [Create an action](#create-an-action)
6. [Next Steps](#next-steps)
7. [Further Reading](#further-reading)

---

## Features

- **Composable form components** – build forms with a clear, declarative API.
- **Form field scoping** – each field manages its own state and validation.
- **Zod (or any Superforms‑compatible) validation** – client‑ and server‑side checks.
- **Automatic ARIA attributes** – accessibility is handled for you.
- **Integration with UI primitives** – use `<Input>`, `<Select>`, `<Checkbox>`, etc. inside forms.

---

## Anatomy

```html
<form>
  <Form.Field>
    <Form.Control>
      <Form.Label />
      <!-- Any Form input component -->
    </Form.Control>
    <Form.Description />
    <Form.FieldErrors />
  </Form.Field>
</form>
```

- `<Form.Field>` – wrapper for a single form field.
- `<Form.Control>` – injects the necessary `id`, `name`, and ARIA attributes.
- `<Form.Label>` – automatically linked to the input via `for`.
- `<Form.Description>` – optional helper text.
- `<Form.FieldErrors>` – displays validation errors.

---

## Example

```html
<form method="POST" use:enhance>
  <Form.Field {form} name="email">
    <Form.Control>
      {#snippet children({ props })}
        <Form.Label>Email</Form.Label>
        <Input {...props} bind:value={$formData.email} />
      {/snippet}
    </Form.Control>
    <Form.Description />
    <Form.FieldErrors />
  </Form.Field>
</form>
```

---

## Installation

```bash
# Using pnpm
pnpm dlx shadcn-svelte@latest add form

# Or with npm
npm i shadcn-svelte@latest

# Or with bun
bun i shadcn-svelte@latest

# Or with yarn
yarn add shadcn-svelte@latest
```

---

## Usage

### 1. Create a form schema

Define the shape of your form with **Zod**.  
Create `src/routes/settings/schema.ts`:

```ts
import { z } from "zod";

export const formSchema = z.object({
  username: z.string().min(2).max(50),
});

export type FormSchema = typeof formSchema;
```

### 2. Setup the load function

`src/routes/settings/+page.server.ts`:

```ts
import type { PageServerLoad } from "./$types.js";
import { superValidate } from "sveltekit-superforms";
import { formSchema } from "./schema";
import { zod } from "sveltekit-superforms/adapters";

export const load: PageServerLoad = async () => {
  return {
    form: await superValidate(zod(formSchema)),
  };
};
```

### 3. Create the form component

`src/routes/settings/settings-form.svelte`:

```svelte
<script lang="ts">
  import * as Form from "$lib/components/ui/form/index.js";
  import { Input } from "$lib/components/ui/input/index.js";
  import { formSchema, type FormSchema } from "./schema";
  import {
    type SuperValidated,
    type Infer,
    superForm,
  } from "sveltekit-superforms";
  import { zodClient } from "sveltekit-superforms/adapters";

  let { data }: { data: { form: SuperValidated<Infer<FormSchema>> } } = $props();

  const form = superForm(data.form, {
    validators: zodClient(formSchema),
  });

  const { form: formData, enhance } = form;
</script>

<form method="POST" use:enhance>
  <Form.Field {form} name="username">
    <Form.Control>
      {#snippet children({ props })}
        <Form.Label>Username</Form.Label>
        <Input {...props} bind:value={$formData.username} />
      {/snippet}
    </Form.Control>
    <Form.Description>This is your public display name.</Form.Description>
    <Form.FieldErrors />
  </Form.Field>
  <Form.Button>Submit</Form.Button>
</form>
```

### 4. Use the component

`src/routes/settings/+page.svelte`:

```svelte
<script lang="ts">
  import type { PageData } from "./$types.js";
  import SettingsForm from "./settings-form.svelte";
  let { data }: { data: PageData } = $props();
</script>

<SettingsForm {data} />
```

### 5. Create an action

`src/routes/settings/+page.server.ts` (add to the file from step 2):

```ts
import type { PageServerLoad, Actions } from "./$types.js";
import { fail } from "@sveltejs/kit";
import { superValidate } from "sveltekit-superforms";
import { zod } from "sveltekit-superforms/adapters";
import { formSchema } from "./schema";

export const actions: Actions = {
  default: async (event) => {
    const form = await superValidate(event, zod(formSchema));
    if (!form.valid) {
      return fail(400, { form });
    }
    // Handle successful submission here
    return { form };
  },
};
```

---

## Next Steps

- Explore **Formsnap** and **SvelteKit‑Superforms** documentation for advanced usage.
- Try other form components: `Checkbox`, `DatePicker`, `RadioGroup`, `Select`, `Switch`, `Textarea`, etc.

---

## Further Reading

- [Formsnap Docs](https://formsnap.dev)
- [SvelteKit‑Superforms Docs](https://sveltekit-superforms.dev)
- [Zod Docs](https://zod.dev)

---

*Built by shadcn. Ported to Svelte by Huntabyte & CokaKoala.*