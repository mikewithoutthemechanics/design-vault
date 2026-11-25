# Design Vault

![Repo size](https://img.shields.io/github/repo-size/mikewithoutthemechanics/design-vault)
![License](https://img.shields.io/github/license/mikewithoutthemechanics/design-vault)
![Top language](https://img.shields.io/github/languages/top/mikewithoutthemechanics/design-vault)

## 👋 Hi, I'm Michael

This is one of my projects. Here's what it's about, why it exists, and how you can use it.

## 📦 What it does

Open-source React components and interactive templates from ThreeUI Community.

## 🎯 The goal it fills

This project exists to solve a specific need and make a particular task simpler. Whether it's automating a workflow, serving a website, or powering a tool, the goal is to be practical, usable, and straightforward.

## 🚀 How to use

1. **Get the code**
   ```bash
   git clone https://github.com/mikewithoutthemechanics/design-vault.git
   cd design-vault
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Run it**
   See the available commands below and pick the one you need.

### Available commands

| `npm run dev` | vite |
| `npm run sync:community` | node scripts/sync-community-from-main.mjs |
| `npm run generate:lib` | node scripts/generate-library-entry.mjs |
| `npm run typecheck` | tsc --noEmit |
| `npm run test` | node --test scripts/community-sanitizers.test.mjs scripts/public-boundary.test.mjs scripts/library-package.test.mjs scripts/prepare-community-release.test.mjs packages/cli/test/*.test.mjs |
| `npm run audit:public` | node scripts/audit-public.mjs |
| `npm run build:site` | npm run typecheck && npm run audit:public && vite build && node scripts/audit-build.mjs |
| `npm run build:lib` | npm run generate:lib && vite build --config vite.lib.config.js && tsc -p tsconfig.lib.json && node scripts/copy-library-assets.mjs |
| `npm run build` | npm run test && npm run build:site && npm run build:lib |
| `npm run prepack` | npm run test && npm run audit:public && npm run build:lib |
| `npm run smoke:package` | node scripts/package-install-smoke.mjs |
| `npm run preview` | vite preview |

## 🛠️ Tech stack

react, vite, typescript

## 💡 Why it matters

This project is built to be useful and easy to pick up. If you're looking for a tool that does design vault, this is a clean, maintainable starting point you can adapt to your own needs.

## 📄 License

MIT

---

Made with ❤️ by Michael
