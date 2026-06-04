# 📈 Time Series Forecasting — ARIMA | Energy Demand Forecasting

An end-to-end Time Series Analysis & Forecasting project using **ARIMA** on Italy's 2016 electricity load and solar generation data. Covers the full pipeline from stationarity testing to model evaluation.

---

## 🚀 How to Run

```bash
# Install dependencies
pip install pandas numpy matplotlib statsmodels scikit-learn jupyter

# Launch the notebook
jupyter notebook Time_Series_Forecasting_ARIMA.ipynb
```

---

## 📁 Project Structure

```
Time_Series_Forecasting_ARIMA/
│
├── Time_Series_Forecasting_ARIMA.ipynb         # Main notebook — full pipeline
├── TimeSeries_TotalSolarGen_and_Load_IT_2016.csv  # Dataset
└── README.md
```

---

## 📊 Dataset

**`TimeSeries_TotalSolarGen_and_Load_IT_2016.csv`** — Italy 2016 hourly energy data

| Column | Description |
|---|---|
| `utc_timestamp` | Hourly UTC datetime |
| `IT_load_new` | Italy electricity load (MW) — **Target 1** |
| `IT_solar_generation` | Italy solar power generation (MW) — **Target 2** |

---

## 🔬 Full Pipeline

### 1. Data Loading & Visualization
- Loaded hourly energy data and converted `utc_timestamp` to datetime
- Plotted **Load vs Solar Generation** over the full year
- Observations:
  - Load shows **cyclical daily patterns** (peaks/valleys = day/night electricity use)
  - Solar generation shows **daytime-only generation** with seasonal fluctuation

### 2. Missing Value Handling
- Checked for nulls with `isnull().sum()`
- Filled missing values using **Forward Fill (`ffill()`)** — carries last known value forward, appropriate for time series

### 3. Stationarity Testing — ADF Test
ARIMA requires the time series to be **stationary** (mean, variance, autocorrelation don't change over time).

**Augmented Dickey-Fuller (ADF) Test:**
- Null Hypothesis: time series is **non-stationary**
- If **p-value < 0.05** → Reject null → series **is stationary**

| Series | p-value | Stationary? |
|---|---|---|
| `IT_load_new` | << 0.05 | ✅ Yes |
| `IT_solar_generation` | << 0.05 | ✅ Yes |

Both series are stationary → **d = 0**, no differencing needed.

### 4. ACF & PACF Analysis
Used to determine the optimal **p** and **q** parameters for ARIMA:

| Plot | Determines | How to read |
|---|---|---|
| **PACF** | `p` (AR term) | Lag where bars **cut off sharply** |
| **ACF** | `q` (MA term) | Lag where bars **drop off** |

From the plots → **p = 2, q = 2** → `ARIMA(2, 0, 2)`

### 5. ARIMA Modeling
- **80/20 train-test split**
- Fitted `ARIMA(p, d, q)` from `statsmodels`
- Made predictions on the test set
- Manual hyperparameter tuning tested `(2,0,2)`, `(2,1,2)`, `(2,2,2)`

### 6. Evaluation — RMSE

| Series | RMSE |
|---|---|
| `IT_load_new` | ~7715 |
| `IT_solar_generation` | ~2486 |

RMSE (Root Mean Squared Error) measures average prediction error in the same unit as the target (MW).

### 7. Visualization — Actual vs Predicted
Plotted actual vs predicted values for both series on the test set to visually assess model fit.

---

## 🧠 Key Concepts Covered

| Concept | Description |
|---|---|
| **Stationarity** | Time series property where mean/variance don't change over time |
| **ADF Test** | Statistical test to check stationarity |
| **ACF** | Autocorrelation — correlation of series with its own past values |
| **PACF** | Partial autocorrelation — direct correlation removing indirect lag effects |
| **ARIMA(p,d,q)** | AR (past values) + I (differencing) + MA (past errors) |
| **RMSE** | Root Mean Squared Error — prediction accuracy metric |

---

## 📦 Requirements

```
pandas
numpy
matplotlib
statsmodels
scikit-learn
jupyter
```

Install all with:
```bash
pip install pandas numpy matplotlib statsmodels scikit-learn jupyter
```

---

## 📉 Results Summary

The ARIMA model successfully captures the general pattern of both Italy's electricity load and solar generation time series. The model performs better on solar generation (lower RMSE) due to its more predictable day/night cyclical pattern.

---

## 👤 Author

**Jannu Akash**
GitHub: [@gunnu-15](https://github.com/gunnu-15)
