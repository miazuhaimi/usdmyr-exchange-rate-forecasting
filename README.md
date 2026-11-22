# USD/MYR Exchange Rate Forecasting

Forecasting the monthly average USD/MYR exchange rate using **univariate exponential smoothing methods** and the **Box-Jenkins ARIMA methodology** in R.

## Data
- **Source:** Bank Negara Malaysia (daily exchange rates)
- **Period:** January 2016 – July 2026 (127 monthly observations, aggregated from daily rates)
- **Split:** 70% fitted (Jan 2016 – May 2023), 30% hold-out (Jun 2023 – Jul 2026)

## Methodology
1. Clean the raw BNM export and aggregate daily rates into monthly averages
2. Time series plot and decomposition (additive & multiplicative)
3. Stationarity check with the ADF test, ACF and PACF. The series becomes stationary after first differencing (d = 1).
4. **Univariate models:** Single Exponential Smoothing (SES), Holt's Method, ARRES
5. **Box-Jenkins:** five candidate ARIMA models, selected by AIC/BIC and validated with the Ljung-Box test
6. Rolling one-step-ahead forecasts over the hold-out period, compared using MSE and MAPE

## Results

**Univariate models (hold-out)**

| Model | Parameters | MSE | MAPE (%) |
|---|---|---|---|
| **SES** | α = 1.000 | **0.00602** | **1.28** |
| Holt's | α = 1.000, β = 0.485 | 0.00714 | 1.43 |
| ARRES | β = 0.200 (fixed) | 0.01150 | 1.92 |

**ARIMA candidates (fitted data)**

| Model | AIC | BIC | Ljung-Box p |
|---|---|---|---|
| ARIMA(1,1,0) | -231.63 | -226.68 | 0.444 |
| **ARIMA(0,1,1)** | **-233.49** | **-228.54** | **0.667** |
| ARIMA(1,1,1) | -231.88 | -224.45 | 0.627 |
| ARIMA(2,1,0) | -232.51 | -225.08 | 0.681 |
| ARIMA(0,1,2) | -232.09 | -224.66 | 0.656 |

ARIMA(0,1,1) had the lowest AIC and BIC, and its residuals behave like white noise (Ljung-Box p = 0.74).

**Final comparison (hold-out)**

| Model | MSE | MAPE (%) |
|---|---|---|
| ARIMA(0,1,1) | **0.00538** | 1.30 |
| SES | 0.00602 | **1.28** |

ARIMA(0,1,1) achieved a lower MSE, while SES had a slightly lower MAPE. The two models perform very similarly.

**Forecast for August 2026:** ARIMA(0,1,1) gives **RM 4.0506 per USD** (95% CI: 3.9206 – 4.1807).

## Tech Stack
R · forecast · tseries · urca · lmtest · Google Colab (R runtime)

## How to Run
1. Download the daily exchange rate data from Bank Negara Malaysia and save it as `exchange-rates.csv`
2. Open the notebook in Google Colab with the **R** runtime
3. Upload `exchange-rates.csv` to the session (Files panel)
4. Run all cells

## Limitations
- The optimal SES α = 1 means the forecast is equal to the last observed value (a naive forecast). This suggests the series behaves close to a random walk, which is common for exchange rates.
- Only univariate models were used. Macroeconomic variables (e.g. interest rates, oil prices) could improve forecasts.

## Author
**Nur Damia Izzati Zuhaimi**: Bachelor of Information Systems (Hons.) Intelligent Systems Engineering, UiTM Shah Alam  
[LinkedIn](https://www.linkedin.com/in/nur-damia-izzati-zuhaimi-4a2897298)
