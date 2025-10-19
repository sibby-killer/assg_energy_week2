# SDG 7: Predicting Daily Solar Energy Potential (NASA POWER, Supervised Regression) 🌞⚡

Forecast daily solar irradiance (kWh/m²/day) from weather variables to support renewable energy planning and grid operations. This project advances UN SDG 7: Affordable and Clean Energy by improving solar resource forecasting.

## Quick links
- Notebook: `notebooks/01_solar_potential_regression.ipynb`
- Script: `src/solar_regression.py`
- Data: `data/nairobi_power_2018_2023.csv` (downloaded via NASA POWER API)
- Assets (plots + metrics): `assets/`
- Open in Colab:
  https://colab.research.google.com/github/USERNAME/REPO/blob/main/notebooks/01_solar_potential_regression.ipynb

## Overview
- Task: Supervised regression to predict `ALLSKY_SFC_SW_DWN` (daily solar irradiance).
- Why it matters: Better forecasts help size solar installations, plan storage, and manage grids—key to scaling clean energy.

## Results (from assets/metrics.txt)
- Gradient Boosting Regressor:
  - MAE: TBD kWh/m²/day
  - RMSE: TBD kWh/m²/day
  - R²: TBD
- Linear Regression:
  - MAE: TBD kWh/m²/day
  - RMSE: TBD kWh/m²/day
  - R²: TBD

## Visuals
- EDA: Daily irradiance over time  
  ![EDA](assets/eda_irradiance_timeseries.png)
- 2023 Actual vs Predicted (test set)  
  ![Timeseries](assets/timeseries_actual_vs_pred.png)
- Predicted vs Actual (GBR)  
  ![Scatter](assets/scatter_pred_vs_actual_gbr.png)
- Residuals (GBR)  
  ![Residuals](assets/residuals_hist_gbr.png)
- Feature Importance (GBR)  
  ![Importance](assets/feature_importance_gbr.png)

## Dataset
- Source: NASA POWER Project (daily time series; public and free)
- Variables:
  - Target: `ALLSKY_SFC_SW_DWN` (kWh/m²/day)
  - Features: `T2M` (°C), `RH2M` (%), `WS2M` (m/s), `PRECTOTCORR` (mm/day)
- Example API (Nairobi; 2018–2023):  
  https://power.larc.nasa.gov/api/temporal/daily/point?parameters=ALLSKY_SFC_SW_DWN,T2M,RH2M,WS2M,PRECTOTCORR&start=20180101&end=20231231&latitude=-1.2921&longitude=36.8219&community=RE&format=CSV&header=true  
  Note: Change LAT/LON to your city and re-run.

## Method
- Problem: Regression
- Split: Time-based
  - Train: 2018–2022
  - Test: 2023
- Features:
  - Raw: T2M, RH2M, WS2M, PRECTOTCORR
  - Engineered: `doy`, `sin_doy`, `cos_doy` (seasonality), `lag1` (persistence)
- Models:
  - Baseline: Linear Regression (with StandardScaler)
  - Main: GradientBoostingRegressor
- Metrics: MAE (primary), RMSE, R²
- Visualizations: Timeseries (actual vs predicted), scatter (pred vs actual), residuals histogram, feature importances

## How to run
- Option A — Google Colab (recommended)
  1. Open the notebook (or use the Open in Colab link).
  2. Run all cells. It will download data, train/evaluate models, and save plots/metrics to `assets/`.
  3. File → Save a copy in GitHub (commit to your repo).
  4. Download `assets/*.png` + `metrics.txt` from Colab Files panel and upload to your repo’s `assets/` folder.

- Option B — Local (Jupyter or Python)
  - Setup: Python 3.9+  
    `pip install -r requirements.txt`
  - Run notebook  
    `jupyter notebook` → open `notebooks/01_solar_potential_regression.ipynb` → Run All
  - Or run script  
    `python -c "from src.solar_regression import main; main(lat=-1.2921, lon=36.8219, out_stem='nairobi_power_2018_2023')"`

## Repo structure
