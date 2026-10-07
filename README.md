# Healthcare Claims ML Forecaster (ZPM & Youth Care)

This Jupyter Notebook automates the processing, trend analysis, and end-of-year financial forecasting for healthcare declarations. It applies a Machine Learning ensemble to historical claim data to predict the total annual approved funds (`Sum_received`) and tracks cumulative claims against insurer contract ceilings (`Contract-plafond`). 

The pipeline handles two distinct healthcare funding streams:
1. **ZPM** (Zorgprestatiemodel - Adult Mental Health Care) via `Zorggroep` and `Uzovi` codes.
2. **Youth Care** (Jeugdtraject) via `Jeugdzorgregio-naam`.

## Key Features

* **Automated Data Cleaning:** Reads directly from standardized financial Excel exports (skipping header metadata rows), maps UZOVI provider codes to specific health insurance groups (e.g., Insurer A, Insurer B, Insurer C), and handles missing values.
* **Feature Engineering:** Calculates average claim values, total claim volumes, and year-to-date (YTD) cumulative claim trends (daily run-rate slope) per region or healthcare provider group.
* **Ensemble Machine Learning:** Uses a `VotingRegressor` combining five distinct models to predict the final year-end totals:
  * Linear Regression
  * Ridge Regression
  * Lasso Regression
  * Random Forest Regressor
  * Gradient Boosting Regressor
* **Ceiling Monitoring:** Maps current and projected claims against predefined insurer contract limits to identify potential budget overruns.
* **Time-Series Extrapolation:** Generates projected future records estimating the cumulative trajectory up to the end of the financial year for dashboarding and internal reporting.

## Required Input Files

Place the following Excel files in the same directory as the notebook. The script expects data tables to begin on **Row 6** (`header=5`) across all inputs:

* `Lijst declaraties.xlsx`: Active declarations and claims for the current operating year.
* `Lijst declaraties historie.xlsx`: Historical claims data from prior years used to train the machine learning models.
* `ZPM-contract-plafond verzekeraars.xlsx`: Contract ceilings defining maximum allowable claim budgets per `Zorggroep`.

## Dependencies

The script requires Python 3 and the following dependencies:

```bash
pip install pandas scikit-learn openpyxl
```

## Technical Pipeline

1. **Historical Baseline:** Parses historical declaration records to extract target variables (`Sum_received`) and training features (`Avg_received`, `Nbr_Claims`, encoded region/group categories, and daily claim trends).
2. **Current Year Setup:** Filters active operational datasets for the current target period and calculates matching YTD feature metrics.
3. **Model Training:** Applies one-hot encoding to provider/region categoricals and fits the `VotingRegressor` ensemble on historical data.
4. **Prediction & Performance Evaluation:** Evaluates ensemble outputs using Mean Absolute Error (MAE) and R² metrics, outputting group-level forecasts.
5. **Run-Rate Extrapolation:** Calculates linear daily run-rates for active claims to generate `cumulative_prediction` timelines through year-end (`YYYY-12-31`).

## Output

The notebook processes both ZPM and Youth Care segments sequentially and generates a final consolidated output:

* **`PREDICTIONS.xlsx`**: A cleaned Excel workbook containing chronological data points per provider/region, including historical claim totals, defined contract ceilings, and ML-generated year-end projections.
