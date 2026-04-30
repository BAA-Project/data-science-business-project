# 🇭🇺 Hungarian Electricity Net Import Analysis & Forecasting

This project presents an end-to-end data analysis and forecasting pipeline focused on Hungary’s electricity net import dependence, using hourly system data from the MAVIR VER dataset (2022–2025).

The goal is to understand **when and why Hungary relies on electricity imports**, identify critical stress hours, evaluate planning accuracy, and assess how targeted demand-side and supply-side interventions can reduce import dependence.

The project combines:
- Exploratory Data Analysis (EDA)
- Statistical diagnostics
- Scenario simulation
- Machine learning-based forecasting

---

## 🎯 Project Idea

Annual electricity import ratios often hide real system risk. While yearly averages may appear acceptable, imports are highly concentrated in specific hours—especially during **winter evening peaks**.

This project uses an hourly, time-resolved approach to:

- Identify high-risk import periods  
- Quantify structural drivers (load, domestic production, time patterns)  
- Evaluate planned vs actual net import accuracy  
- Simulate realistic import-reduction scenarios  
- Forecast short-term net import  

---

## 📂 Dataset

- **Source**: MAVIR VER – Hourly Net System Flow Data  
- **File**: `VER tényleges Nettó Forgalmi Óránkénti kumulált adatok.xlsx`  
- **Sheet**: `Exportált adatok`  
- **Time range**: January 2022 – January 2026 (hourly)  

---

## 📊 Key Columns

| Column                      | Description                              |
|-----------------------------|------------------------------------------|
| Timestamp                   | Hourly timestamp                         |
| Actual System Load          | Total electricity demand                 |
| Domestic Production         | Electricity generated within Hungary     |
| Actual Import / Export      | Real cross-border electricity flows      |
| Planned Import / Export     | Scheduled cross-border electricity flows |
| Network Losses              | Transmission and distribution losses     |

---

## ⚙️ Derived Metrics

- Net import (import − export)  
- Import share (relative to system load)  
- Planning error (actual vs planned)  
- Time features (hour, month, season, weekend)  

---

## 🧹 Data Quality Checks

Before analysis, the pipeline performs:

- Removal of placeholder and all-NaN rows  
- Timezone-aware timestamp parsing (DST-safe)  
- Duplicate timestamp detection (none found)  
- Missing value diagnostics (none significant)  
- Hourly continuity validation (no gaps detected)  

**Final dataset**: 35,064 clean hourly observations  

---

## 📈 Exploratory Data Analysis (EDA)

The project generates 30+ visualizations covering:

### Time Series
- Monthly mean load, production, and net import  
- Annual net import share  
- Cumulative net import  

### Distribution & Extremes
- Net import distribution (p95, p99 thresholds)  
- Top 1% worst import hours  
- Duration curve  

### Temporal Patterns
- Hour-of-day profiles (overall & seasonal)  
- Weekday vs weekend behavior  
- Heatmaps:
  - Month × hour  
  - Day-of-week × hour  
  - Year × month  

### Planning Accuracy
- Planned vs actual comparison  
- Monthly and hourly error (MAE)  
- Error distributions  

### Advanced Diagnostics
- Rolling z-score anomaly detection  
- STL seasonal decomposition  
- Correlation analysis  
- Quantile bands and boxplots  

All figures are saved in `eda_outputs/`.

---

## ⚡ Scenario Analysis

A simulation module evaluates how demand-side interventions affect imports.

### Example Scenario
- Winter evenings (17:00–22:00, Nov–Feb)  
- 2% load reduction  

### Result
- ~0.2% reduction in annual net import  
- Larger impact during peak stress hours  

A sensitivity curve shows how increasing peak reduction affects total imports.

---

## 🧠 Driver Analysis (Interpretable Model)

A Ridge regression model with time-series cross-validation is used to identify key drivers of net import.

**Features:**
- System load  
- Domestic production  
- Hour, day, month  
- Weekend indicator  

Model coefficients provide interpretable insights into system dynamics.

---

## 🤖 Forecasting: Net Import Prediction

An XGBoost model is used for short-term forecasting.

### Features
- Lagged net import (1h, 7h, 14h)  
- Time-based features  

### Performance
- **MAE**: ~150  
- **RMSE**: ~219  

The model captures both daily patterns and seasonal structure.

---

## 📦 Outputs

### Visuals
- 33 PNG figures (`eda_outputs/`)  
- Ordered to follow the analytical narrative  

### Data Exports
- `hour_profile_summary.csv`  
- `top200_net_import_hours.csv`  
- `annual_net_import_share.csv`  

---

`## How to Run

### 1. Install dependencies

    
    pip install pandas numpy matplotlib openpyxl statsmodels scikit-learn xgboost
    

### 2. Place the dataset in the project folder

    
    VER tényleges Nettó Forgalmi Óránkénti kumulált adatok.xlsx
    

### 3. Run the analysis script

    
    python analysis_script.py
    

### 4. View outputs

    
    All figures and CSV files are saved in the eda_outputs/ directory.
    Open PNG files in numeric order (01–33).
    
##
project/
│
├── data.xlsx
├── analysis_script.py
├── README.md
│
├── eda_outputs/
│   ├── 01_*.png
│   ├── ...
│   ├── 33_*.png
│   ├── *.csv

##Technologies Used:
Python
Pandas, NumPy
Matplotlib
Statsmodels (STL)
Scikit-learn
XGBoost
OpenPyXL

##👥 Team Members
Balázs Bence Balázs
Kupcsik András
Abigail Wanjiru Kamau
