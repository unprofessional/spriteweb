# SPRITEweb

Public-facing website for [SPRITEbot](https://github.com/unprofessional/spritebot) — a Discord bot for running lightweight tabletop RPG campaigns.

## What This Is

A static site (Astro, GitHub Pages) that serves as:

- **Landing page** — what SPRITEbot does and why you'd want it
- **Invite link** — one-click bot authorization for Discord servers
- **Pricing info** — subscription tiers and feature breakdown
- **Quick start guide** — how to use the bot as a player or GM

## Development

```bash
npm install
npm run dev      # local dev server
npm run build    # static build to dist/
```

## Deployment

Pushes to `master` auto-deploy to GitHub Pages via GitHub Actions.

## Plan

See [`plan/spriteweb.md`](plan/spriteweb.md) for the full implementation plan.
