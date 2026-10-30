# 🚲 Bike Sharing Demand — Regression

[![Kaggle](https://img.shields.io/badge/Kaggle-Bike%20Sharing%20Demand-20BEFF?logo=kaggle&logoColor=white)](https://www.kaggle.com/competitions/bike-sharing-demand)
[![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Scikit-learn](https://img.shields.io/badge/Scikit--learn-ML-F7931E?logo=scikitlearn&logoColor=white)](https://scikit-learn.org/)

My fifth Data Science project and a regression task based on time-dependent data.

The goal is to predict the number of bikes rented during each hour using historical rental patterns, calendar information, and weather conditions.

<details>
<summary>🏆 Kaggle Result</summary>

**Kaggle Score: 0.39975**

</details>

---

## 🎯 Project Goal

The main objective is to build a regression model that predicts hourly bike rental demand.

The project focuses on:
* Exploring time-dependent patterns in bike rentals.
* Analysing the relationship between demand, time, and weather.
* Comparing different regression algorithms.
* Using time-aware validation instead of a random train-validation split.
* Improving model performance through feature engineering and hyperparameter tuning.

---

## 📊 Dataset

The project uses the [Bike Sharing Demand dataset](https://www.kaggle.com/competitions/bike-sharing-demand/data) from Kaggle.

The dataset contains hourly rental information, including:

* `datetime` — date and time of the rental.
* `season` — season of the year.
* `holiday` — whether the day is a holiday.
* `workingday` — whether the day is a working day.
* `weather` — weather conditions.
* `temp` — temperature.
* `atemp` — feels-like temperature.
* `humidity` — relative humidity.
* `windspeed` — wind speed.
* `count` — total number of bike rentals (target variable).

The training dataset also contains `casual` and `registered`, but these features were excluded because their sum directly determines the target variable:

`count = casual + registered`

Using them would result in target leakage.

---

## 🔍 Exploratory Data Analysis (EDA)

The first stage focused on understanding the dataset and identifying patterns that could help predict bike rental demand.

Main steps:
* Checked dataset structure, data types, and missing values.
* Analysed the target variable distribution.
* Compared rental demand across seasons, hours, working days, and weather conditions.
* Explored relationships between numerical features.
* Investigated the correlation between `temp` and `atemp`.

Key observations:
* The target variable has a right-skewed distribution.
* Working days show noticeable morning and evening peaks.
* Weekends have a more even distribution of rentals, with increased activity around midday.
* Worse weather conditions are generally associated with lower rental demand.
* `temp` and `atemp` are strongly correlated.

---

## ⚙️ Data Preprocessing & Feature Engineering

Several preprocessing and feature engineering steps were applied before model training.

* Converted the target variable using `log1p()` to reduce right skewness.
* Extracted temporal features from `datetime`:
  * `year`
  * `month`
  * `day`
  * `dayofweek`
  * `hour`
* Compared models with and without additional temporal features.
* Excluded `datetime`, `casual`, `registered`, and the target variable from the final feature set.

The `day` feature was excluded from the final model because it did not show a clear pattern during EDA and could introduce a train-test distribution gap.

---

## 🤖 Machine Learning Models

Several regression algorithms were trained and compared:

* Linear Regression
* K-Nearest Neighbors (KNN)
* Decision Tree Regressor
* Random Forest Regressor
* Gradient Boosting Regressor

The models were evaluated on a chronological train-validation split to preserve the time order of observations.

### Model Comparison

| Model | Validation RMSLE |
|---|---:|
| Linear Regression | 1.0190 |
| KNN | 0.9398 |
| Decision Tree | 0.5298 |
| Random Forest | 0.3789 |
| Gradient Boosting | 0.3951 |
| Tuned Gradient Boosting | **0.3200** |

Lower RMSLE indicates better performance.

Gradient Boosting achieved the best validation result after hyperparameter tuning.

---

## 📈 Model Evaluation & Tuning

The evaluation metric for the competition is **Root Mean Squared Logarithmic Error (RMSLE)**.

Since the target variable is right-skewed, `log1p()` was applied before model training. Predictions were converted back using `expm1()`.

To avoid data leakage and preserve temporal order:
* Used a chronological train-validation split.
* Applied `TimeSeriesSplit` for cross-validation.
* Compared baseline models before tuning.
* Used `RandomizedSearchCV` to search for better hyperparameters.

Random Forest tuning did not produce a meaningful improvement over the baseline model.

Gradient Boosting showed better results after tuning.

**Best Gradient Boosting parameters:**
* `n_estimators = 300`
* `learning_rate = 0.05`
* `max_depth = 5`
* `min_samples_split = 10`
* `min_samples_leaf = 4`
* `subsample = 0.9`
* `max_features = 0.5`

---

## 🏁 Final Result

The final model was a tuned `GradientBoostingRegressor`.

* Best local validation RMSLE: **0.3200**
* Kaggle score: **0.39975**

The model was retrained on the full training dataset and used to generate predictions for the Kaggle test dataset.

---

## 💡 What I Learned

This project helped me better understand regression problems involving time-dependent data.

### Data Analysis
* How to identify patterns in hourly, weekly, and seasonal data.
* Why visual analysis is important for understanding feature relationships.
* How highly correlated features can affect modelling decisions.

### Target Transformation & Evaluation
* How and why to use `log1p()` and `expm1()` for a right-skewed target.
* How RMSLE differs from RMSE and why the evaluation metric matters.
* How to interpret model performance when the target variable has a wide range.

### Validation & Data Leakage
* Why random train-validation splitting is not suitable for time-dependent data.
* How chronological splitting and `TimeSeriesSplit` help preserve temporal order.
* How target leakage can lead to misleading model performance.

### Machine Learning
* How different regression algorithms perform on the same dataset.
* Why tree-based models can capture non-linear relationships.
* How hyperparameter tuning can affect validation performance.

### Feature Engineering
* How to extract useful information from datetime features.
* Why not every additional feature improves model performance.
* Why checking train-test distributions is important when creating new features.

---

## 🔧 What I Would Improve

* Explore additional time-based features and their interactions.
* Investigate more advanced time-series forecasting approaches.
* Experiment with other boosting algorithms.
* Analyse prediction errors in more detail and investigate periods with the largest deviations.

---

## ▶️ How to Run

### 1. Clone the repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd <YOUR_PROJECT_FOLDER>
```

### 2. Install dependencies

It is recommended to use a virtual environment.

```bash
python -m venv venv
```

Activate it:

```bash
# Windows
venv\Scripts\activate

# macOS / Linux
source venv/bin/activate
```

Install the required libraries:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

### 3. Download the dataset

Download the competition data from [Kaggle — Bike Sharing Demand](https://www.kaggle.com/competitions/bike-sharing-demand/data).

Place `train.csv` and `test.csv` in the project directory.

### 4. Run the notebook

```bash
jupyter notebook
```

Open the project notebook and run the cells in order.

---

## 🔗 Links & Resources

* [Kaggle Competition](https://www.kaggle.com/competitions/bike-sharing-demand)
* [Kaggle Dataset](https://www.kaggle.com/competitions/bike-sharing-demand/data)
* [Scikit-learn Documentation](https://scikit-learn.org/stable/)

---

**Project workflow:**

EDA → Data Preprocessing → Feature Engineering → Model Training → Validation → Hyperparameter Tuning → Final Predictions