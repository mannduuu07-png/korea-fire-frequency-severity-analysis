# Validation Log

## Bug 1: District names are not unique identifiers

An early version of the frequency-vs-severity analysis compared
regions across time periods using district name alone (e.g. '중구').
This is incorrect: 중구, 남구, 동구, 서구 and similar names are shared
by multiple cities nationwide — 중구 alone exists in 6 different
cities. Comparing by name caused two different cities to be counted
as "the same persistent region" in the 2015-19 vs 2020-24 period-split
check. Fixed by using (시도, 시군구) tuples throughout instead of
district name alone. See 04_robustness_checks.ipynb.

## Bug 2: Un-normalized administrative renamings (2015-2024)

Several administrative boundary/name changes occurred during the
analysis period, causing the same region to appear under two
different labels:

- 강원도 → 강원특별자치도 (2023-06-11, simple rename)
- 전라북도 → 전북특별자치도 (2024-01-18, simple rename)
- 경상북도 군위군 → 대구광역시 (2023-07-01, province reassignment)
- 경기도 부천시: 원미구/소사구/오정구 abolished 2016-07-04, reinstated
  2024-01-01 — for 2016-2023, records used unified "부천시" with no
  sub-district; for other years, sub-district names were used

Before this fix, "강원도 원주시" (2,263 records) and "강원특별자치도
원주시" (955 records) were treated as two separate regions, each
producing its own (incorrect) excess-risk ratio and ranking. Once
merged, 원주시's 10-year cumulative fire count was correctly
recalculated at 3,218 records, revealing a real pattern that had
been obscured by the split.

Fixed by normalizing all four cases to a single consistent
(시도, 시군구) identity in 01_data_cleaning.ipynb before any
downstream aggregation. All notebooks were re-run after this fix;
the core finding (청주시상당구 as the sole region persisting across
both sub-periods) was unaffected, since that region has no naming-
history issues. Correlation coefficients across all notebooks
weakened slightly after the fix (e.g. primary Spearman ρ from 0.100
to 0.051), consistent with the earlier duplicated entries having
introduced noise.

## Known remaining limitation

Two duplicate rows were found in the raw concatenated dataset
(~0.0005% of ~406,000 rows) and were not removed, as their impact is
negligible at this scale.
