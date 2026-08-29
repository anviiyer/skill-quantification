# Skill Quantification of Trading Platforms

**Are FPI equity traders skilled, or just lucky?**

This project applies a four-test statistical framework - originally developed for opinion trading platforms - to real-world Foreign Portfolio Investment (FPI) equity trading data from SEBI, to empirically test whether trading success reflects skill or chance.

## Overview

The question of whether success in trading is driven by skill or chance carries real theoretical and regulatory weight. This project adapts the methodology of Bagchi et al. (2025) - originally applied to the Probo opinion trading platform - to SEBI's trade-wise FPI equity data for **January-March 2025**.

Four statistical tests are used to probe for a skill signal:

1. **Skill Dilution Test** - randomly flips trade outcomes and checks whether performance degrades, which would indicate genuine skill in the original outcomes.
2. **Persistence Test** - checks whether a user's performance in one month correlates with their performance the next (Spearman correlation).
3. **Learning Test** - checks whether win rate improves as users accumulate more completed trades.
4. **Skill Gradient Test (OpTraS Scoring System)** - a composite score combining performance, strategy, activity, and foresight, used to compare consistent vs. inconsistent traders.

## Success Metrics

For each user `u`:

A **position** = a `(SUB_ACC, ISIN)` pair with both a buy and sell leg present. A position is a **WIN** if average sell price > average buy price. Only users with ≥ 20 trade records are included.

## Dataset

- **Source:** SEBI trade-wise FPI equity data, January–March 2025
- **Key columns used:** `SUB_ACC` (user ID), `ISIN`, `TR_TYPE` (buy/sell classification), `QUANTITY`, `VALUE`, `TR_DATE`
- Rows with missing `SUB_ACC` are dropped; users with < 20 total trades are excluded, matching the threshold in the original paper.

## OpTraS Scoring System

A composite skill score combining four exponentially-decayed components (recent trades weighted more heavily):

| Component | Symbol | Weight | Measures |
|---|---|---|---|
| Performance | π | 40% | Exponentially weighted sum of returns |
| Strategy | ϱ | 40% | Exponentially weighted avg + median per-position ROI |
| Activity | θ | 10% | Engagement, sigmoid-compressed by percentile rank |
| Foresight | ϕ | 10% | Fraction of trades that are sell-side (proactive exits) |

Users are classified as **Consistent** (≥ 20 trades in any single calendar week) or **Inconsistent**, and weekly mean OpTraS scores are compared across the two groups.

## Key Results

- **Skill Dilution Test:** Baseline mean win rate ≈ 0.42 (below 0.5, reflecting a falling January 2025 market). As outcomes are randomly flipped, WR rises monotonically toward ~0.58 - direction is inverted relative to the original paper (which had baseline WR > 0.5), but the ~0.16 gap still confirms outcomes are systematically non-random.
- **Persistence Test:** Tested via one-sided Spearman correlation of win rate across consecutive months (Jan-Feb, Feb-Mar 2025), plus a 3-month pairwise correlation heatmap.
- **Learning Test:** Median completed positions per user is only 4 (vs. hundreds on Probo), limiting statistical power. From rank ~50 onward, WR stabilizes and trends upward toward ~0.62, supporting a learning effect.
- **OpTraS Skill Gradient:** For most of January, Consistent traders outscore Inconsistent traders (mean scores ~510–600 vs. ~465-560), with a late-month crossover attributable to small sample size.

**Overall:** Across all four tests, results are broadly consistent with a **skill-based interpretation** of FPI trading - though sparse position data, a sub-0.5 baseline win rate, and a single-month window for most tests introduce important caveats.

## References

1. Bagchi, A., Pal, A., & Kumar, S. (2025). *Quantifying Skill on Opinion Trading Platforms*. Technical report.
2. Steyvers, M., & Benjamin, A. S. (2019). The Joint Contribution of Participation and Performance to Learning Functions. *Behavior Research Methods*, 51, 1531-1543.
3. Stafford, T., & Vaci, N. (2022). Maximizing the Potential of Digital Games for Understanding Skill Acquisition. *Current Directions in Psychological Science*, 31(1), 49-55.
