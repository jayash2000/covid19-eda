# COVID-19 Trend Forecasting

A beginner-friendly **time-series forecasting project** focused on examining worldwide daily reported COVID-19 cases and predicting upcoming observations using several machine-learning approaches and simple benchmark methods.

## Project Overview

This project uses the global daily confirmed-case series to investigate patterns such as trend, autocorrelation, and weekly seasonality. The main question is whether a machine-learning model can improve on a seasonal-naive forecast, where the prediction is based on the corresponding day from the previous week.

Model performance is evaluated using a chronological train/test split and four common regression metrics: MAE, RMSE, MAPE, and R².

## Data

- **Dataset:** JHU CSSE COVID-19 time-series data: CSSEGISandData/COVID-19
- **Input file:** time_series_covid19_confirmed_global.csv
- **Target:** Global daily confirmed cases
- **Processing:** Country-level cumulative totals are aggregated globally and converted into daily new-case counts through differencing.
- **Period:** 2020-01-22 through 2023-03-09
- **Observations:** 1,143 daily records

## Project Structure

```
COVID-19 Trend Analysis/
├── 01_eda.ipynb
├── 02_data_cleaning.ipynb
├── 03_model_building.ipynb
├── utils.py
├── requirements.txt
├── README.md
└── data/
    └── covid.csv
```
Run notebooks in order: 
```
01_eda.ipynb → 02_data_cleaning.ipynb → 03_model_building.ipynb
```

## Features

The forecasting pipeline combines historical case values, rolling statistics, and calendar-based variables.

## Time-Series Features

- Lagged values from 1, 2, 3, 7, and 14 days earlier
- 7-day rolling mean
- 14-day rolling mean
- 7-day rolling standard deviation
- 14-day rolling standard deviation

## Calendar Features

- Day of the week
- Month
- Weekend indicator

These variables are intended to capture both short-term dependence and recurring weekly reporting patterns.

## Models

- Naive forecast
- Seasonal-naive forecast

## Machine-Learning Models

- Linear Regression
- Ridge Regression
- Lasso Regression
- Decision Tree
- Random Forest
- Gradient Boosting
- K-Nearest Neighbors

The seasonal-naive method predicts a value using the corresponding observation from the previous week.

## Evaluation

Model performance is measured using:

- **MAE:** Mean Absolute Error
- **RMSE:** Root Mean Squared Error
- **MAPE:** Mean Absolute Percentage Error
- **R²:** Coefficient of Determination

The dataset is divided chronologically rather than randomly so that future observations are not used to train the models.

## Results

The results below were obtained by executing `03_model_building.ipynb`.

The experiment uses an 80/20 chronological split, with 226 observations allocated to the test set.

| Model | MAE | RMSE | MAPE % | R² |
|---|---|---|---|---|
| **Linear Regression** | **65,059** | **98,967** | 16.10 | **0.8688** |
| Ridge | 65,005 | 99,003 | 16.09 | 0.8687 |
| Random Forest | 74,656 | 101,554 | 21.04 | 0.8618 |
| Seasonal Naive | 74,271 | 111,325 | 18.15 | 0.8339 |
| Naive | 742,448 | 785,583 | 312.32 | −7.27 |

## Main Observations

### 1. Recent history is useful for forecasting
The regression models perform reasonably well because daily reported cases exhibit substantial short-term dependence. Recent observations provide useful information about the following day's reported count.

### 2. Weekly reporting patterns are important
A noticeable seven-day pattern is present in the dataset. Reported case numbers often vary according to the day of the week, making the weekly lag and calendar variables useful predictors.

The exploratory analysis in 01_eda.ipynb includes time-series visualizations and seasonal decomposition to examine this behavior.

### 3. Machine-learning models improve on the simple naive approach
The ordinary naive forecast performs poorly because COVID-19 case numbers changed substantially throughout different periods of the pandemic. Using only the immediately preceding observation therefore provides a weak representation of the underlying series.

### 4. The seasonal baseline is more competitive
The seasonal-naive approach uses the value from the same weekday in the previous week. Its performance is considerably stronger than the ordinary naive forecast because it incorporates the recurring weekly reporting pattern.

### 5. Results should be interpreted carefully
The target variable represents reported confirmed cases, not the actual number of infections.

Observed values can be affected by factors such as:

- Testing availability
- Reporting delays
- Changes in reporting procedures
- Public-health policies
- Differences in data collection
- Backlogged or corrected reports

Consequently, the forecasting models primarily capture statistical patterns in the reported data rather than underlying epidemiological processes.

## Limitation

This project has several limitations:

- The target is based on reported cases rather than estimated infections.
- Reporting practices changed throughout the pandemic.
- The model does not explicitly include vaccination rates, mobility, interventions, variants, or other epidemiological variables.
- Long-term forecasts may be substantially less reliable than short-horizon predictions.
- The chronological test split evaluates performance on a specific historical period and may not represent future pandemic conditions.
- A strong statistical relationship between lagged values and future observations should not be interpreted as evidence of causation.

## Project Workflow

The analysis is organized into three main stages.

### Step 1 — Exploratory Analysis
`01_eda.ipynb`

This notebook examines:

- Overall case trends
- Daily changes
- Weekly patterns
- Rolling statistics
- Time-series behavior
- Seasonal structure

### Step 2 — Data Preparation
`02_data_cleaning.ipynb`

This stage prepares the raw dataset for modeling by:

- Aggregating global case counts
- Converting cumulative totals into daily observations
- Creating lag variables
- Generating rolling statistics
- Adding calendar features
- Handling missing observations created by lagging

### Step 3 — Model Development
`03_model_building.ipynb`

The final notebook:

- Creates the chronological train/test split
- Trains the forecasting models
- Generates predictions
- Evaluates each model
- Compares the results with baseline methods
- Produces the final visualizations

## Technology Stack

The project uses Python and the following libraries:

- **pandas —** data manipulation
- **NumPy —** numerical operations
- **Matplotlib —** visualization
- **scikit-learn —** machine-learning models and evaluation
- **statsmodels —** statistical and time-series analysis

## Getting Started

### 1. Clone the Repository
```
git clone <your-repository-url>
cd "COVID-19 Trend Analysis"
```

### 2. Install Dependencies
```
pip install -r requirements.txt
```

### 3. Start Jupyter Notebook
```
jupyter notebook
```

### 4. Run the Notebooks

Execute the notebooks sequentially:

```
01_eda.ipynb
02_data_cleaning.ipynb
03_model_building.ipynb
```

Running them in this order ensures that the exploratory analysis, data preparation, and modeling stages are completed consistently.

## Dataset Source

The project uses the COVID-19 time-series data maintained by the Johns Hopkins University Center for Systems Science and Engineering (JHU CSSE):

[CSSEGISandData/COVID-19](https://github.com/CSSEGISandData/COVID-19)

The relevant source file is:

```
time_series_covid19_confirmed_global.csv
```

## Reproductibility

The reported model metrics are based on the results generated by `03_model_building.ipynb`.

To reproduce the analysis:

1. Install the packages listed in requirements.txt.
2. Place the required dataset in the appropriate data/ directory.
3. Execute the notebooks in numerical order.
4. Review the generated predictions, metrics, and visualizations.

## Summary

This project demonstrates how a relatively simple time-series feature-engineering pipeline can be combined with classical machine-learning regression models to forecast daily reported COVID-19 cases.

The analysis emphasizes the importance of lagged observations, rolling statistics, and weekly calendar effects when modeling this type of time-dependent data. It also demonstrates why forecasting models should be compared against simple baselines rather than evaluated in isolation.