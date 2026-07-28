# 🇮🇳 Google 24/7 Carbon-Free Data Center — India Site Selection & ROI

This project investigates how Google could transition an Indian data center to
24/7 carbon-free energy (CFE) by analyzing state-level renewable availability,
electricity tariffs, grid connectivity/latency, and policy incentives —
combining them into a single composite site-selection score and a
supporting ROI model. Scoped to **India only**.

## Project Overview

The project ranks Indian states as candidate Google data center locations by
balancing three pillars — **renewable energy availability**, **electricity
cost**, and **latency to Google's existing Cloud regions in India** — into one
weighted composite score, then layers a business/ROI model on top (estimated
CO₂ avoided, energy cost savings, incentive savings, and ROI % against an
assumed CAPEX) for a configurable data center size.

This work is inspired by Google's public commitment to operate its data
centers on 100% carbon-free energy, every hour of every day, by 2030.

##  Objectives

- Rank Indian states by renewable share, industrial electricity tariff, and
  distance/connectivity to Google's existing Cloud India regions.
- Forecast state-level renewable share out to 2030 using a simple
  growth-rate extrapolation.
- Estimate the business case (CO₂ avoided, cost savings, ROI %) for a
  hypothetical data center at each candidate location.
- Be transparent about which numbers are real, government-sourced data vs.
  curated estimates — see **Data Sourcing & Honesty** below.

##  Datasets Used

| File | What it contains | Status |
|---|---|---|
| `state_region.csv` | State → POSOCO grid region, lat/lon | Grid region from source repo; lat/lon added (real coordinates) |
| `renewable_energy.csv` | State → 2024 renewable share % + assumed annual growth rate to 2030 | Curated estimate (see below) |
| `electricity_cost.csv` | State → industrial/HT electricity tariff (₹/kWh) | Curated estimate (see below) |
| `connectivity.csv` | State → near a major submarine cable landing station? | Real facts (known landing cities), simplified proximity cutoff |
| `google_cloud_regions.csv` | Existing Google Cloud India regions (Mumbai `asia-south1`, Delhi NCR `asia-south2`) with lat/lon | Real, verifiable (Google Cloud's published region list) |
| `incentives.csv` | State → renewable policy incentive (₹/kWh) | Curated estimate, least confident of the set |
| `RAW_SOURCE_State_Region_corrected.csv` | Original, untouched file from the source GitHub repo | Real, unmodified |
| `RAW_SOURCE_India_monthly_data.csv` | Original, untouched POSOCO/CEA national monthly generation-mix file from the source repo | Real, unmodified |

**Note on the earlier project vision:** the original scope described a much
larger dataset — hourly `renewables_timeseries.csv`, `demand_profiles.csv`,
`complete_seda.csv` emission factors, `latency.csv` simulations, US files
(`pr_US.csv`), etc. Most of those files either don't exist in the source repo
or don't exist as ready-to-use, state-level Indian data anywhere publicly. To
keep this project honest and runnable, it was rebuilt around **only the data
that's real or reasonably estimable**, listed in the table above. See
**Data Sourcing & Honesty** for the full breakdown of what's real vs.
estimated.

##  Methodology

1. **Load & merge** — all state-level CSVs are merged on `state` into one
   working table.
2. **Forecast renewable share** — linear extrapolation from 2024 share using
   a per-state growth rate, capped at 95%, to any target year up to 2030.
3. **Latency scoring** — great-circle distance from each state to the
   nearest existing Google Cloud India region, with a 30% effective-distance
   discount for states near a submarine cable landing station.
4. **Normalize & score** — renewable share, tariff (inverted), and distance
   (inverted) are each min-max normalized 0–1, then combined into a
   composite score using configurable weights:
   `Score = w_renewable × RE_score + w_cost × Cost_score + w_latency × Latency_score`
   Default (Balanced) weights: 0.5 / 0.3 / 0.2. Presets also include
   Greenest, Cheapest, and Lowest-latency.
5. **ROI model** — for an assumed data center IT load (MW):
   `ROI % = (EnergySavings + CarbonSavings + IncentiveSavings) / CAPEX × 100`
   - Energy savings: vs. the average tariff across all candidate states
   - Carbon savings: emissions avoided (renewable share × annual energy ×
     India's CEA grid emission factor, 0.716 tCO₂/MWh) × an assumed carbon price
   - Incentive savings: state incentive rate × annual energy
   - CAPEX: ~₹41.5 crore/MW (hyperscale data center rule of thumb, ~$5M/MW)

##  Key Insights Expected

- Which Indian states offer the best balance of cheap, clean, well-connected
  power for a hyperscale data center — today and under a 2030 outlook.
- How much of that ranking is driven by renewables vs. cost vs. connectivity,
  and how it shifts under different priorities (Greenest vs. Cheapest).
- A rough, order-of-magnitude ROI comparison across states for a given
  facility size.

##  Tech Stack

- Python (Pandas, NumPy)
- Matplotlib (charts, inline in the notebook)
- Jupyter Notebook (all analysis, single notebook)

##  Project Structure

Flat — everything in one folder, no subfolders. Put the notebook and all
CSVs in the same directory and run.

```
├── india_dc_analysis.ipynb
├── state_region.csv
├── renewable_energy.csv
├── electricity_cost.csv
├── connectivity.csv
├── google_cloud_regions.csv
├── incentives.csv
├── RAW_SOURCE_State_Region_corrected.csv
├── RAW_SOURCE_India_monthly_data.csv
├── requirements.txt
└── README.md
```

##  How to Run

```bash
pip install pandas numpy matplotlib jupyter
jupyter notebook india_dc_analysis.ipynb
```

Then: `Kernel → Restart Kernel and Run All Cells`. Edit the `YEAR`,
`SCENARIO`, and `MW_CAPACITY` variables in the "CHANGE ME" cell and re-run
from there down to explore different scenarios.

##  Data Sourcing & Honesty

Be upfront about this if you present or publish the project:

- **Real, verifiable:** `google_cloud_regions.csv` (Google's actual published
  regions), the grid-region mapping in `state_region.csv`, the submarine
  cable landing cities in `connectivity.csv`, and both `RAW_SOURCE_*.csv`
  files (untouched originals from the source GitHub repo).
- **Curated estimates, not pulled from a source table:** `renewable_energy.csv`,
  `electricity_cost.csv`, and `incentives.csv`. No single public dataset
  exists with Indian state-level renewable share %, industrial tariffs, and
  incentive rates all together — these three files were built from general,
  directionally-realistic knowledge of India's energy landscape (e.g.
  Gujarat/Rajasthan/Karnataka lead on renewable capacity; Himachal/J&K are
  hydro-heavy), not verified against CEA/MNRE/SERC source documents
  line-by-line.
- **For production or academic use:** replace those three files with actual
  figures from CEA annual reports, MNRE state-wise renewable capacity data,
  and individual State Electricity Regulatory Commission (SERC) tariff
  orders. The code doesn't care where the numbers come from as long as the
  column names match.

##  Future Work

- Replace the three estimated datasets with verified CEA/MNRE/SERC figures.
- Add hourly time-matching (true 24/7 CFE, not just annual renewable share).
- Automate data collection from POSOCO/CEA public reports.
- Integrate a real-time carbon intensity API (e.g. WattTime, ElectricityMap)
  once available for Indian grid regions.
- Extend the model to sub-state (district/industrial-park) granularity.

