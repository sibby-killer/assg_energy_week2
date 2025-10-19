# assg_energy_week2
Predict daily solar energy potential (kWh/m²/day) from routine weather variables to support solar planning and grid operations. This advances SDG 7 by enabling better forecasting for clean energy
"""# SDG 7: Predicting Daily Solar Energy Potential (NASA POWER, Supervised Regression) 🌞⚡

Forecast daily solar irradiance (kWh/m²/day) from weather variables to support renewable energy planning and grid operations. This project advances UN SDG 7: Affordable and Clean Energy by improving solar resource forecasting.

## Quick links
- Notebook: `notebooks/01_solar_potential_regression.ipynb`
- Script: `src/solar_regression.py`
- Assets (plots + metrics): `assets/`
- Open in Colab (update USERNAME/REPO after pushing):  
  https://colab.research.google.com/github/USERNAME/REPO/blob/main/notebooks/01_solar_potential_regression.ipynb

## Overview
- Task: Supervised regression to predict `ALLSKY_SFC_SW_DWN` (daily solar irradiance).
- Why it matters: Better forecasts help size solar installations, plan storage, and manage grids—key to scaling clean energy.

## Results
- Gradient Boosting Regressor:
  - MAE: {gbr['MAE']} kWh/m²/day
  - RMSE: {gbr['RMSE']} kWh/m²/day
  - R²: {gbr['R2']}
- Linear Regression:
  - MAE: {lr['MAE']} kWh/m²/day
  - RMSE: {lr['RMSE']} kWh/m²/day
  - R²: {lr['R2']}

## Visuals
- EDA: Daily irradiance over time  
  ![EDA Irradiance Timeseries](assets/eda_irradiance_timeseries.png)
- 2023 Actual vs Predicted (test set)  
  ![Timeseries Actual vs Predicted](assets/timeseries_actual_vs_pred.png)
- Predicted vs Actual (GBR)  
  ![Scatter Pred vs Actual (GBR)](assets/scatter_pred_vs_actual_gbr.png)
- Residuals (GBR)  
  ![Residuals Histogram (GBR)](assets/residuals_hist_gbr.png)
- Feature Importance (GBR)  
  ![Feature Importance (GBR)](assets/feature_importance_gbr.png)

## Dataset
- Source: NASA POWER Project (daily time series; public and free)
- Variables used:
  - Target: `ALLSKY_SFC_SW_DWN` (kWh/m²/day)
  - Features: `T2M` (°C), `RH2M` (%), `WS2M` (m/s), `PRECTOTCORR` (mm/day)
- Example API (Nairobi; 2018–2023):  
  https://power.larc.nasa.gov/api/temporal/daily/point?parameters=ALLSKY_SFC_SW_DWN,T2M,RH2M,WS2M,PRECTOTCORR&start=20180101&end=20231231&latitude=-1.2921&longitude=36.8219&community=RE&format=CSV&header=true
- Note: The notebook downloads data automatically. Change LAT/LON to use a different city.

## Method
- Problem framing: Regression
- Split: Time-based
  - Train: 2018–2022
  - Test: 2023
- Features:
  - Raw: `T2M`, `RH2M`, `WS2M`, `PRECTOTCORR`
  - Engineered: `doy`, `sin_doy`, `cos_doy` (seasonality), `lag1` (previous day irradiance)
- Models:
  - Baseline: Linear Regression (with StandardScaler)
  - Main: GradientBoostingRegressor
- Metrics:
  - Primary: MAE
  - Secondary: RMSE, R²
- Visualizations: Timeseries (actual vs predicted), scatter (pred vs actual), residuals histogram, feature importances

## How to run

### Option A — Google Colab (recommended)
1) Open the notebook in Colab (link above).
2) Run all cells. It will:
   - Download NASA data
   - Train/evaluate models
   - Save plots and metrics to `assets/`
3) File → Save a copy in GitHub (commit the notebook).
4) Download `assets/*.png` + `assets/metrics.txt` from the Colab Files panel and upload them to your repo’s `assets/` folder.

### Option B — Local (Jupyter or Python)
- Setup:
  - Python 3.9+ recommended
  - `pip install -r requirements.txt`
- Run the notebook:
  - `jupyter notebook` → open `notebooks/01_solar_potential_regression.ipynb` → Run All
- Or run the script:
  - `python -c "from src.solar_regression import main; main(lat=-1.2921, lon=36.8219, out_stem='nairobi_power_2018_2023')"`

## Repo structure
- `notebooks/01_solar_potential_regression.ipynb`
- `src/solar_regression.py`
- `assets/`
  - `eda_irradiance_timeseries.png`
  - `timeseries_actual_vs_pred.png`
  - `scatter_pred_vs_actual_gbr.png`
  - `residuals_hist_gbr.png`
  - `feature_importance_gbr.png`
  - `metrics.txt`
- `requirements.txt`
- `README.md`

## Ethics & limitations
- Location-specific: A model trained for one site won’t generalize elsewhere without retraining.
- Data quality: NASA POWER blends satellite/model estimates; missing or biased periods are possible.
- Climate variability: Shifts or extremes can reduce accuracy; retrain periodically.
- Decision support only: Use results to inform planning, not real-time operational control without validation.

## SDG impact
Accurate solar forecasts help:
- Plan capacity and storage sizing
- Improve grid reliability
- Reduce fossil backup needs  
This supports SDG 7: Affordable and Clean Energy.

## How to customize city
- In the notebook or `src/solar_regression.py`, change:
  - `LAT`, `LON` to your city’s coordinates
  - Optionally adjust `start`/`end` in `power_url`
- Re-run to regenerate data, metrics, and plots.

## Assignment deliverables mapping
- Code: `notebooks/01_solar_potential_regression.ipynb`, `src/solar_regression.py`
- README: This file, with embedded screenshots and instructions
- Screenshots/plots: `assets/*.png`
- Report/article: Problem → Data → Method → Results → Ethics → SDG Impact (link to this repo)
- Pitch deck: 8 slides (Title, Problem, Data, Approach, Results, Impact, Ethics, Next Steps)

## License & citation
- Code: MIT (adjust if preferred)
- Data: “Data courtesy of NASA/POWER Project at NASA Langley Research Center”  
  NASA POWER API: https://power.larc.nasa.gov

## Requirements
- pandas
- numpy
- scikit-learn
- matplotlib
- requests
"""
