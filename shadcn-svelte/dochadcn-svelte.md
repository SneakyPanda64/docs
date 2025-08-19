# shadcn‑svelte

> **shadcn‑svelte** is an unofficial, community‑led Svelte port of the popular
> [shadcn/ui](https://github.com/shadcn/ui) component library.  
> We are not affiliated with the original project, but we received the author’s
> blessing before creating this repository.

---

## 📦 Features

| Feature | Description |
|---------|-------------|
| **Svelte‑friendly** | All components are written in Svelte 5 and use the latest SvelteKit APIs. |
| **Tailwind‑powered** | Styling is done with Tailwind CSS, so you can customize every component with utility classes. |
| **Accessibility** | Every component follows WAI‑ARIA best practices out of the box. |
| **Zero‑config** | No build‑time configuration required – just copy the component files into your project. |
| **Open source** | MIT‑licensed, free to use and modify. |

---

## 🚀 Quick Start

```bash
# Install the package
pnpm add shadcn-svelte

# Or, if you prefer npm
npm i shadcn-svelte
```

> **Tip** – The library ships with a set of pre‑built components that you can import directly:

```svelte
<script>
  import { Button } from 'shadcn-svelte';
</script>

<Button variant="primary">
  Click me
</Button>
```

> For a full list of available components, see the [official documentation](https://shadcn-svelte.com/docs).

---

## 📚 Documentation

The complete API reference, component usage examples, and theming guide can be found on the website:

- **Website** – <https://shadcn-svelte.com/docs>
- **Component Gallery** – <https://shadcn-svelte.com/docs/components>

> All examples on the site are written in Svelte and use Tailwind classes.  
> Feel free to copy‑paste them into your own projects.

---

## 📁 Repository Structure

```
shadcn-svelte/
├─ .github/          # GitHub Actions & issue templates
├─ docs/             # Static site source (generated with SvelteKit)
├─ packages/         # Individual component packages
├─ registry-template/ # Boilerplate for creating new component packages
├─ src/              # Core library source (if any)
├─ tests/            # Unit & integration tests
├─ .eslintrc.js
├─ .prettierrc
├─ package.json
└─ README.md
```

---

## 🤝 Contributing

We welcome contributions! Please read our [Contributing Guide](CONTRIBUTING.md) before submitting a pull request.

---

## 📜 License

MIT – see the [LICENSE](LICENSE.md) file for details.

---

## 🎉 Sponsors

This project is supported by the following wonderful people and organizations:

| Sponsor | Link |
|---------|------|
| @huntabyte | <https://ko-fi.com/huntabyte> |
| @CokaKoala | <https://ko-fi.com/CokaKoala> |
| AdrianGonz97 | <https://ko-fi.com/AdrianGonz97> |

---

## 📞 Community

- **Discord** – Join our server to ask questions, share ideas, or just say hi!  
  <https://discord.gg/shadcn-svelte>

---

## 📌 Notes

- We are not affiliated with the original shadcn/ui project, but we have received the author’s blessing to create this community‑led port.
- The library is actively maintained and updated to keep pace with SvelteKit 2 and Tailwind CSS 4.

---

Happy coding! 🎉