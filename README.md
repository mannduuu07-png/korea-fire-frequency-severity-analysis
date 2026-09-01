# Korea Fire Frequency-Severity Analysis

**Does fire frequency explain a region's damage risk?** Analysis of
10 years (2015–2024) of nationwide Korean fire statistics shows the
answer is largely no — fire frequency and per-fire casualty severity
are only weakly, and not statistically significantly, related. A
small number of regions show persistently high casualty rates that
volume-based prioritization would miss entirely.

![Frequency vs Severity](outputs/figures/frequency_vs_severity_scatter.png)

## Key Findings
- **Nationwide trend**: fire count down 11.7% (2015–17 avg vs
  2022–24 avg), while property damage rose 116.6% (nominal terms).
  Death counts were flat (+3.2%, not a meaningful trend).
- **Fire frequency does not reliably predict per-fire casualty
  severity**: Spearman correlation between cumulative fire count and
  casualties-per-100-fires is ρ=0.051 (p=0.424, not significant) in
  the primary analysis (247 regions, 300+ cumulative fires). This
  holds regardless of sampling threshold (ρ=0.05–0.07, p>0.29 at
  200/300/500-fire thresholds) and weakens further when the period is
  split (2015–19: ρ=0.043, p=0.504; 2020–24: ρ=0.121, p=0.058,
  borderline).
- **청주시상당구 (Cheongju Sangdang-gu) is the strongest individual
  case**: the only region that appears in the high-severity top 10 in
  *both* the 2015–19 and 2020–24 sub-periods, and shows 2.26x excess
  casualty risk versus what its place-type mix (residential/
  industrial/etc.) would predict — 1.59x even after excluding its
  single worst incident. Not explained by chance, a mass-casualty
  outlier, a sampling threshold, or the kinds of places its fires
  occur in.
- **21 regions** show excess risk ≥1.3x under the same place-mix
  control (leave-one-region-out baseline), forming a candidate
  priority list — though only 청주시상당구 has been cross-validated
  against the time-period split; treat the rest as exploratory.
- **A 강원특별자치도 (Gangwon) cluster emerged after fixing a region-
  naming bug**: 5 of the top 15 excess-risk regions (삼척시, 양양군,
  원주시, 태백시, 춘천시) are in Gangwon province — a pattern that was
  invisible before administrative-boundary name changes (e.g. 강원도 →
  강원특별자치도, 2023) were corrected, because affected regions were
  being split into two separate, under-threshold entries. See
  `docs/validation_log.md`.
- **Confirmed annual seasonality**: fires peak in March and winter
  months; STL decomposition and ACF (lag=12: ρ=0.49; lag=24: ρ=0.46,
  both significant at α=0.05) confirm this is a genuine recurring
  cycle, not noise in year-by-year averages.


## Data
**Source**: 소방청 (Korea National Fire Agency) 연간화재통계 (Annual Fire
Statistics), 2015–2024, published via [data.go.kr](https://www.data.go.kr).
Incident-level records (~406,000 rows) with date, region, ignition
source, place type, and casualty/property damage figures. Raw files
are not included in this repo; see `docs/data_inventory.md` for
download details.

## Methodology Highlights
- **Region identification bugs found and fixed**: yearly files used
  inconsistent column names (e.g. `일시` vs `화재발생년월일`), and
  district names alone are not unique identifiers — 중구 exists in 6
  different cities nationwide, so an earlier version of this analysis
  (comparing regions by name only) miscounted two different cities as
  one persistent region. A second, separate bug was found later:
  administrative renamings during 2015–2024 (강원도→강원특별자치도,
  전라북도→전북특별자치도, 군위군 province reassignment, and 부천시's
  district structure being abolished in 2016 and reinstated in 2024)
  caused several regions to be split across two labels and
  undercounted. Both are fixed by normalizing to a consistent
  `(시도, 시군구)` identity before any aggregation. See
  `docs/validation_log.md` for full details.
- **Leave-one-out baseline**: when computing a region's "expected"
  casualty rate from national place-type statistics, that region's
  own data is excluded from the baseline to avoid its own extreme
  values inflating its own benchmark.
- **Worst-incident exclusion**: per-region statistics are recomputed
  excluding each region's single most severe incident, to confirm
  results aren't artifacts of one mass-casualty event (e.g. this
  ruled out 과천시 and 밀양시 as false positives — see
  `05_excess_risk_analysis.ipynb`).
- **Primary analysis + sensitivity checks**: main results use a
  300+ cumulative fires threshold (stated a priori); robustness
  confirmed at 200 and 500 as well.

## Limitations
- These are correlational, exploratory findings, not causal claims.
- 20 of the 21 priority-list regions are flagged by the place-mix
  control only; only 청주시상당구 has been additionally verified
  against the 2015–19/2020–24 time-period split.
- No confidence intervals or multiple-comparison correction have been
  applied to the excess-risk ratios across 247 simultaneously
  compared regions; the priority list should be read as exploratory
  candidates, not statistically confirmed outliers.
- Property damage figures are nominal (not inflation-adjusted).
- This project stops at identifying and characterizing the pattern.
  It does not build a predictive model — see Phase 2 below.

## Phase 2 (Planned)
Incorporate building age (건축물대장), elderly population ratio
(KOSIS), and fire station response distance as candidate explanatory
variables for the excess risk observed in priority regions — including
the newly identified Gangwon cluster — and test whether they support
a forecasting model.

## Reproducing This Analysis
Each notebook is self-contained after `01_data_cleaning.ipynb` has
been run once (it saves a cleaned dataset to Google Drive that later
notebooks load). Open in Colab via the badge at the top of each
notebook.

## AI Assistance
This project's analysis was developed with AI assistance (used for
statistical method suggestions, code review, and cross-checking
claims — including catching several bugs and unverified sources
along the way). See `docs/ai_assistance.md` for details.
