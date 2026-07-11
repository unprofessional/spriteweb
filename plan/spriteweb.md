# SPRITEweb — Public Site Plan

> **Status:** Planning
> **Repo:** `unprofessional/spriteweb`
> **Hosting:** GitHub Pages (static)
> **Stack:** Astro (static output) + vanilla CSS

---

## Purpose

A lightweight public-facing site for SPRITEbot. Three jobs:

1. **Explain** — what the bot does, who it's for, what it looks like
2. **Invite** — one-click Discord OAuth2 bot authorization
3. **Subscribe** — pricing tiers, feature breakdown, link to payment flow

This is a static site. No backend, no API calls, no admin dashboard (admin stays in Discord slash commands for now).

---

## Pages

### `/` — Landing Page

Hero section with:

- Bot name + tagline (e.g. "Tabletop RPG campaigns, right inside Discord")
- Brief value prop (3-4 bullet points or short blocks):
  - **Campaign management** — create games, define custom stat templates, publish for players to join
  - **Character sheets** — interactive creation flow, custom fields, public/private visibility
  - **In-character roleplay** — webhook-powered message proxying with `/ic` and `/ooc`, supports long posts and editing
  - **Inventory tracking** — per-character item management with custom fields
  - **Dice rolling** — cryptographic dice roller, `1d2` through `15d999`
  - **Thread bumping** — auto-bump registered threads on a schedule
- **Add to Discord** button (prominent CTA)
- Link to pricing section

### `/pricing` — Subscription Info

Feature comparison table:

| Feature                        | Free | Premium |
| ------------------------------ | ---- | ------- |
| View/browse games & characters | ✅   | ✅      |
| Join games, switch context     | ✅   | ✅      |
| Dice rolling                   | ✅   | ✅      |
| Create characters & stats      | ❌   | ✅      |
| Inventory management           | ❌   | ✅      |
| Game/campaign admin            | ❌   | ✅      |
| Auto thread bumping            | ❌   | ✅      |

Pricing cards:

| Plan      | Price         | Effective Monthly |
| --------- | ------------- | ----------------- |
| Monthly   | $3.00/mo      | $3.00             |
| Quarterly | $7.99/quarter | ~$2.66            |
| Annual    | $27.99/year   | ~$2.33            |

- All plans are **per server**, not per user
- One subscription unlocks premium for the entire server
- Note: payment flow will link to Stripe Checkout (once Stripe integration ships in SPRITEbot). Until then, show tiers as "coming soon" or link to the Discord server for early access.

### `/guide` — Quick Start Guide

Adapted from `INSTRUCTIONS.md` in the bot repo. Player-facing, not developer-facing:

- How to join a game
- How to create a character
- How to use `/ic` and `/ooc`
- How to edit/delete proxied messages
- How inventory works
- How dice rolling works

Keep it scannable — short sections, maybe collapsible details for longer topics.

### `/legal` — Terms & Privacy (stub)

Placeholder page. Will need real content before Stripe goes live:

- What data the bot stores (guild IDs, user IDs, character data)
- Data retention (30-day soft-delete window, then permanent)
- No selling of data
- Contact info

---

## Design Direction

- Clean, dark theme (matches Discord aesthetic)
- Minimal — no frameworks beyond Astro, no component libraries
- Mobile-responsive
- Fast — static HTML, minimal JS, no client-side hydration needed
- Bot avatar/branding in header

---

## Tech Stack

### Why Astro

- Static site generator with zero JS by default
- Markdown support for the guide page (can import from `.md` files)
- Built-in GitHub Pages deployment action
- No React/Vue/Svelte dependency needed for a content site
- If we ever want interactive components later, Astro supports islands

### Build & Deploy

- `astro build` → static output to `dist/`
- GitHub Actions workflow: on push to `master`, build and deploy to GitHub Pages
- Custom domain: optional (can use `unprofessional.github.io/spriteweb` or a custom domain later)

### Project Structure

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
│   └── styles/
│       └── global.css          # site-wide styles
├── public/
│   └── assets/                 # bot avatar, og images, favicon
├── plan/
│   └── spriteweb.md            # this file
├── astro.config.mjs
├── package.json
└── README.md
```

---

## Implementation Passes

### Pass 1: Scaffold + Landing Page

- Initialize Astro project (static output mode)
- Base layout with nav, footer, dark theme
- Landing page with feature blocks and invite button
- GitHub Actions deploy workflow
- Confirm it builds and deploys to GitHub Pages

### Pass 2: Pricing Page

- Feature comparison table (free vs premium)
- Pricing cards with tier breakdown
- CTA button (links to Discord server or Stripe Checkout when ready)

### Pass 3: Quick Start Guide

- Adapt `INSTRUCTIONS.md` into web-friendly format
- Section anchors for deep linking
- Collapsible sections for longer topics if needed

### Pass 4: Legal Stub + Polish

- Terms & privacy placeholder page
- Meta tags (Open Graph, Twitter cards) for link previews
- Favicon + bot avatar in header
- Final responsive/mobile pass

---

## Bot Invite URL

Discord OAuth2 bot authorization URL format:

```
https://discord.com/oauth2/authorize?client_id=BOT_CLIENT_ID&permissions=PERMISSIONS_INT&scope=bot%20applications.commands
```

- `BOT_CLIENT_ID`: SPRITEbot's Discord application ID
- `permissions`: calculated from required permissions (manage webhooks, send messages, manage messages, etc.)
- `scope`: `bot` + `applications.commands`

The invite button on the landing page will use this URL directly. No backend needed.

---

## Out of Scope (For Now)

- **Admin dashboard** — stays in Discord slash commands (`/admin orphans`, `/admin games`, etc.)
- **Stripe Checkout integration** — the site shows pricing info but actual payment flows are handled by SPRITEbot + Stripe (separate project)
- **User authentication** — no login, no Discord OAuth2 for users, no session management
- **Blog / changelog** — can add later if needed
- **API** — this is a static site, no server-side logic

---

## Open Questions

- [ ] Custom domain? (e.g. `spritebot.gg`, `sprite.gg`, or just use `unprofessional.github.io/spriteweb`)
- [ ] Bot client ID + permissions integer for the invite URL (mads to provide)
- [ ] Any specific branding/color palette beyond "dark theme, Discord-adjacent"?
- [ ] Screenshots or GIFs of the bot in action for the landing page?
