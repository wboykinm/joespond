# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a data analysis project predicting **ice-out dates for Joe's Pond, Vermont** (44.409°N, 72.200°W) using climate data. Ice-out is the annual moment when winter ice breaks up on the pond — a local tradition with recorded dates since 1988.

The notebook uses PRISM gridded climate data to build OLS regression models that predict ice-out day-of-year (DOY), time-of-day, and a combined fractional-DOY timestamp from winter/spring temperature and precipitation metrics.

## Environment Setup

```bash
# Python 3.12, managed with uv
uv sync                     # install dependencies from pyproject.toml / uv.lock
source .venv/bin/activate   # activate virtualenv
jupyter lab                 # launch notebook server
```

Key dependencies: pandas, numpy, scipy, matplotlib, seaborn, beautifulsoup4, requests.

## Notebooks

### `joes_pond_prism_analysis.ipynb` (primary)
PRISM-based pipeline: loads cached ice-out records from `data/joes_pond_iceout.csv`, merges with PRISM 800m gridded daily time series (exported from PRISM Data Explorer), computes fixed-window (Jan 1 → mid-March) climate features, runs correlation analysis, and builds LOOCV-validated OLS models. Predicts three targets: DOY, minutes_since_midnight, and fractional_doy (combined timestamp).

### `archive/` (prior work)
Contains the earlier NOAA station-based notebook (`dead_end_joes_pond_iceout_analysis.ipynb`) which used multi-station composites from NCEI daily data. Kept for reference but no longer the active analysis.

## Data Layout

- `data/joes_pond_iceout.csv` — cached ice-out records (year, date, time)
- `data/prism_timeseries/` — PRISM gridded daily exports (manually downloaded from PRISM Data Explorer); see `README_PRISM_DOWNLOAD.md` inside for export instructions
- `data/ghcnd-stations.txt` — GHCN-D station metadata file (used by archived notebook)
- `data/ncei_daily/` — NOAA NCEI station CSVs (used by archived notebook)

## Key Analysis Patterns

- **Climate feature window**: fixed window (Jan 1 → mid-March), parameterized via `FIXED_WINDOW_START` and `FIXED_WINDOW_END` in the setup cell
- **Derived metrics**: mean/std temperature, freezing/thaw degree-days, precipitation total
- **Model selection**: exhaustive search over feature combinations (up to 3 features), ranked by LOOCV RMSE
- **Prediction targets**: DOY, minutes_since_midnight, and fractional_doy (combined date+time)
- **Weak time-of-day model handling**: when the standalone time model has negative CV R², the notebook falls back to the historical mean and prints a warning
- `PREDICTION_YEAR = 2026` is the current forward-prediction target

## Updating PRISM Data

PRISM data must be manually exported from the PRISM Data Explorer (no public API). To extend the training window, export a new CSV with a later end date, place it in `data/prism_timeseries/`, and re-run the notebook. The loader merges overlapping files automatically.
