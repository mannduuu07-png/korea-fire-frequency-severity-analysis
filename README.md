# Korea Fire Frequency-Severity Analysis

**Does fire frequency explain a region's damage risk?** Analysis of
10 years (2015–2024) of nationwide Korean fire statistics shows the
answer is largely no — fire frequency and per-fire casualty severity
are only weakly correlated, and a small number of regions show
persistently high casualty rates that volume-based prioritization
would miss entirely.

![Frequency vs Severity](frequency_vs_severity_scatter.png)

## Key Findings

- **Nationwide trend**: fire count down 11.7% (2015–17 avg vs
  2022–24 avg), while property damage rose 116.6% (nominal terms).
  Death counts were flat (+3.2%, not a meaningful trend).
- **Frequency and severity are only weakly related**: Spearman
  correlation between cumulative fire count and casualties-per-100-
  fires is ρ=0.10 (p=0.11, not significant) in the primary analysis,
  and ranges 0.05–0.21 across sampling thresholds — never strongly
  or consistently significant. Frequency alone is not a reliable
  predictor of casualty risk.
- **청주시상당구 (Cheongju Sangdang-gu) is the strongest individual
  case**: high-severity in both the 2015–19 and 2020–24 sub-periods,
  and shows ~1.6x excess casualty risk even after controlling for
  place-type composition (residential/industrial/etc.) and excluding
  its single worst incident. Not explained by chance, a mass-casualty
  outlier, or what kind of places its fires occur in.
- **24 regions** show excess risk ≥1.3x under the same place-mix
  control, forming a candidate priority list — though only 청주시상당구
  has been cross-validated against the time-period split.
- **Confirmed annual seasonality**: fires peak in March and winter
  months; STL decomposition and ACF (lag=12: ρ=0.49; lag=24: ρ=0.46,
  both significant at α=0.05) confirm this is a genuine recurring
  cycle, not noise in year-by-year averages.

## Repository Structure
