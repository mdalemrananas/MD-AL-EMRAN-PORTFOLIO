## MD AL EMRAN — Portfolio (Vite + React + TypeScript)

Modern, fast, and accessible personal portfolio built with Vite, React, TypeScript, Tailwind CSS, and shadcn/ui.

### Features
- Clean, responsive UI with focus on readability and contrast
- Sections: Hero, About, Skills, Projects, Research, Education, Contact
- Certifications flip-cards with back-side details
- Smooth animations and keyboard-friendly interactions
- Ready for GitHub Pages deployment

### Tech Stack
- React 18, TypeScript, Vite 5
- Tailwind CSS, shadcn/ui (Radix primitives)
- Recharts for data visualizations

---

## Local Development

Prerequisites:
- Node.js 18+ (recommended LTS 20+) and npm

Install and run:

```bash
npm ci
npm run dev
```

The app will start at `http://127.0.0.1:5173` (see `vite.config.ts`).

Build and preview production:

```bash
npm run build
npm run preview
```

---

## Fresh Upload to GitHub (Solo Contributor)

Follow these steps in Git Bash to ensure a clean history where only your account appears as contributor.

1) Remove any existing Git history:
```bash
rm -rf .git
```

2) Initialize a new repository and set your identity:
```bash
git init
git config user.name "YOUR_NAME"
git config user.email "YOUR_EMAIL"
```

3) Ensure dependencies/outputs are ignored (see `.gitignore`). Then commit:
```bash
git add .
git commit -m "Initial commit: personal portfolio"
```

4) Create a new empty GitHub repository (no README/license). Then add remote and push:
```bash
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main
```

To verify authorship locally:
```bash
git log --format='%an <%ae>'
```

---

## Deploy to GitHub Pages (via Actions)

This repo is configured for GitHub Pages paths using `vite.config.ts` with:
- `base: '/Portfolio-Md-Al-Emran/'` for production

If your repository name differs, either:
- Update `base` to `'/YOUR_REPO_NAME/'` in `vite.config.ts`, or
- Set environment variable `VITE_BASE=/YOUR_REPO_NAME/` in the workflow

Steps:
1) In your GitHub repo → Settings → Pages → Build and deployment: set Source to "GitHub Actions".
2) Create `.github/workflows/deploy.yml` with the following content:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: ["main"]

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'

      - name: Install
        run: npm ci

      - name: Build
        env:
          # Set this to your repo name if different from vite.config.ts
          # VITE_BASE: /YOUR_REPO_NAME/
          NODE_OPTIONS: --max_old_space_size=4096
        run: npm run build

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./dist

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

3) Commit and push the workflow:
```bash
mkdir -p .github/workflows
printf "%s" "<paste the YAML above>" > .github/workflows/deploy.yml
git add .github/workflows/deploy.yml
git commit -m "chore: add GitHub Pages deployment workflow"
git push
```

GitHub Pages will build and publish automatically. Check the deployment URL on the repo’s "Actions" or "Pages" section.

---

## Project Structure

```text
src/
  components/        # UI sections and shared components
  hooks/             # Custom hooks
  lib/               # Utilities
  pages/             # Page-level components
  assets/            # Images / documents (resume, profile image)
```

Key config:
- `vite.config.ts`: dev/preview ports and production base path
- `tailwind.config.ts`: design tokens and plugins

---

## License
This project is licensed under the terms in `LICENSE`.

---

## Support
If something doesn’t run locally:
- Ensure Node.js ≥ 18 is installed and available in PATH
- Run `npm ci` from the project root
- Try `npm run build` and then `npm run preview`
- Share any error logs so they can be addressed quickly
