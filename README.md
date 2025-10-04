# Panel Time-Series Forecasting — Sales (Notebooks)

A portfolio-ready set of Jupyter notebooks for **panel time-series forecasting** (daily sales across multiple stores × items).  
The project emphasizes **clean validation** (holdout + rolling-origin backtest), interpretable **statistical baselines** (SARIMAX, TBATS, ARIMA), and practical **automation** (AutoTS), with optional Prophet / Darts / NeuralProphet experiments.

> **Note on data**: the original dataset is **not redistributed** here due to licensing/ownership.  
> To reproduce, place your CSV/Parquet under `data/` and adjust the path in the notebooks. A minimal schema is described below.

## What’s inside
- **Notebooks** (cleaned for sharing):
  - `practice.ipynb` — main workflow (panel setup, feature engineering: calendar & Fourier terms, backtesting, leaderboard).
  - `tbats_prophet_arima.ipynb` — focused comparison of TBATS / Prophet / ARIMA with Optuna-tuned ARIMA.
- **Validation**:
  - **Holdout**: last 90 days.
  - **Rolling-origin backtest** (expand) with forecast horizon **H=90**.
  - Primary metric: **SMAPE**; secondary diagnostics via MAE/MAPE where relevant.

## How to run
1. Python 3.9+ recommended. Create a fresh environment and install deps:
   ```bash
   pip install -r requirements.txt
   ```
2. Put your dataset under `data/` (see schema below). In the notebooks, set the file path accordingly (e.g., `data/HW_train.csv` or `data/train.parquet`).  
3. Open the notebooks and run cells top-to-bottom:
   ```bash
   jupyter lab
   ```

## Data schema (example)
Minimum columns:
- `date` — daily timestamp (YYYY-MM-DD).
- `store_id` — store identifier (int/str).
- `item_id` — item identifier (int/str).
- `sales` — target numeric value per (store_id, item_id, date).

Optional exogenous features (if available):
- `holiday`, `is_weekend`, `dow`, `month`, `quarter`, custom **Fourier** seasonalities, etc.

> If you used a file named **`HW_train.csv`** in your course, place that file into `data/` and update the paths. If that file originates from a public source (e.g., Kaggle), you can reference its official page in your local notes; this repo intentionally avoids re-hosting third‑party data.

## Results (short)
- **Leader (stable):** AutoTS on panel segments — SMAPE ≈ **8–9%** on holdout and backtest.
- **Strong classical baseline:** SARIMAX (weekly + yearly Fourier) — SMAPE ≈ **9–11%**.
- **Prophet / TBATS / ARIMA:** competitive on select segments; Prophet tends to be stable but slightly weaker; ARIMA benefits from Optuna tuning.

## Roadmap (optional)
- Add holiday calendars as exogenous factors for SARIMAX/Prophet.
- Add prediction intervals (Quantiles / Pinball loss) visualizations.
- Try a light **ensemble** (e.g., average of AutoTS + SARIMAX on overlapping segments).
- Per‑segment error analysis (top outliers, volume vs error).

## Environment
See `requirements.txt`. Heavy libraries (Prophet, PyCaret, Darts, Neural* packages) may take time to build; feel free to comment them out if you only want the statistical baselines first.

---

© 2025 — Released under the MIT License (see `LICENSE`).  
This repository shares **code and methodology only**; no third‑party datasets are redistributed.
