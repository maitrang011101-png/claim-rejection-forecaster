# Spending-Prediction-System-Error-Analysis
Healthcare Revenue Gap &amp; Claim Rejection Forecasting Engine
# Healthcare Revenue Gap & Claim Rejection Forecasting Engine

An end-to-end data pipeline and ensemble machine learning engine built to analyze healthcare claims, model budget ceiling utilization, and forecast year-end revenue gaps for **ZPM** (Zorgprestatiemodel) and **Jeugdtraject** care frameworks.

---

#  Executive Summary & Architecture

In Dutch healthcare operations, claim rejections and unmonitored contract ceilings (*Contract-plafonds*) directly result in multi-hundred-thousand-euro revenue leakages. This project processes historical and current claim lists (`Lijst declaraties`) across major insurers (*Zorggroepen*) to:

1. **Clean & Parse Dynamic Excel Schemas:** Automated ingestion starting from dynamic header indices (e.g., Row 6 offset handling).
2. **Feature Engineering & Historical Tracking:** Extract YTD cumulative claim trends, calculate average claim acceptance, and map unique codes to care provider groups.
3. **Ensemble Machine Learning Pipeline:** Predict total year-end accepted claim sums (`Predicted_Sum_received`) using a blended Voting Regressor.
4. **Trajectory & Ceiling Projection:** Calculate daily linear burn rates to project future claim performance through December 31st against strict contract ceilings.

---

# Tech Stack & Methods

* **Language:** Python 3.9
* **Data Processing:** `pandas`, `numpy`, `openpyxl`
* **Machine Learning:** `scikit-learn`
  * **Linear Models:** `LinearRegression`, `Ridge`, `Lasso`
  * **Tree-Based Ensembles:** `RandomForestRegressor`, `GradientBoostingRegressor`
  * **Meta-Estimator:** `VotingRegressor` (Model Averaging)
* **Metrics:** Mean Absolute Error (MAE), Coefficient of Determination ($R^2$)

---

# Predictive Modeling & Engineering Pipeline
