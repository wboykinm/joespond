# PRISM Download Checklist (Joe's Pond)

Target folder for this notebook:
- `data/prism_timeseries/`

Coordinates:
- Latitude: `44.40887152650143`
- Longitude: `-72.2000267441201`

Date range for daily exports:
- Start: `1981-01-01`
- End: `2026-03-13`

Required variables:
- `tmean` (daily mean temperature)
- `ppt` (daily precipitation)

Optional but useful:
- `tmin`
- `tmax`

Recommended filename patterns:
- `prism_tmean_daily_1981_2026.csv`
- `prism_ppt_daily_1981_2026.csv`
- `prism_tmin_daily_1981_2026.csv` (optional)
- `prism_tmax_daily_1981_2026.csv` (optional)

After downloading:
1. Put CSV files in `data/prism_timeseries/`.
2. Re-run notebook `joes_pond_prism_analysis.ipynb` from section **2** onward.

Notes:
- The loader infers variable names from filenames/columns, so names can vary.
- If your export uses different headers, keep the date column and one numeric value column.
