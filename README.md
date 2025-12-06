# 🐆 **BITxADATA-Hackhaton**  
Team

[**macwsn**](https://github.com/macwsn) - Maciej Wisniewski

[**mat32121**](https://github.com/mat32121) - Mateusz Kosman

[**radbene**](https://github.com/radbene) - Radoslaw Benedykcinski

[**tommeh**](https://github.com/RETIOM) - Tomasz Idzkowski


### Winning solution in Machine Learning path


## 📂 Repository Structure

| File | Description |
|------|-------------|
| **data/train.csv** | Training dataset containing features + target |
| **data/test.csv** | Test dataset used for final evaluation |
| **data/sample_submission.csv** | Example submission format required for scoring |

---

## 🧾 Dataset Description

| Column | Meaning | Type |
|--------|---------|------|
| `data` | Date (YY-MM-DD) | datetime |
| `święto` | Public holiday | bool |
| `dzień_roboczy` | Working day | bool |
| `pogoda` | Weather type (categorical) | string |
| `temperatura` | Temperature [°C] | int |
| `odczuwalna_temperatura` | Feels-like temperature [°C] | int |
| `wilgotność` | Humidity [%] | int |
| `prędkość_wiatru` | Wind speed [km/h] | int |
| `studenty_ms` | **Target — number of students on campus** | int |

---

## 🎯 Goal

Build a predictive model estimating the number of students per day.  
Evaluation metric: **RMSLE** (Root Mean Squared Logarithmic Error).

> Lower RMSLE = better model quality.

---

## 🚀 Solution Approach / Methodology

### 🧹 1. Data Loading & Cleaning
- Train + Test merged for feature consistency  
- Date column transformed to `datetime`  
- Boolean values converted to integers  

### 🏗 2. Feature Engineering  
Key generated features:

| Feature | Purpose |
|--------|----------|
| `studencki_czwartek` | Captures traditionally high-attendance Thursdays |
| `start_roku` | Early October spike (academic year start) |
| `juwenalia_czas` | Mid-May festival period, expected crowd increase |
| `wakacje`, `sesja`, `weekend` | Seasonal + exam + weekend indicators |
| `zla_pogoda_total` | Aggregates bad weather factors |
| Weather expanded via `pd.get_dummies()` | Categorical → numeric |

These engineered features significantly increased model sensitivity to real academic patterns.

### 🤖 3. Model & Hyperparameter Tuning  
Model used: **XGBRegressor**

Hyperparameters optimized using **Optuna**, including:

- `max_depth`, `gamma`
- `reg_alpha`, `reg_lambda`
- `learning_rate`
- `n_estimators`
- `subsample`, `colsample_bytree`

KFold cross-validation ensured stable, non-overfitted evaluation.

### 🏁 4. Final Training & Prediction
- Target transformed using **log1p** → aligned with RMSLE scoring  
- Predictions reversed using **expm1**  
- Negative values cut at 0, results rounded  

Feature importance was analyzed to confirm the impact of engineered features — especially **Academic Thursday**, which proved highly influential.

---

## 🛠 Stack

| Category | Tools |
|---------|-------|
| Language | Python |
| ML | XGBoost |
| Optimization | Optuna |
| Data | Pandas, NumPy |
| Validation | KFold Cross-Validation |

---
