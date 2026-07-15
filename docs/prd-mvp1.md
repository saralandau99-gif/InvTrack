# InvTrack — MVP1 PRD
*Personal Trading Journal (shares, manual entry)*

Companion to the InvTrack One-Pager. This document breaks MVP1 into epics and user stories with acceptance criteria. Primary persona: **Noa** (learning investor). Secondary persona **Amit** is referenced only where a story anticipates his needs without adding MVP1 scope.

---

## Epic 1 — Track a share (add, edit, archive)
*Core CRUD for the object the whole app is built around.*

**US-1.1** — As Noa, I want to add a share I'm watching or holding, so that it appears in my daily view.
- Acceptance criteria:
  - Form requires: ticker, status
  - Form allows (optional depending on status): quantity owned, buy price, buy date, thesis, tags, target sell price, stop-loss price, review date
  - Ticker is validated as non-empty; no live market lookup in MVP1 (manual entry)
  - On save, share appears immediately in the dashboard under the correct section (Needs attention / All positions)

**US-1.2** — As Noa, I want to edit any field on a share I've already added, so that I can update my plan as things change.
- Acceptance criteria:
  - Edit opens the same form, pre-filled with current values
  - Saving an edit creates a new entry in the share's timeline (see Epic 3) recording what changed
  - Editing does not require re-entering unchanged fields

**US-1.3** — As Noa, I want to archive a share that's no longer relevant to me, so that my daily view stays focused without losing the history.
- Acceptance criteria:
  - Archive is available from the dashboard row and from the edit form
  - Archived shares are hidden from the default dashboard view but remain accessible from an "Archived" filter
  - Archiving does not delete timeline history or require quantity to be 0
  - Archiving is reversible (unarchive)

**US-1.4** — As Noa, I want to mark a share as Sold, so that closed positions are distinguished from ones I'm still deciding on.
- Acceptance criteria:
  - Setting status to Sold prompts for sell price and sell date
  - Sold shares remain visible under a distinct "Sold" filter (not the same as Archived — see one-pager for the distinction)
  - A Sold share automatically drops out of the "Needs attention" section

---

## Epic 2 — Reflect intent through status and triggers
*The status model and target/stop fields that make the app forward-looking, not just a log.*

**US-2.1** — As Noa, I want to set a status for each share (Watching, Waiting to buy, Holding, Waiting to buy more, Waiting to sell, Sold), so that the app reflects what I actually intend to do next.
- Acceptance criteria:
  - Status is a required field, defaulting to "Watching" for new shares with 0 quantity
  - Changing status is always available from the edit form
  - Status directly drives which dashboard section the share appears in

**US-2.2** — As Noa, I want to set a target sell price and/or a stop-loss price, so that I have a pre-committed plan instead of deciding emotionally in the moment.
- Acceptance criteria:
  - Both fields are optional and independent (a share can have one, both, or neither)
  - When today's manually entered price crosses either threshold, the share moves into "Needs attention" with a visible reason (e.g. "target reached" / "stop-loss breached")

**US-2.3** — As Noa, I want to set a review date instead of a price trigger, so that shares without a clear price target still get revisited (e.g. "check back after earnings on [date]").
- Acceptance criteria:
  - Review date is optional and independent of price triggers
  - A share whose review date has passed appears in "Needs attention" with the reason "review due"

*(Forward note for MVP2, not in scope now: Amit would want these same fields enforced as hard rules with alerts — MVP1 only needs to display them, not enforce or notify.)*

---

## Epic 3 — See the story behind a share (timeline)
*Turns a static record into a log of reasoning over time.*

**US-3.1** — As Noa, I want to write down why I'm buying or watching a share, so that I can check my past reasoning against what actually happened.
- Acceptance criteria:
  - Thesis is a free-text field, optional but prompted at creation
  - Tags are optional, free-text, comma-separated (no fixed taxonomy in MVP1)

**US-3.2** — As Noa, I want to see a chronological timeline for each share, so that I can review its whole history in one place.
- Acceptance criteria:
  - Timeline shows, in order: creation, every price update, every status change, every edit, sell (if applicable)
  - Each timeline entry shows date and a short auto-generated description (e.g. "Status changed: Watching → Holding")
  - Timeline is read-only (edits happen through the share form, not directly on timeline entries)

---

## Epic 4 — Know what needs attention today (dashboard)
*The daily-use surface — the feature that determines whether the app earns a place in the morning routine.*

**US-4.1** — As Noa, I want to open the app and immediately see which shares need a decision today, so that I don't have to scan my whole list every morning.
- Acceptance criteria:
  - "Needs attention" section shows shares where: target hit, stop-loss breached, or review date passed
  - Section is empty (and visibly says so) when nothing needs attention — not just hidden
  - Sorted by most urgent first (breached stop-loss above target-hit above overdue review, ties broken by how overdue)

**US-4.2** — As Noa, I want to see my full list of tracked shares below the urgent section, grouped or filterable by status, so that I still have visibility into everything I'm watching.
- Acceptance criteria:
  - All non-archived, non-sold shares are listed regardless of urgency
  - Each row shows: ticker, status badge, quantity, current price
  - Edit and archive actions available inline on every row

**US-4.3** — As Noa, I want to see my overall portfolio value and unrealized P&L at a glance, so that I have context before drilling into individual shares.
- Acceptance criteria:
  - Portfolio value = sum of (quantity × latest manually entered price) across held positions
  - Unrealized P&L = current value minus cost basis, shown with clear positive/negative styling
  - Values update immediately after any price entry or edit

---

## Epic 5 — Keep prices current (manual entry)
*Deliberately manual in MVP1 — see one-pager rationale.*

**US-5.1** — As Noa, I want to quickly enter today's price for a share, so that my dashboard and P&L stay accurate without much daily effort.
- Acceptance criteria:
  - Price entry takes no more than 2 taps/clicks per share from the dashboard (no need to open the full edit form)
  - Entering a price creates a timeline event
  - If no price is entered on a given day, the app uses the last known price and does not block usage

---

## Epic 6 — Take my data with me (export)
*Small in scope, disproportionately valuable — bridges to MVP4 (AI insights) and general data ownership.*

**US-6.1** — As Noa, I want to export my full list of tracked shares and positions, so that I can review it elsewhere or share it with an AI tool to learn from my own decisions.
- Acceptance criteria:
  - Export produces a single file (CSV or JSON) containing all non-deleted shares: ticker, status, quantity, buy price/date, thesis, tags, target/stop, review date, current price, timeline summary
  - Archived and Sold shares are included, clearly flagged, not silently dropped
  - Export is a single action from the dashboard, no configuration required for MVP1

---

## Out of scope for MVP1 (confirm not accidentally built)
- Any live market data or price API integration (MVP2)
- Push notifications/alerts (MVP2)
- Any asset type other than shares (MVP3)
- AI-generated insights or predicted-vs-actual analysis (MVP4) — export in Epic 6 exists specifically to enable this later, not to perform it now
