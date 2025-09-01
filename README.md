🌍 GOOGLE CARBON FREE DATA CENTER

This project investigates how Google data centers can transition to 24/7 carbon-free energy (CFE) by analyzing electricity generation, grid emissions, tariffs, incentives, and demand profiles in India and the United States. Using real-world datasets from government sources (POSOCO, CEA, EIA, OGD) combined with manually built components, the project creates a unified framework for comparing renewable penetration, electricity prices, and policy landscapes across regions.

The analysis includes building renewable time-series datasets, mapping state-wise tariffs, simulating demand–supply mismatches (latency), and estimating carbon intensity based on grid factors. The findings aim to provide insights into the feasibility of achieving 24/7 carbon-free operations, highlight policy and economic trade-offs, and suggest strategies for future clean energy–driven digital infrastructure.


📌 Project Overview

This project explores how Google data centers can transition to 24/7 carbon-free energy (CFE) by analyzing electricity generation, grid emissions, tariffs, demand, and incentives in India and the United States.

The project simulates data center energy consumption against real-world electricity mixes to evaluate:

How carbon-free grids can support large-scale digital infrastructure.

The trade-offs between renewable penetration, grid emissions, latency, and electricity tariffs.

The role of government incentives and carbon factors in shaping energy choices.

This work is inspired by Google’s commitment to operate its data centers on 100% carbon-free energy, every hour of every day.

🎯 Objectives

Build a renewable energy time-series dataset combining U.S. ISO and India’s POSOCO/CEA data.

Simulate demand profiles for data centers in both countries.

Compare state/region-level electricity tariffs for industrial and domestic sectors.

Estimate carbon intensity (emission factors) from U.S. SEDS + India CEA baseline.

Model latency datasets (renewable availability vs demand fulfillment).

Explore policy incentives for renewable adoption in data center operations.

📊 Datasets Used

Regions

File: State_Region_corrected.csv

Mapping of U.S. states + Indian states to grid regions.

Renewables Timeseries

File: renewables_timeseries.csv (merged from file_02.csv (U.S.) + gen_by_fuel.csv (India) + POSOCO/CEA daily reports)

Hourly data of grid generation mix (Thermal, Hydro, Nuclear, Solar, Wind, etc.).

Prices (Tariffs)

File: pr_US.csv (U.S. tariffs) + India tariffs (manually compiled from State Electricity Regulatory Commissions / OGD portal).

Latency

File: latency.csv (simulated)

Measures mismatch between demand and renewable supply at sub-hourly levels.

Emission Factors (EF)

File: complete_seda.csv (U.S. data from EIA SEDS) + CEA Baseline Database (India).

Incentives

File: incentives.csv (manually created)

Captures policy incentives (Renewable Purchase Obligations, feed-in tariffs, tax benefits).

Demand Profiles

File: demand_profiles.csv (combined India_monthly_data.csv + U.S. ISO demand).

Hourly and monthly demand curves for comparison with renewable availability.

⚙️ Methodology

Data Collection & Cleaning

Extracted raw datasets from POSOCO, CEA, EIA, Kaggle, and OGD portal.

Converted multiple sources (CSV, Excel, PDF) into unified .csv format.

Merging Renewable Time-Series

Combined India and U.S. datasets into a single hourly renewable mix dataset.

Tariff & Incentives Mapping

Integrated U.S. tariff datasets with Indian state-wise tariff reports.

Added incentives data manually (policies, subsidies, tax credits).

Carbon Intensity Modeling

Used complete_seda.csv + CEA Baseline factors to calculate CO₂/MWh.

Demand–Supply Matching

Mapped data center demand profiles against renewable availability.

Simulated latency (renewable shortage time slots).

Visualization & Forecasting

Built interactive dashboards (Plotly) to visualize energy mix, tariffs, and demand-supply gaps.

Used Linear Regression (scikit-learn) to forecast renewable penetration trends.

🚀 Key Insights Expected

How renewable availability differs between U.S. and India.

Whether industrial tariffs make clean energy adoption feasible.

The role of government incentives in achieving carbon neutrality.

Potential mismatch (latency) between data center demand and renewable generation.

Policy recommendations for improving carbon-free grid integration.

🛠️ Tech Stack

Python (Pandas, NumPy, Scikit-learn)

Plotly / Matplotlib (visualizations)

Jupyter Notebooks (analysis)

Pathlib (project structuring)

📂 Project Structure
├── data/
│   ├── State_Region_corrected.csv
│   ├── renewables_timeseries.csv
│   ├── pr_US.csv
│   ├── latency.csv
│   ├── complete_seda.csv
│   ├── incentives.csv
│   ├── demand_profiles.csv
│   ├── India_monthly_data.csv
│   ├── gen_by_fuel.csv
│   ├── file_02.csv
│   └── Daily_Power_Gen_Source_march_23.csv
│
├── notebooks/
│   ├── data_cleaning.ipynb
│   ├── renewables_analysis.ipynb
│   ├── demand_forecasting.ipynb
│   └── carbon_intensity_model.ipynb
│
├── scripts/
│   ├── preprocess.py
│   ├── merge_datasets.py
│   └── simulate_latency.py
│
├── README.md
└── requirements.txt

📈 Future Work

Automate data scraping from POSOCO/CEA PDFs.

Integrate real-time carbon intensity API (e.g., WattTime, ElectricityMap).

Extend scope to Europe / Southeast Asia.

Optimize data center location & scheduling strategies for CFE.
