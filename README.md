# Explainable Machine Learning for Dengue Forecasting in Iquitos, Peru

This repository contains the code and experiments developed for the study:

> **Evaluation of Explainable Machine Learning Models for Dengue Forecasting**

The study evaluates explainable machine learning models for forecasting weekly dengue incidence in **Iquitos, Peru**, by integrating historical epidemiological records with climatic variables obtained from **NASA POWER**.

The proposed approach combines time-series feature engineering, Bayesian hyperparameter optimization, ensemble machine learning models, and SHAP-based explainability to improve both predictive performance and model interpretability.

## Research Objective

The objective of this study is to develop and evaluate machine learning models capable of forecasting weekly dengue cases in Iquitos while identifying the epidemiological, climatic, and seasonal factors that influence model predictions.

The study evaluates the following tree-based ensemble models:

* Random Forest
* XGBoost
* LightGBM

The models are optimized using **Optuna** and evaluated using temporally ordered data to prevent information leakage. The best-performing model is subsequently analyzed using **SHAP (SHapley Additive exPlanations)**.

## Methodology

The proposed workflow consists of the following stages:

1. **Data acquisition**

   * Epidemiological dengue records from Peru's National Open Data Portal.
   * Climatic variables from NASA POWER.

2. **Data preprocessing**

   * Conversion of climatic records from Day of Year (DOY) to calendar dates.
   * Aggregation of climatic variables at the weekly level.
   * Aggregation of dengue cases by epidemiological week.
   * Integration of epidemiological and climatic datasets.
   * Treatment of weeks without reported cases.

3. **Feature engineering**

   * Autoregressive dengue lags from 1 to 4 weeks.
   * Rolling averages over 4 and 8 weeks.
   * Lagged climatic variables.
   * Seasonal encoding using sine and cosine transformations.
   * Temperature-range and precipitation-humidity interaction features.

4. **Model training and optimization**

   * Chronological train-validation split.
   * Time-series cross-validation.
   * Bayesian hyperparameter optimization with Optuna.
   * Evaluation of Random Forest, XGBoost, and LightGBM.

5. **Model explainability**

   * Global feature importance using SHAP values.
   * SHAP summary plots.
   * SHAP dependence plots.
   * Analysis of non-linear feature effects and interactions.

## Repository Structure

```text
xai-dengue-forecasting/
│
├── Dengue_Iquitos_Version_Original.ipynb
├── Explicabilidad.ipynb
├── README.md
├── requirements.txt
└── LICENSE
```

### Notebooks

#### `Dengue_Iquitos_Version_Original.ipynb`

This notebook contains the main machine learning pipeline, including:

* Data loading and preprocessing.
* Integration of epidemiological and climatic data.
* Feature engineering.
* Chronological data partitioning.
* Time-series cross-validation.
* Hyperparameter optimization using Optuna.
* Training of Random Forest, XGBoost, and LightGBM models.
* Model evaluation using regression metrics.
* Visualization of observed and predicted dengue cases.

#### `Explicabilidad.ipynb`

This notebook contains the explainability analysis of the selected model using SHAP.

The analysis includes:

* Global feature importance.
* SHAP summary plots.
* Feature dependence plots.
* Analysis of non-linear relationships.
* Analysis of interactions between epidemiological, climatic, and seasonal variables.

## Dataset

The study integrates two sources of information:

### Epidemiological data

Weekly dengue case records for **Iquitos, Peru**, covering the period from **2000 to 2024**.

### Climatic data

Daily climatic variables obtained from **NASA POWER**, including:

* Mean temperature.
* Maximum temperature.
* Minimum temperature.
* Corrected precipitation.
* Relative humidity.
* Mean wind speed.
* Maximum wind speed.
* Minimum wind speed.

The climatic variables are aggregated at the weekly level and incorporated into the forecasting dataset using lagged representations.

> **Note:** The datasets are not included in this repository. Please consult the corresponding data sources and their terms of use before downloading or redistributing the data.

## Models

The following machine learning models are evaluated:

| Model         | Description                                                                     |
| ------------- | ------------------------------------------------------------------------------- |
| Random Forest | A bagging-based ensemble model that combines multiple decision trees.           |
| XGBoost       | A gradient boosting model with regularization and sequential tree construction. |
| LightGBM      | An efficient gradient boosting model that uses leaf-wise tree growth.           |

## Evaluation Metrics

Model performance is evaluated using:

* **R² (Coefficient of Determination):** Measures the proportion of variance explained by the model.
* **RMSE (Root Mean Squared Error):** Penalizes large prediction errors more strongly.
* **MAE (Mean Absolute Error):** Measures the average absolute difference between observed and predicted values.
* **MASE (Mean Absolute Scaled Error):** Compares model performance with a naive persistence forecast.

A MASE value below 1 indicates that the model performs better than the naive baseline.

## Results

All evaluated models demonstrated strong forecasting performance.

| Model         |         R² |       RMSE |        MAE |       MASE |
| ------------- | ---------: | ---------: | ---------: | ---------: |
| Random Forest |     0.7925 |     9.1200 |     5.4600 |     0.5722 |
| XGBoost       |     0.8024 |     8.9011 | **5.3033** | **0.5558** |
| LightGBM      | **0.8062** | **8.8138** |     5.4411 |     0.5702 |

LightGBM achieved the highest R² and the lowest RMSE, indicating strong performance in capturing high-magnitude dengue outbreaks.

XGBoost obtained the lowest MAE and MASE, showing slightly better average forecasting performance across the validation period.

All models achieved a MASE below 1, indicating better performance than a naive persistence forecast.

## Explainability

SHAP was applied to interpret the predictions generated by the selected model.

The analysis showed that recent epidemiological dynamics were the main drivers of short-term forecasts. Variables such as:

* `cases_lag1`
* `cases_roll4`
* `cases_lag2`
* `cases_lag4`

had a strong influence on the predicted number of dengue cases.

Seasonal variables and lagged climatic features also contributed to the forecasts, revealing non-linear relationships between environmental conditions and dengue transmission dynamics.

## Requirements

The notebooks were developed using Python and Google Colab.

Main dependencies include:

```text
pandas
numpy
matplotlib
seaborn
scikit-learn
xgboost
lightgbm
optuna
shap
```

Install the required packages using:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost lightgbm optuna shap
```

## How to Run

### Option 1: Google Colab

1. Open `Dengue_Iquitos_Version_Original.ipynb` in Google Colab.
2. Upload or connect the required datasets.
3. Run the notebook cells sequentially.
4. Open `Explicabilidad.ipynb`.
5. Run the SHAP analysis using the trained model and validation data.

### Option 2: Local Environment

Clone the repository:

```bash
git clone https://github.com/YOUR-USERNAME/xai-dengue-forecasting.git
```

Move to the project directory:

```bash
cd xai-dengue-forecasting
```

Install the dependencies:

```bash
pip install -r requirements.txt
```

Launch Jupyter Notebook:

```bash
jupyter notebook
```

## Reproducibility

To reproduce the experiments:

1. Obtain the epidemiological data from Peru's National Open Data Portal.
2. Obtain the climatic data for Iquitos from NASA POWER.
3. Follow the preprocessing and feature-engineering steps in `Dengue_Iquitos_Version_Original.ipynb`.
4. Run the model optimization and evaluation pipeline.
5. Execute `Explicabilidad.ipynb` to reproduce the SHAP analyses.

Results may vary slightly depending on package versions, random seeds, and computational environments.

## Limitations

The study has several limitations:

* The analysis focuses on Iquitos and may not directly generalize to other geographic regions.
* Epidemiological records may be affected by underreporting.
* The model does not include intervention-related variables such as vector-control campaigns.
* Sociodemographic, mobility, and immunity-related variables are not included.
* The evaluation period includes years affected by the COVID-19 pandemic, which may have influenced epidemiological surveillance.

## Future Work

Potential extensions include:

* Multi-horizon forecasting.
* Evaluation in additional endemic cities in Peru.
* Integration of vector infestation indicators.
* Incorporation of human mobility data.
* Inclusion of intervention and public-health variables.
* Development of an interactive dashboard for forecasts and SHAP explanations.

## License

This project is distributed under the MIT License. See the `LICENSE` file for more information.

## Acknowledgments

This work integrates open epidemiological data from Peru's National Open Data Portal and climatic information provided by NASA POWER.
