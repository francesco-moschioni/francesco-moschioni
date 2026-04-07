# Francesco — Economics × Data × Code

> *Master's student in Economics @ Bocconi University.*
> *I build structural econometric models and the data pipelines that feed them.*

---

## 🎓 Thesis — NYC Urban Mobility Demand Estimation

My master's thesis applies a **Mixed Logit / BLP structural demand model** to the New York City urban mobility market, estimating modal substitution patterns across Yellow Taxi, Green Taxi, HVFHV (Uber/Lyft/Via), Subway, Bus, and Citi Bike.

**Research question**: How do surge pricing algorithms and transit disruptions reshape consumer substitution across modes in real time?

### Methodology

The estimation framework is progressive and defensible at each step:

```
Descriptive Analysis → Simple Logit → Nested Logit → BLP Mixed Logit
```

- **Market definition**: `(Origin TLC zone × Destination TLC zone × Hour × Date)` — ~263 zones, 2021–2025
- **Berry inversion**: linearises market shares into mean utilities → enables IV/GMM identification
- **Endogeneity**: HVFHV surge pricing instrumented via Hausman-type IVs + BLP differentiation instruments + GTFS scheduled frequency
- **Random coefficients**: heterogeneous price and travel-time sensitivities drawn from Census income distributions (ACS 2022 at tract level)
- **Post-estimation extension**: NLP-based Service Disruption Index from MTA service alerts

**Reference**: Guerrero, Guevara, Cherchi & Ortúzar (2021), *Transportation Research Part B* — structural modal demand in Valparaíso.

---

### Data Pipeline

The project processes ~20 GB of raw data across six heterogeneous public sources, producing a unified market-level panel for estimation.

| Source | Volume | Pipeline stage |
|---|---|---|
| NYC TLC Trip Records (Yellow/Green/HVFHV/FHV) | ~1.1B trips | Clean → zone-hour aggregation |
| MTA Subway Hourly Entries | 43.7M rows | Clean → station-level panel |
| MTA Subway Origin-Destination Estimates | 126.3M rows | Weight matrix for flow estimation |
| MTA Subway Estimated Flows (derived) | 1.36B rows | Station → TLC zone spatial join |
| Citi Bike Trip Data | 221.7M rows | Clean → TLC zone spatial join |
| NOAA ISD Global Hourly Weather | 18 files × 3 stations | Download → cleaning pending |
| U.S. Census ACS 2022 (5-year) + TIGER/Line | 2,327 tracts | Crosswalk → TLC zone demographics |

**Key engineering challenges solved**:
- Row-group-by-row-group PyArrow I/O throughout — never loading full files into RAM
- DuckDB temp directory routed to D: to avoid C: exhaustion on large aggregations
- Explicit `pa.schema` on all Parquet writers — prevents type inference failures on sparse early row groups
- Resume-safe shard patterns on all CSV → Parquet pipelines
- PyArrow filter-before-pandas pattern on HVFHV files (~18–21M rows/month) to prevent OOM

---

### Difference-in-Differences Extension

A companion **reduced-form analysis** exploits the NYC Congestion Pricing surcharge (effective January 9, 2025) as a natural experiment.

- **Treatment**: TLC zones inside Manhattan below 60th Street
- **Control**: Manhattan above 60th Street (main); outer boroughs (robustness)
- **Design**: event study with monthly indicators, October 2024 as omitted reference
- **Outcome**: `ln(trip_count)` at zone × month × mode level
- **Implementation**: Stata (`reghdfe`, `esttab`, `coefplot`) via Python-built panel (`build_did_panel.py` → DuckDB → `did_panel.csv`)

---

### Tech Stack

**Python** — PyArrow, DuckDB, GeoPandas, pandas, NetworkX, PyBLP (planned)

**LaTeX** — TeX Live + VS Code + LaTeX Workshop; Zotero + Better BibTeX

**Data sources** — NYC TLC, MTA Open Data (Socrata), Citi Bike S3, NOAA ISD, U.S. Census Bureau API, GTFS

---

## 📫 Contact

[![LinkedIn](https://img.shields.io/badge/LinkedIn-blue?style=flat&logo=linkedin)](https://www.linkedin.com/in/francesco-moschioni/)
[![Email](https://img.shields.io/badge/Email-grey?style=flat&logo=gmail)](mailto:framoschioni@gmail.com)

---

*All raw data is public. Code available on request — thesis submission pending.*
