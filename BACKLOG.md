# Enhancement Backlog

Living backlog for the Itch Tracker dashboard (`index.html`). Grouped by theme, roughly
in priority order within each group. Check items off as they ship; convert an item to a
GitHub issue when work actually starts (reference the issue number back here) rather
than opening issues for the whole backlog up front.

## Privacy / Security

- [x] ~~Stop exposing the live data feed in a public repo~~ — **won't fix.** Reviewed
      and declined: the sheet has no identifiable information, so the exposure isn't a
      meaningful risk, and the tradeoff (pasting a CSV URL on every use) wasn't worth
      it. Closed as [#1](https://github.com/jluzier/itch-tracker/issues/1).

## Data & Insights

Fields already captured per entry but never surfaced in any chart or callout:
`stress`, `sleep`, `hydration`, `tempHi`, `humidity`.

- [ ] Surface stress / sleep / hydration / weather correlations against itch level —
      cheapest high-value win since the data is already being collected.
- [ ] Lagged-effect view — all current comparisons (alcohol, Vanicream, histamine foods)
      are same-day only. Add a "next-day itch" comparison alongside same-day, since
      flares from food/alcohol/hormones often show up 12–48h later.
- [ ] Show sample size (n) and spread on insight callouts, not just an average — with
      small samples, "6.2 vs 3.1" can read as a stronger signal than it is.
- [ ] Multi-factor combinations — e.g. alcohol-during-luteal vs alcohol-during-follicular.
      Factors are currently only ever evaluated in isolation.

## Clinical / Doctor-Visit Use

- [ ] Printable one-page visit summary — condensed export of the top insights across all
      three tabs, since this app exists to support an APD conversation with a provider.
- [ ] CSV/JSON export of the processed dataset, for handing to a provider directly
      rather than linking the raw sheet.

## Data Quality / Robustness

- [x] Manual cycle-start override — added an "Edit cycle starts" panel on the Cycle
      Mapping tab to exclude a bad auto-detected date or add a missed one, stored as a
      diff in localStorage on top of `detectCycleStarts()` (always resettable).
      Closed as [#2](https://github.com/jluzier/itch-tracker/issues/2).
- [x] Logging-gap awareness — Timeline tab now shows a current-streak stat, a
      logging-gaps stat, and a callout listing the largest gaps by date range.
      Closed as [#3](https://github.com/jluzier/itch-tracker/issues/3).

## UX

- [ ] Date range filter on the Timeline chart and the Diet log table — the log table
      currently renders every row ever logged and will get unwieldy as data grows.
- [ ] Colorblind-safe severity encoding — severity is currently color-only
      (red/amber/teal); add a text or shape fallback.
- [ ] Dark mode.
