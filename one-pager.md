# InvTrack — One-Pager

## Vision

Most investment trackers answer "what do I own and what's it worth?" They're spreadsheets with prettier UI. **InvTrack answers a different question: "why did I make this decision, and what happens next?"**

Individual investors — especially those learning the market with small money — don't lose because they can't see their portfolio value. They lose because they forget *why* they bought something, second-guess a sound plan mid-swing, or hold past the point their own logic told them to sell. InvTrack is a daily-use companion that captures the reasoning behind every position, not just the position itself, and surfaces exactly what needs attention each morning.

**Target user (v1):** an individual, self-directed retail investor — starting small, learning how the market behaves, and wanting to build the discipline of trading with a written plan instead of gut reactions.

**Long-term vision:** the same "position + reasoning + intent" model extended across every asset class a person holds — cash, real estate, collectibles, crypto, pensions and investment funds — giving one honest view of net worth *and* the logic behind it.

---

## Personas

**1. Noa, the Learning Investor (primary persona for MVP1)**
Late 20s, has a stable job unrelated to finance, and started investing a small amount six months ago "to learn by doing." She reads financial news most mornings across a few different channels/sources before work, but struggles to remember which headline was actually relevant to which stock she owns. She has bought shares on conviction before and then sold too early out of anxiety — or held too long out of hope — because she had no written plan to check herself against. She doesn't need professional-grade analytics; she needs a mirror that shows her own reasoning back to her before she acts on impulse. Success for Noa looks like: opening the app for 5 minutes every morning and knowing exactly what to watch and why.

**2. Amit, the Aspiring Systematic Trader (secondary persona, relevant from MVP2 onward)**
Early 30s, more experienced than Noa, trades a modestly larger portfolio, and increasingly wants his decisions to follow rules rather than emotion. He already keeps a rough mental (or spreadsheet) system of price targets and stop-losses but finds it tedious to maintain and easy to ignore under pressure. He's less interested in journaling his feelings and more interested in **discipline enforcement** — a system that makes his own rules visible and hard to quietly break. He represents where the product can grow without abandoning Noa: same data model, higher rigor.

---

## Market Research — Existing Tools

| Tool | Core Model | Strength | Gap vs. InvTrack |
|---|---|---|---|
| **TradeZella / TraderSync / Tradervue** | Log *closed* trades, analyze performance (win rate, R-multiples, strategy tagging) after the fact | Deep post-trade analytics, broker sync, pattern discovery | Backward-looking — reviews trades once they're done, not built around live open questions like "what's my trigger to act on this now" |
| **Edgewonk** | Similar closed-trade journal, simpler pricing (flat annual fee) | Straightforward, affordable | Same backward-looking model as above |
| **TradesViz** | High-volume analytics platform: seasonality data, broker auto-sync, AI Q&A over trade history | Extremely data-dense, strong for high-frequency traders | Built for volume and depth, not for a beginner's simple daily "what needs my attention" view — steep for the target persona |
| **Plancana** | Mobile-first manual journal emphasizing emotional/behavioral tagging per trade | Captures the *why* and emotional state, good behavioral insight | Still trade-centric (post-decision logging), not position-and-watchlist-centric; no explicit "trigger" concept for future action |
| **Stonk Journal / free spreadsheets** | Fully manual trade logging, no automation | Free, simple, flexible, supports any asset | No structure around ongoing intent — a static log, not a living daily view |
| **"Thesis" (usethesis.com)** | Investment tracker positioned around clearer decisions to hold or exit | Closest existing concept to InvTrack's core idea | Validates the market need; a nudge to differentiate further via the explicit status model and morning-triage view, not just naming |

**Takeaway:** the trading-journal category is well-served for **active traders reviewing performance after the fact**. It is thin on tools built around a **beginner's forward-looking daily ritual** — "here's what I'm watching, here's my plan, here's what needs a decision today." That gap is InvTrack's opening, and it's also the clearest way to differentiate a portfolio narrative from "another trading journal."

---

## Core Product Principle

Every asset in the system carries three things, not one:
1. **The position** — what you hold (or are watching), and how much.
2. **The thesis** — why you made this decision, in your own words.
3. **The intent** — what has to happen for you to act again (a price target, a stop-loss, a review trigger).

This triad is what separates InvTrack from a portfolio tracker or a post-trade journal — and it's the design decision the whole roadmap is built to protect, even as scope grows.

---

## MVP Roadmap

### MVP1 — Personal Trading Journal (shares, manual entry)
*Goal: prove the "position + thesis + intent" model is genuinely useful in daily habit form, before adding any technical complexity.*

- Add a share: ticker, quantity currently owned (can be 0 — i.e., watchlist-only), buy price & date (if owned)
- **Status per share**, reflecting the user's actual intent:
  - Watching · Waiting to Buy · Holding · Waiting to Buy More · Waiting to Sell · Sold
- Thesis field: free-text "why," optional tags (e.g. earnings play, long-term, dividend)
- Intent rules: target sell price, stop-loss price, and/or a review date
- **Timeline per share**: chronological log of price updates, status changes, and notes — the share's story, not just its current number
- Daily home view: positions + watchlist, sorted by "needs attention" (near target/stop, or a stale review date)
- Manual daily price entry (~30 seconds/morning per share)
- Simple P&L, per-position and portfolio-level
- **Archive** action (removes an irrelevant share from the daily view without deleting its history) — distinct from **Sold** status (a closed position, still part of the record)
- **Export** — download the full list of tracked shares and positions (e.g. CSV/JSON), so the data can be shared with any external tool, including feeding it to an AI for review and learning

### MVP2 — Live Market Data
*Goal: remove the manual friction once the habit and data model are validated.*

- Auto-pull live prices via a market data API (replaces manual entry)
- Push/in-app alerts when a target or stop-loss is hit
- Historical price chart per share, overlaid with the user's own timeline events

### MVP3 — Multi-Asset Portfolio
*Goal: generalize the model beyond shares.*

- Extend position + thesis + intent to: cash savings, real estate, collectibles (wine, whiskey, etc.), bitcoin/crypto, pensions and investment funds
- Cross-asset net worth view
- Asset-class-specific fields where needed (e.g. real estate has no daily price — review cadence replaces price triggers)

### MVP4 — AI-Powered Insights
*Goal: use the accumulated thesis + intent + outcome data to surface what the manual eye misses.*

- Predicted-vs-actual review: for each closed or long-held position, compare the original thesis/target against what actually happened
- Pattern surfacing across positions (e.g. "positions tagged 'earnings play' hit target 40% of the time vs. 70% for 'long-term'")
- Depends on MVP1's export/data model being rich enough to analyze — a natural reason MVP1's export feature earns its place early

### Later / Not Yet Scoped
- iOS app (post-desktop validation)
- Multi-device sync
- Tax-reporting exports

---

## Why This Sequencing (the PM logic)

- **MVP1 is deliberately data-source-agnostic (manual entry).** It de-risks the *product* question (does this habit and data model actually help?) before the *engineering* question (live data integration). Building live data first would mean building infrastructure before knowing if the core loop is worth using daily.
- **Shares before other asset classes.** Shares have the highest decision frequency and the clearest "trigger" logic (price crosses X). They're the hardest test of the thesis+intent model — if it holds up here, it generalizes more easily to slower-moving assets like real estate.
- **Personal utility is the primary success metric**, not feature breadth. If MVP1 doesn't get opened every morning, no later MVP is worth building.

---

## Open Questions to Resolve Before Build
- Desktop tech stack (native vs. cross-platform, given the later iOS goal)
- Where "review date" triggers surface if no price event happens (notification? just top of daily view?)
- Data export/backup approach for MVP1 (local file? later cloud sync ahead of MVP2?)
