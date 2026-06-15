# NHL Player Evaluation — Buffalo Sabres Forwards

Evaluating Buffalo Sabres forwards by even-strength (5v5) production and on-ice
expected-goals share, adjusted for usage (zone starts and ice time), using public
NHL data from the 2025-26 season.

**Data:** [MoneyPuck.com](https://moneypuck.com/data.htm) public skater data, 2025-26 season (5v5). Credit: MoneyPuck.com.

---

## Question

Which Sabres forwards drive play at even strength once you account for how they're deployed?

A player's raw totals are shaped by how much and where they play. This project
normalizes for ice time (per-60 rates) and shows deployment context (offensive vs.
defensive zone starts) alongside results, so players are compared on fairer footing.

## Method

- Filtered to Buffalo forwards at 5v5 with a minimum ice-time threshold (100 minutes) to reduce small-sample noise
- Converted counting stats to **per-60 rates**: points, goals, shots, and individual expected goals (ixG)
- Computed **on-ice expected-goals share** = xGF / (xGF + xGA) — the share of total expected goals that belong to Buffalo while the player is on the ice
- Added **offensive zone start %** = O-zone starts / (O-zone + D-zone starts) as deployment context
- Visualized deployment vs. territorial impact, and ranked forwards by xG share

## Validation

- **Cross-checked the xG-share calculation against MoneyPuck's own published `onIce_xGoalsPercentage`.** An initial comparison showed large discrepancies, which on inspection were a units mismatch (MoneyPuck stores the figure as a 0–1 proportion; mine was on a 0–100 scale). After aligning scales, my values matched theirs to within their rounding precision (all differences < 0.5).
- **Sanity-checked rate-stat rankings against known player roles:** the team's top producers (Thompson, Tuch) ranked at the top on production, and depth forwards ranked at the bottom — consistent with their real usage.
- **Caught and fixed a bug** where offensive zone start % returned exactly 50% for every player (a copy-paste error pulling the defensive-zone column twice). A suspiciously uniform result flagged the issue; the corrected metric showed realistic deployment spread (24%–63%).

## Findings

- **Deployment alone does not explain territorial results for this roster.** There is no strong relationship between how sheltered a forward is and his xG share — suggesting individual ability and linemate context matter more than zone starts here.
- **Josh Dunne stands out for difficulty of minutes:** the lowest offensive-zone-start share on the team (~24%) yet an above-even xG share (~51%) — holding his own territorially while starting consistently in the defensive zone.
- **The top producers don't dominate territory.** Thompson and Tuch get the most offensive-zone deployment (~59–61%) but sit right around a 50% xG share. They drive personal offense (highest shot and points rates) without tilting overall territory — plausibly a function of competition quality or a high-event style, which this dataset can't fully isolate.
- **Zach Benson tilts the ice without being sheltered** (~49% O-zone starts, ~53% xG share), a positive two-way signal for a young player.

## Limitations

- Single season (2025-26) is a relatively small sample; rate stats remain noisy for low-ice-time players (e.g., Helenius, Kulich), who should be read with caution.
- Expected goals is a model, not ground truth; results inherit MoneyPuck's xG assumptions.
- No quality-of-competition or linemate adjustment, so "territorial impact" can't be cleanly separated from who a player is deployed with and against.
- Analysis is 5v5 only; special-teams contribution is excluded.

## Next steps

- Add multi-season data to stabilize rate stats and look at trends/age curves
- Incorporate quality-of-competition and with-or-without-you (linemate) context
- Extend to defensemen and to special-teams situations
- Build out shot-level analysis using MoneyPuck's shot data for a custom expected-goals view

## Files

- `notebook.ipynb` — full analysis, top to bottom
- `figures/usage_vs_production.png` — deployment vs. xG share scatter
- `figures/xg_share.png` — forwards ranked by xG share

---

*Data courtesy of MoneyPuck.com. For non-commercial, educational use.*
