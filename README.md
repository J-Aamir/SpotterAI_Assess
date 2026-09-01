# Spotter AI - Freight Rate Prediction Assessment

**Author:** Javeria Aamir

## Project Overview
This repository contains a machine learning pipeline developed for the Spotter AI freight rate prediction assessment. The objective is to accurately predict load rates based on route characteristics (distance, coordinates), freight details (weight, equipment), and temporal dynamics (seasonality, day-of-week trends). 

## Repository Structure
Due to organizational preferences, the project files are structured into specific directories:
* `Datasets/`: Contains the raw training, validation, and December template files.
* `Pyfiles/`: Contains the main Jupyter Notebook (`Model.ipynb`) and the official validation script (`score.py`).
* `Outputs/`: Contains the final model predictions (`validation_predictions.csv` and `december-chart-inputs.csv`) alongside the generated chart.

## Setup & Installation
To reproduce the environment and run the code, install the necessary dependencies:

```bash
pip install -r requirements.txt
```

## Modeling Approach
The final deployment model utilizes a **Weighted Voting Ensemble** combining the strengths of two algorithms:
1. **HistGradientBoostingRegressor (80% weight):** Captures complex, non-linear interactions between spatial coordinates, market indices, and load attributes.
2. **Ridge Regression (20% weight):** Provides smooth, continuous gradient learning for temporal features to prevent the decision trees from outputting stagnant "flat-line" predictions across similar dates.

**Key Preprocessing Highlights:**
* **Zero-Leakage Imputation:** Missing values for `weight` and `market_index` were imputed using medians calculated *strictly* from the training dataset.
* **Temporal Feature Engineering:** Engineered granular time-based features (`day_of_week`, `day_of_year`, `is_weekend`, `is_month_end`) from the `date` column to ensure the model responds dynamically to weekend dips and end-of-year holiday spikes in December.

## Running the Validator
To run the official Spotter AI validation script with this repository's folder structure, execute the following command from the root directory:

```bash
python Pyfiles/score.py --predictions Outputs/validation_predictions.csv --december-predictions Outputs/december-chart-inputs.csv
```
