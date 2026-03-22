# Joe's Pond Ice-Out Prediction

Predicting ice-out dates for [Joe's Pond](https://en.wikipedia.org/wiki/Joe%27s_Pond), Vermont (44.409°N, 72.200°W) using winter climate data and OLS regression.

Ice-out is the annual moment when winter ice breaks up on the pond — a local tradition tracked by the community since 1988. This project uses PRISM gridded climate data to build leave-one-out cross-validated models that predict when ice-out will occur each spring.

## Quick Start

This project uses Python 3.12, managed with [uv](https://docs.astral.sh/uv/).

```bash
uv sync
source .venv/bin/activate
jupyter lab
```

Then open `joes_pond_prism_analysis.ipynb` and run all cells.

## How It Works

The notebook builds OLS regression models that predict ice-out timing from winter/spring climate features computed over a fixed window (January 1 through mid-March). Climate features include mean temperature, temperature variability, freezing/thaw degree-days, and precipitation totals.

Three prediction targets are modeled:
- **Day of year (DOY)** — which date ice-out occurs
- **Time of day** — what time ice-out occurs (weak predictability from climate data alone)
- **Fractional DOY** — a combined date+time timestamp predicted jointly

The model searches exhaustively over feature combinations (up to 3 features) and ranks by LOOCV RMSE.

## Data

- **Ice-out records** are scraped from the web and cached in `data/joes_pond_iceout.csv`
- **Climate data** comes from [PRISM](https://prism.oregonstate.edu/) 800m gridded daily time series, exported from the PRISM Data Explorer and stored in `data/prism_timeseries/`. See `data/prism_timeseries/README_PRISM_DOWNLOAD.md` for export instructions.
