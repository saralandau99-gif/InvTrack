# InvTrack

A personal investment tracker built around one idea: know not just what you own, but *why* you bought it, and *what has to happen* for you to buy more, sell, or walk away. Every position carries its own reasoning and its own exit/entry triggers — turning gut-feel decisions into a plan you can actually check yourself against, instead of a static list of holdings and prices.

This repo is both a working project and a product-management portfolio piece — the `/docs` folder holds the product thinking (vision, requirements, data model) that precedes any code.

## Status

MVP1 (personal trading journal, shares, manual entry) is in the design phase: one-pager and PRD are done, ERD is drafted, UI prototype exists. Build has not started.

## Docs

- [`docs/one-pager.md`](docs/one-pager.md) — vision, personas, market research, MVP roadmap (MVP1–4), and the reasoning behind the sequencing
- [`docs/prd-mvp1.md`](docs/prd-mvp1.md) — MVP1 epics and user stories with acceptance criteria
- [`docs/erd.md`](docs/erd.md) — MVP1 data model

## Roadmap

| MVP | Scope |
|---|---|
| MVP1 | Manual share tracking: position + thesis + intent, daily dashboard, timeline, export |
| MVP2 | Live market data, price alerts, historical charts |
| MVP3 | Multi-asset support (cash, real estate, collectibles, crypto, pensions/funds) |
| MVP4 | AI-powered insights (predicted vs. actual, pattern surfacing) |

See the one-pager for the full rationale behind this sequencing.

## Platform

Desktop first, iOS planned after MVP1 is validated in daily use.

## Architecture Decision Records

- [`docs/adr/001-mvp1-tech-stack.md`](docs/adr/001-mvp1-tech-stack.md) — TypeScript + React + Electron + SQLite, no backend for MVP1, React Native planned for iOS

