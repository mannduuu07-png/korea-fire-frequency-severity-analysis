# Data Dictionary

Column definitions for 소방청 연간화재통계 (2015-2024), as used across
all notebooks in this repository.

| Column | Description |
|---|---|
| 화재발생년월일 | Fire occurrence date (and time, where available) |
| 시도 | Province/metropolitan city (normalized — see `validation_log.md`) |
| 시군구 | City/county/district (normalized — see `validation_log.md`) |
| 화재유형 | Fire type (건축·구조물 / 자동차·철도차량 / 위험물·가스제조소등 / 선박·항공기 / 임야 / 기타) |
| 발화열원대분류 / 발화열원소분류 | Ignition heat source, major/minor category (e.g. 담뱃불,라이터불 / 화학적 발화열) |
| 발화요인대분류 / 발화요인소분류 | Ignition cause, major/minor category |
| 최초착화물대분류 / 최초착화물소분류 | First material ignited |
| 인명피해(명)소계 | Total casualties (사망 + 부상) |
| 사망 | Deaths |
| 부상 | Injuries |
| 재산피해소계 | Total property damage, **unit: thousand KRW** (nominal, not inflation-adjusted) |
| 장소대분류 / 장소중분류 / 장소소분류 | Location type, major/mid/minor category (e.g. 주거 / 산업시설 / 생활서비스) |

**Derived columns** (created in `01_data_cleaning.ipynb`):
- `연도`, `월` — extracted from 화재발생년월일

**Derived metrics** (created in analysis notebooks):
- `casualties_per_100_excl` — casualties per 100 fires, excluding each region's single worst incident (see `03_frequency_vs_severity.ipynb`)
- `excess_risk_ratio` — actual casualty rate ÷ expected casualty rate from place-type mix (see `05_excess_risk_analysis.ipynb`)
