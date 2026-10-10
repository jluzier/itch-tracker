# Enhancement Backlog

Living backlog for the Itch Tracker dashboard (`index.html`). Grouped by theme, roughly
in priority order within each group. Check items off as they ship; convert an item to a
GitHub issue when work actually starts (reference the issue number back here) rather
than opening issues for the whole backlog up front.

## Next Up

Cross-group priority order for the open items below:

1. Cycle pattern items (below) — the leading theory is a menstrual-cycle link (APD);
   these make the pattern checkable across cycles and shareable with doctors.
2. CSV/JSON export (Clinical) — small and self-contained.
3. Colorblind-safe severity (UX) — mostly limited to the timeline chart points.
4. PCP visit summary (Clinical) — needs layout decisions.
5. Lagged-effect view (Data & Insights) — deferred while the cycle theory is explored.
6. Treatments tried (Clinical) — deferred: needs a new form field, and the form is not
   changing for now.

## Cycle Pattern

Built only on existing sheet columns — no form changes.

- [x] Cycle data quality — `buildCycles()` splits history into cycles using
      calendar-day math and flags any cycle over 35 days or containing a 7+ day logging
      gap (e.g. the Jul 25 → Oct 8 "75-day cycle" hiding the Aug–Sep gap). Flagged
      cycles show the reason on the Cycle banner and are excluded from cross-cycle stats.
- [x] All-cycles heatmap — new "All Cycles" view on the Cycle tab: one row per cycle,
      cells colored by itch with the number printed, Valtrex days dotted, unlogged days
      dashed. Toggle between "Days before next period" (−21…+7 around each period start,
      premenstrual week and −14 marked) and "From period start" (cycle day 1–35).
      Flagged cycles are dimmed with a † and the reason in the tooltip.
- [x] Per-cycle table + pooled stats — "Cycle by cycle" table under the heatmap with
      menses (d1–5), early follicular (d6–10), luteal (last 14d) and premenstrual
      (last 7d) averages with n, plus flare onset (first itch ≥5 after day 5) and the
      period day itch settled to ≤2. A pooled callout summarizes complete, unflagged
      cycles only.
- [x] Expected flare window — blue callout at the top of the Cycle tab: next period =
      last start + median complete-cycle length; window opens at the earliest observed
      flare onset and runs to period day 4. Past predictions are scored (period days
      early/late, flare onset vs. window) using only data available at the time; warns
      when no period is logged a week past the expected date.
- [x] Visit Summary cycle section — backward-aligned heatmap, pooled cycle line, and
      per-cycle table on the printable page, replacing the old forward-counted "Cycle
      phase" factor row. Tightened spacing elsewhere (sparkline, table rows, top 4
      itch locations, 120-char note excerpts) to keep it on one letter page.

## Privacy / Security

- [x] ~~Stop exposing the live data feed in a public repo~~ — **won't fix.** Reviewed
      and declined: the sheet has no identifiable information, so the exposure isn't a
      meaningful risk, and the tradeoff (pasting a CSV URL on every use) wasn't worth
      it. Closed as [#1](https://github.com/jluzier/itch-tracker/issues/1).

## Data & Insights

- [x] Surface stress / sleep / hydration / weather correlations against itch level —
      new Factors tab: multi-select toggles for stress/sleep/hydration, one scatter per
      selected factor with n and Pearson r, plus separate temp and humidity charts.
- [ ] Lagged-effect view — all current comparisons (alcohol, Vanicream, histamine foods)
      are same-day only. Add a "next-day itch" comparison alongside same-day, since
      flares from food/alcohol/hormones often show up 12–48h later.
- [x] Show sample size (n) and spread on insight callouts, not just an average — with
      small samples, "6.2 vs 3.1" can read as a stronger signal than it is. Added
      `n=` and min–max range to the alcohol, Vanicream, and histamine callouts, plus a
      "small sample" caution note (n<5) across those and the four cycle-phase callouts.
- [x] Multi-factor combinations — Combinations card on the Factors tab. Pick any two
      factors: categorical × categorical shows grouped bars with n per cell (cells under
      n=3 hidden); categorical × numeric shows a scatter split by group with r per group.
- [x] Justin home/away/in-NY correlation — new "Avg itch: Justin home vs away" bar
      chart + insight callout on the Timeline tab, parsing the sheet's new `Justin?`
      column (Yes/N/In NY) added to track whether Stefanie being with vs. away from
      Justin correlates with itch level. The Timeline chart and callout were later
      removed; Justin remains a factor in the Combinations card and in the timeline
      point tooltips.

## Clinical / Doctor-Visit Use

- [x] Printable one-page visit summary — Visit Summary tab with coverage, KPIs, itch
      sparkline, factor table with n, itch locations, and severe-day note excerpts.
      Print CSS limits output to one letter page. Data is cut off before the most recent
      14+ day logging gap.
- [ ] Visit summary: treatments tried — add a field for medications and topical
      treatments so the summary can show what was used during each period.
- [ ] CSV/JSON export of the processed dataset, for handing to a provider directly
      rather than linking the raw sheet.
- [ ] Visit summary: PCP version — broader overall-burden framing (sleep, stress,
      cycle) for a primary care visit, vs. the dermatology-focused layout.

## Data Quality / Robustness

- [x] Manual cycle-start override — added an "Edit cycle starts" panel on the Cycle
      Mapping tab to exclude a bad auto-detected date or add a missed one, stored as a
      diff in localStorage on top of `detectCycleStarts()` (always resettable).
      Closed as [#2](https://github.com/jluzier/itch-tracker/issues/2).
- [x] Logging-gap awareness — a current-streak stat (later promoted to the header,
      see below) plus a Timeline callout listing the largest gaps by date range. A
      logging-gaps stat also shipped but was later dropped from the header as not
      pulling its weight. Closed as
      [#3](https://github.com/jluzier/itch-tracker/issues/3).

## UX

- [x] Date range filter on the Timeline chart and the Diet log table — added
      7d/30d/90d/All-time pill filters to each independently (Timeline's streak/gap
      stats stay computed over full history regardless of the selected window). The
      Timeline now defaults to 30d; the Diet log still defaults to All time.
- [ ] Colorblind-safe severity encoding — most places already pair the color with the
      number; the main color-only spot is the point colors on the itch level over
      time chart. Add a shape or tooltip fallback there.
- [x] Dark mode — toggle button in the header, persisted in `localStorage`, defaulting
      to the OS `prefers-color-scheme` on first visit. Accent colors (teal/amber/red/
      blue/navy) stay constant across themes; only surfaces, text, and chart grid/tick
      colors invert.
- [x] Cross-tab glance stats in the header — Last log itch level and Current streak
      shown as compact chips directly under the "Itch Tracker" title, visible
      regardless of which tab is active ("Last fetched" moved to the right-side
      controls to make room). First pass reused the full `.stat` card component and
      looked too tall; replaced with small pill-style chips. A Logging gaps chip was
      also tried and later dropped. Removed the now-duplicate tiles from Timeline's
      strip and the old header text line they replaced.
