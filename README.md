# Hungarian Electricity Net Import Analysis & Forecasting
This project presents an end‑to‑end data analysis and forecasting pipeline focused on Hungary’s electricity net import dependence, using hourly system data from the MAVIR VER dataset between 2022 and 2025.
The goal is to understand when and why Hungary relies most on electricity imports, identify critical stress hours, evaluate planning accuracy, and assess how targeted demand‑side and supply‑side interventions could reduce import dependence. The project combines exploratory data analysis (EDA), statistical diagnostics, scenario simulation, and machine‑learning‑based forecasting.

# Project Idea
Annual electricity import ratios often hide the real system risk. While yearly averages may look acceptable, imports are highly concentrated in specific hours, especially during winter evening peaks.
This project takes a time‑resolved (hourly) approach to:

Identify high‑risk import periods
Quantify structural drivers such as load, domestic production, and time patterns
Evaluate planned vs. actual net import accuracy
Simulate realistic import‑reduction scenarios
Forecast short‑term net import using machine learning

The results support data‑driven energy policy and system planning, shifting focus from annual averages to critical hours.

#Dataset
Dataset used: MAVIR VER – Tényleges Nettó Forgalmi Óránkénti Kumulált Adatok
File: VER tényleges Nettó Forgalmi Óránkénti kumulált adatok.xlsx
Sheet: Exportált adatok
Time range: January 2022 – January 2026 (hourly)

# Key Columns

| Column                      | Description                              |
|-----------------------------|------------------------------------------|
| Timestamp                   | Hourly timestamp                         |
| Actual System Load          | Total electricity demand                 |
| Domestic Production         | Electricity generated within Hungary     |
| Actual Import / Export      | Real cross-border electricity flows      |
| Planned Import / Export     | Scheduled cross-border electricity flows |
| Network Losses              | Transmission and distribution losses     |

# Derived Metrics

- Net import (import − export)
- Import share (relative to system load)
- Planning error (actual vs planned)
- Time features (hour, month, season, weekend)

Data Quality Checks
Before analysis, the pipeline performs automated checks:

Removal of placeholder and all‑NaN rows
Timezone‑aware timestamp parsing (DST safe)
Detection of duplicate timestamps (none found)
Missing value diagnostics (none significant)
Verification of hourly continuity (no missing hours)

The final dataset contains 35,064 clean hourly observations.

Exploratory Data Analysis (EDA)
The project produces a comprehensive EDA with over 30 generated visualizations, including:
Time‑Series Analysis

Monthly mean load, domestic production, and net import
Annual net import share (sum‑based)
Cumulative net import over time

Distribution & Extremes

Net import histogram with p95 and p99 thresholds
Identification of top 1% worst import hours
Hour‑of‑day and month distributions of extreme hours
Duration curve (sorted net import hours)

Temporal Patterns

Hour‑of‑day profiles (overall and by season)
Weekday vs weekend profiles
Heatmaps:

Month × hour
Day‑of‑week × hour
Year × month (daily net import)



Planning Accuracy

Planned vs actual net import scatter
Monthly and hourly plan error (MAE)
Distribution of planning errors

Advanced Diagnostics

Rolling z‑score anomaly detection
STL seasonal decomposition of daily net import
Correlation heatmap
Quantile “fan charts” by hour
Boxplots by hour and by month

All figures are saved in chronological order to eda_outputs/.

Import Reduction Scenario Simulator
A scenario module estimates how targeted interventions affect imports:
Example scenario

Winter evenings (17:00–22:00, Nov–Feb)
2% load reduction
Net import assumed to change proportionally

Result:

~0.2% reduction in annual net import
Disproportionately larger reduction during peak stress hours

A sensitivity curve shows how increasing peak‑shaving intensity affects total import reduction.

Driver Analysis (Interpretable Model)
A Ridge regression with time‑series cross‑validation is used to understand which factors drive net import:
Features include:

System load
Domestic production
Hour of day
Day of week
Month
Weekend indicator

Model coefficients are visualized as a proxy for feature importance, providing interpretability rather than prediction accuracy.

Forecasting: Net Import Prediction (XGBoost)
To predict short‑term net import, an XGBoost regression model is trained using:
Features

Lagged net import values (1h, 7h, 14h)
Hour
Day of week
Month
Weekend indicator

Train/Test Split

Time‑based 80% training / 20% testing split

Performance

MAE: ~150
RMSE: ~219

A time‑series plot compares actual vs predicted net import on the test period, showing the model captures daily dynamics and seasonal structure.

Outputs
The pipeline generates:
Visual outputs

33 PNG figures saved in eda_outputs/
Ordered to tell a coherent analytical story

Data exports

hour_profile_summary.csv
top200_net_import_hours.csv
annual_net_import_share.csv

These outputs can be directly used in reports, presentations, or dashboards

How to Run
1. Install dependencies
   pip install pandas numpy matplotlib openpyxl statsmodels scikit-learn xgboost

2. Place dataset in project folder
   VER tényleges Nettó Forgalmi Óránkénti kumulált adatok.xlsx
3. Run analysis
   python analysis_script.py
4. View results

Open eda_outputs/
Start from 01_*.png to follow the EDA narrative

Project Structure
project/
│
├── VER tényleges Nettó Forgalmi Óránkénti kumulált adatok.xlsx
├── analysis_script.py
├── README.md
│
├── eda_outputs/
│   ├── 01_monthly_means_load_prod_netimport.png
│   ├── ...
│   ├── 33_heatmap_year_month_daily_netimport.png
│   ├── hour_profile_summary.csv
│   ├── top200_net_import_hours.csv
│   └── annual_net_import_share.csv

#Technologies Used
Python
Pandas, NumPy
Matplotlib
Statsmodels (STL decomposition)
Scikit‑learn (Ridge, time‑series CV)
XGBoost
OpenPyXL

#Team Members
Balás Bence Balázs
Kupcsik András
Abigail Wanjiru Kamau
