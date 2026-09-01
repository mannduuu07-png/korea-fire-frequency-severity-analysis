# Korea Fire Frequency-Severity Analysis

**Does fire frequency explain a region's damage risk?** Analysis of
10 years (2015–2024) of nationwide Korean fire statistics shows the
answer is largely no — fire frequency and per-fire casualty severity
are only weakly, and not statistically significantly, related. A
small number of regions show persistently high casualty rates that
volume-based prioritization would miss entirely.

![Frequency vs Severity](frequency_vs_severity_scatter.png)

## Key Findings
* Nationwide trend: fires down 11.7%, property damage up 116.6% (2015-17 vs 2022-24 avg); death counts flat.
* Fire frequency doesn't reliably predict severity: weak, not statistically significant correlation (ρ=0.05, p=0.42) between fire count and per-fire casualties — holds across every threshold and time period tested (see `03_frequency_vs_severity.ipynb`, `04_robustness_checks.ipynb`).
* 청주시상당구 is the most consistent individual case across sensitivity checks: the only region flagged as high-severity in both 2015-19 and 2020-24, with 1.6-2.3x excess casualty risk beyond what its place-type mix predicts (no confidence interval has been computed, so chance is not formally ruled out).
* 21 regions show excess risk ≥1.3x, forming a candidate priority list — though only 청주시상당구 is cross-validated across time periods; treat the rest as exploratory.
* Five Gangwon-province regions appeared among the top 15 exploratory excess-risk candidates — a pattern invisible before an administrative-boundary naming bug (강원도 → 강원특별자치도, 2023) was fixed, because affected regions were being split into two separate entries with independently miscalculated excess-risk values (see `docs/validation_log.md`).
* Seasonality supported by STL decomposition and ACF (lag=12: ρ=0.49; lag=24: ρ=0.46, both significant at α=0.05) — fires peak in March and winter months.

## Data
**Source**: 소방청 (Korea National Fire Agency) 연간화재통계 (Annual Fire
Statistics), 2015–2024, published via [data.go.kr](https://www.data.go.kr).
Incident-level records (~406,000 rows) with date, region, ignition
source, place type, and casualty/property damage figures. Raw files
are not included in this repo; see `docs/data_inventory.md` for
download details.

## Methodology Highlights
* Data-quality bugs found and fixed: two separate region-identification bugs surfaced during this analysis — non-unique district names, and un-normalized administrative renamings — that would have skewed several results if left uncaught. See `docs/validation_log.md` for the full story.
* Leave-one-out baseline: when computing a region's "expected" casualty rate from national place-type statistics, that region's own data is excluded from the baseline to avoid its own extreme values inflating its own benchmark.
* Worst-incident exclusion: per-region statistics are recomputed excluding each region's single most severe incident, to confirm results aren't artifacts of one mass-casualty event (e.g. this ruled out 과천시 and 밀양시 as false positives — see `05_excess_risk_analysis.ipynb`).
* Primary analysis + sensitivity checks: main results use a 300+ cumulative fires threshold (stated a priori); robustness confirmed at 200 and 500 as well.

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
