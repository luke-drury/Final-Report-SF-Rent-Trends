# San Francisco Rent Trends — Final Report
**Author:** Luke Drury  
**Course:** UC Berkeley Exec Ed ML/AI — Capstone  
**Repository:** https://github.com/luke-drury/Final-Report-SF-Rent-Trends  
**Last updated:** 2025-10-07

---

## 1. Executive Summary
**Business question.** What explains variation in rent levels across San Francisco ZIP codes and how are rents likely to move over the next year?

**Key findings (high level):**
- Median income and transit access are positively associated with higher rents; crime shows a weaker/negative association after controls.
- Citywide ZORI exhibits clear seasonality; the 12‑month baseline forecast achieved **RMSE=22.16** and **MAPE=1.02%** on the hold‑out.
- ZIPs cluster into **k=3** affordability/amenity segments (silhouette **0.23**).

**Recommendations:**
- Track affordability by ZIP cluster; refresh forecasts quarterly.
- Collect additional features (employment centers, school quality) before causal claims.

---

## 2. Data Sources
- Zillow Observed Rent Index (ZORI) — ZIP‑level typical asking rent (CSV).
- SF ZIP Polygons — GEOJSON boundaries for choropleths and joins.
- SFMTA (Muni) Stops — proxy for transit access.
- SFPD Incidents — proxy for safety (last 12 months).
- Inside Airbnb (SF) — tourism pressure (listings density).
- Census/ACS (ZCTA) — median household income via API or prepared CSV.

---

## 3. Methods
- **OLS (cross‑sectional):** RMSE=0.072, R²=0.494 (robust SEs).  
- **Forecast (SARIMAX):** RMSE=22.16, MAPE=1.02%.  
- **Clustering:** k=3, silhouette=0.23.

---

## 4. Project Structure
```
sf-rent-trends-final/
- data/
  - zori_zip.csv
  - sf_zip_codes.geojson
  - muni_stops.csv
  - airbnb_sf_listings.csv
  - (optional) sfpd_incidents.csv
  - (optional) income_zip.csv
- notebooks/
  - SF_Rent_Trends_Final.ipynb
- environment.txt
- README_FINAL.md
```
---

## 5. How to Reproduce
1. `pip install -r environment.txt`
2. Put inputs in `./data/` exactly as named above.
3. Run **notebooks/SF_Rent_Trends_Final.ipynb**.
4. This notebook writes **README_FINAL.md** with your metrics.
