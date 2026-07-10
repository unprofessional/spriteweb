# Codex Conventions — SPRITEweb

Read this before writing code. It describes the architecture, required
patterns, and conventions for this project.

For implementation plans, see `plan/`.

---

## Architecture

### Stack

- **Framework:** Astro (static output mode, no SSR)
- **Styling:** Vanilla CSS (no Tailwind, no component libraries)
- **Hosting:** GitHub Pages
- **CI/CD:** GitHub Actions (build on push to `main`, deploy to Pages)

### Design Principles

- **Zero client JS by default.** Astro ships no JavaScript unless a
  component explicitly opts in with `client:*` directives. Keep it that way.
- **Static only.** No API calls, no server-side logic, no auth. This is a
  content site.
- **Mobile-first responsive.** Dark theme, Discord-adjacent aesthetic.
- **Fast.** Every page should be static HTML + CSS. No hydration, no
  framework runtime.

### Directory Structure

```
spriteweb/
├── src/
│   ├── layouts/
│   │   └── Base.astro          # shared HTML shell, nav, footer
│   ├── pages/
│   │   ├── index.astro         # landing page
│   │   ├── pricing.astro       # subscription tiers
│   │   ├── guide.astro         # quick start guide
│   │   └── legal.astro         # terms & privacy stub
│   ├── components/             # reusable Astro components
│   └── styles/
│       └── global.css          # site-wide styles
├── public/
│   └── assets/                 # bot avatar, og images, favicon
├── plan/                       # implementation plans
├── astro.config.mjs
├── package.json
└── README.md
```

---

## Pages

| Route       | Purpose                                      |
| ----------- | -------------------------------------------- |
| `/`         | Landing page — features, invite button       |
| `/pricing`  | Subscription tiers, free vs premium table    |
| `/guide`    | Quick start guide for players and GMs        |
| `/legal`    | Terms & privacy stub                         |

---

## Code Style

### Prettier

```json
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "all",
  "printWidth": 100
}
```

**Always run `npx prettier --write .` before committing.**

### Git

- Branch from `develop`: `feature/<short-name>`
- Commits: imperative present tense ("Add pricing page with tier cards")
- Deploy branch: `main` (triggers GitHub Actions deploy)
- Push to `develop` for review before merging to `main`.

### Astro Components

- Use `.astro` files for all components and pages.
- Prefer Astro components over framework components (React, Vue, etc.)
  unless there is a specific interactivity requirement.
- Keep components simple — props in the frontmatter, HTML in the template.

---

## Content Guidelines

- **Tone:** Clear, friendly, slightly playful. Match the bot's personality
  without being cringe.
- **Audience:** Discord server admins and TTRPG players evaluating the bot.
  Not developers.
- **Length:** Concise. Feature descriptions should be 1-2 sentences max.
  The guide page can be longer but should use scannable sections.

---

## Assets

- Bot avatar and branding assets go in `public/assets/`.
- Use descriptive filenames (`sprite-avatar.png`, not `img1.png`).
- Optimize images before committing (reasonable file sizes for web).

---

## Deployment

### GitHub Pages

Astro has a built-in GitHub Pages integration:

```js
// astro.config.mjs
import { defineConfig } from 'astro/config';

export default defineConfig({
  site: 'https://unprofessional.github.io',
  base: '/spriteweb',
  output: 'static',
});
```

### GitHub Actions

Deploy workflow triggers on push to `main`:

1. Checkout → Install → Build → Deploy to Pages

Use the official `withastro/action` or `actions/deploy-pages` pattern.

---

## Out of Scope

- **Admin dashboard** — admin commands stay in Discord (`/admin`)
- **User authentication** — no login, no OAuth2
- **API / backend** — this is a static site
- **Stripe Checkout** — pricing page shows info only; payment flows are
  handled by SPRITEbot
