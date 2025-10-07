# San Francisco Rent Trends — Final Technical & Nontechnical Report

**Author:** Luke Drury  
**Course:** UC Berkeley Exec Ed ML/AI — Capstone  
**Repository:** https://github.com/luke-drury/Luke-Drury-Initial-Report-and-Exploratory-Data-Analysis-EDA-  
**Last updated:** 2025-10-07

---

## Executive Summary (Nontechnical)

**Business question.** What explains variation in rent levels across San Francisco ZIP codes and how are rents likely to move over the next year?

**Key findings (high level):**
- _[Replace after running notebook]_ Median income and transit access are positively associated with higher rents; crime is weakly/negatively associated after controls.
- Citywide ZORI has _[seasonality + trend]_ with a forecasted **_[X%]_** change over the next 12 months (baseline seasonal model).
- ZIPs cluster into **_[k]_** affordability/amenity segments with distinct geographic patterns.

**Recommendations (for a nontechnical audience):**
1. Track the affordability mix by ZIP cluster to guide policy or product targeting.
2. For short‑term planning, use the baseline 12‑month forecast; revisit quarterly as new data arrives.
3. Prioritize additional data collection (e.g., employment centers, school quality) before using results for causal decisions.

---

## Project Organization (Technical)

```
sf-rent-trends/
├─ data/
│  ├─ zori_zip.csv
│  ├─ sf_zip_codes.geojson
│  ├─ muni_stops.csv
│  ├─ sfpd_incidents.csv
│  ├─ airbnb_sf_listings.csv
│  └─ (auto-fetched) income_zip via Census API
├─ notebooks/
│  └─ 01_eda_modeling.ipynb
├─ README.md
└─ environment.txt
```

**Primary notebook:** [`Capstone Main Code.ipynb`](./Capstone%20Main%20Code.ipynb) (exported copy should also be in `notebooks/01_eda_modeling.ipynb`).

---

## Data Sources

- **Zillow Observed Rent Index (ZORI)** — ZIP‑level typical asking rent (CSV).  
- **SF ZIP Polygons** — GIS boundaries to support choropleths and spatial joins.  
- **SFMTA (Muni) Stops** — proxy for transit access.  
- **SFPD Incidents** — proxy for safety (12‑month window).  
- **Inside Airbnb (SF)** — proxy for tourism pressure (listings density).  
- **Census/ACS (ZCTA)** — median household income via API (`S1901_C01_012E` or `B19013_001E`).

Exact download URLs and transformations are documented within the notebook’s **Data Loading** cells.

---

## Methods

### Cleaning & Feature Engineering
- Standardize 5‑digit ZIP keys and CRS (`EPSG:4326`).
- Parse dates; constrain SFPD incidents to most recent 12 months relative to latest ZORI.
- ZIP‑level features created:
  - `rent_index` (latest ZORI); `log_rent`
  - `median_income`; `log_income`
  - `transit_density` (stops per 1k residents)
  - `crime_per_1k`
  - `airbnb_density` (per km²)

### Exploratory Analysis & Visualizations
- Histograms and boxplots for continuous features with readable labels.
- Scatterplots (e.g., `log_rent` vs `log_income`) with trend lines and confidence bands.
- Choropleth maps of rent and key drivers by ZIP.
- Correlation heatmap with masking of upper triangle.

### Modeling
1. **Cross‑sectional OLS (ZIP‑level)**  
   - `log_rent ~ log_income + transit_density + crime_per_1k + airbnb_density`  
   - Robust (HC3) standard errors; report RMSE & R².  
   - Diagnostics: residual QQ‑plot and heteroskedasticity check.

2. **Time‑series Forecast (citywide monthly ZORI)**  
   - Aggregate ZIP ZORI to city median.  
   - Train SARIMAX (1,1,1)x(1,1,1)\_12 baseline.  
   - Hold‑out last 12 months; report RMSE & MAPE.  
   - Plot forecast vs truth with 80/95% intervals.

3. **Clustering (ZIP‑level)**  
   - Standardize features; try k=3..5 (K‑means).  
   - Pick k by silhouette score; visualize 2D PCA projection and map by cluster.

---

## Results (Technical)

> Replace the placeholders after you run the notebook end‑to‑end.

- **OLS:** RMSE = `___`, R² = `___`; signs: `log_income (+)`, `transit_density (+)`, `crime_per_1k (−/ns)`, `airbnb_density (±)`.
- **Forecast:** RMSE\_test = `___`, MAPE\_test = `___`; error within acceptable bounds for baseline seasonal model.
- **Clustering:** `k = ___`, silhouette = `___`; geographic separation aligns with amenity gradients.

All figures include descriptive titles, legible axes, and captions that interpret the plot in plain English.

---

## How to Reproduce

1. `python -m venv .venv && source .venv/bin/activate`
2. `pip install -r environment.txt`  
3. Place inputs in `./data/` using the names above.  
4. (Optional) `export CENSUS_API_KEY=YOUR_KEY` for ACS income.  
5. Open and run `notebooks/01_eda_modeling.ipynb` (or `Capstone Main Code.ipynb`).

---

## Assumptions & Limitations

- ZCTA ≈ ZIP; minor boundary mismatches possible.  
- Proxies are non‑causal and noisy; treat results as descriptive.  
- Baseline SARIMAX may underfit regime shifts; revisit with exogenous features.

---

## Ethical Considerations

Neighborhood metrics can influence perceptions; avoid stigmatizing communities. Clearly label all outputs as **descriptive** insights rather than causal claims.

---

## References

- Zillow Research: ZORI methodology.  
- DataSF: ZIP polygons, SFMTA stops, SFPD incidents.  
- Inside Airbnb (SF).  
- U.S. Census/ACS API documentation for `S1901` / `B19013`.

