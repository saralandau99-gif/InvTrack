# ADR-001: MVP1 Tech Stack

**Status:** Accepted
**Date:** 2026-07-15
**Context owner:** Solo developer, product-management background, familiar with reading code but limited hands-on build experience. This ADR doubles as a portfolio artifact — the record should be legible to a non-engineer reviewer.

## Context

MVP1 (per the PRD) is a single-user, local-only, manually-entered share tracker: no live market data, no multi-device sync, no server-side logic. The ERD defines two related, fixed-shape entities (`SHARE`, `TIMELINE_EVENT`). The product's stated long-term goal includes a desktop app now and an iOS app later, built by someone who wants to stay hands-on and learn the codebase, not hand it off.

Three decisions needed: frontend language/framework, desktop packaging approach, and data storage.

## Decision

- **Frontend:** TypeScript + React
- **Desktop shell:** Electron
- **Data storage:** SQLite (local file, via a lightweight library such as `better-sqlite3`)
- **iOS (later, not MVP1):** React Native, same language as desktop
- **Backend:** None for MVP1 — see rationale below

## Rationale

**TypeScript + React.** Matches the language and mental model already used in the HTML/CSS prototypes built during design. Most widely documented frontend stack available, which matters directly for a builder who wants to understand what they're shipping rather than copy-paste a black box — any error encountered has an existing, well-explained answer online.

**Electron over Tauri (or other lighter alternatives).** Tauri produces smaller, more efficient apps and is a reasonable technical choice for a more experienced team. Electron was chosen instead specifically because of its far larger base of tutorials, examples, and community troubleshooting — a better fit for someone building while learning. This is a deliberate tradeoff of runtime efficiency for learnability; worth revisiting once the builder is more experienced or if app size/performance becomes a real problem.

**SQLite over a NoSQL store (e.g. MongoDB).** The ERD is relational by nature: two fixed-shape entities with a clear one-to-many foreign-key relationship, uniform fields, and typed columns (decimal, date, boolean). NoSQL databases add value when data shape varies per record; that's not the case here, so choosing one would add complexity (a new query paradigm, another dependency) without a matching benefit. SQLite specifically (rather than a client-server SQL database like PostgreSQL) fits because MVP1 is single-user and local — it's a single file on disk, not a hosted service, with no separate database process to install or run.

**No backend for MVP1.** With no live market data and no cross-device sync in scope, there is nothing for a backend to do: the Electron app's local Node.js process reads and writes the SQLite file directly. Introducing a backend now would add a component with no current job. This is a deliberate scope boundary, not an oversight — revisit at MVP2 (live market data requires calling an external API) and again if MVP3+ introduces sync between desktop and iOS.

**React Native for iOS (deferred).** Not built in MVP1, but the choice is recorded now because it shaped the frontend decision above: sharing a language (TypeScript) and framework family (React) between desktop and iOS means logic, types, and lessons learned on desktop largely transfer, even though the UI layer itself will be rebuilt per platform.

## Alternatives considered

| Option | Rejected because |
|---|---|
| Tauri (desktop shell) | Smaller/faster, but thinner documentation and community base — worse fit for learning while building |
| Flutter (single codebase for desktop + iOS) | Most efficient long-term for code sharing, but requires learning Dart, a language with a much thinner troubleshooting trail than TypeScript |
| MongoDB / other NoSQL | Data is uniformly structured and relational; NoSQL would add a new paradigm with no corresponding benefit |
| PostgreSQL / hosted SQL | Requires running a database server; unnecessary for a single-user local app |
| Backend/API from the start | Nothing in MVP1's scope requires server-side logic; would be built ahead of need |

## Consequences

- Electron app bundle size and memory footprint will be larger than a Tauri equivalent — acceptable for a desktop personal-use app, worth monitoring if it becomes annoying in practice.
- The SQLite schema can be derived close to 1:1 from the existing ERD (`docs/erd.md`), so the data model doc and the actual database structure should stay in sync as source and implementation.
- Moving to React Native for iOS will still require learning a new set of platform-specific concerns (navigation, native modules) even though the language stays the same — this is expected, not a sign the desktop choice was wrong.
- Introducing a backend at MVP2 is anticipated, not accidental scope creep — worth a new ADR at that point rather than retrofitting this one.
