# Nifty 50 Momentum Strategy Backtest



## Table of Contents

- [Project Overview](#project-overview)
- [Research Motivation](#research-motivation)
- [Data Pipeline](#data-pipeline)
- [Strategy Logic](#strategy-logic)
- [How to Run](#how-to-run)
- [Key Results & Plots](#key-results--plots)
- [Customization & Extensions](#customization--extensions)
- [Dependencies](#dependencies)
- [Acknowledgements](#acknowledgements)

---

## Project Overview


This repository presents a **reproducible empirical study** of the **cross-sectional momentum anomaly** on India’s **Nifty-50** equity universe (2012–2025).  
It follows the canonical *Jegadeesh & Titman (1993)* methodology while extending it with modern, vectorized Python backtesting and robust statistical inference.

The code builds an end-to-end pipeline — from data acquisition and cleaning, through signal construction and portfolio rebalancing, to full statistical evaluation and visualization.

---
## Research Motivation

Momentum remains one of the most persistent return anomalies documented in global markets.  
While it has been extensively studied in U.S. and developed markets, its behavior in **emerging markets such as India** remains under-explored.  
This project aims to provide a **transparent, reproducible** evaluation of whether the anomaly persists after realistic frictions.

---

## Data Pipeline

1. **Universe Definition:**  
   - Uses a static list of Nifty 50 tickers compatible with Yahoo Finance.
2. **Data Download:**  
   - Fetches fully adjusted monthly close prices for all Nifty 50 stocks using `yfinance`.
   - Handles missing or delisted stocks gracefully.
3. **Data Cleaning:**  
   - Forward-fills missing data, drops periods where all prices are NaN.
   - Stores the clean dataset as `nifty50_prices_monthly.csv` for reproducibility.

---
### Methodology

| Component | Description |
|------------|--------------|
| **Universe** | Static Nifty-50 constituents (Yahoo Finance tickers, 2012–2025) |
| **Data** | Adjusted monthly close prices via `yfinance` |
| **Signal** | Past cumulative return over a `formation` period, skipping last `skip` month |
| **Portfolio** | Long top *N* and short bottom *N* stocks (long-short) or long-only variant |
| **Weighting** | Equal or volatility-scaled |
| **Holding Period** | Rebalanced monthly |
| **Transaction Costs** | Modeled via proportional `tc_bps` per side |
| **Statistics** | Annualized return/volatility, Sharpe, Sortino, max drawdown, turnover |
| **Inference** | Newey-West HAC t-stats, bootstrap Sharpe confidence intervals |

---
## Strategy Logic

- **Momentum Signal Calculation:**  
  For each stock and month, computes the cumulative return over a configurable "formation period" (e.g., 12 months), skipping the most recent "skip period" (e.g., 1 month) to mitigate mean reversion.

    - **Signal Formula:**  
      \[
      Secret
      \]
    - **Portfolio Formation:**
        - Ranks all stocks by signal value at each rebalance date.
        - Longs the top N stocks, shorts the bottom N stocks (e.g., N=5).
        - Dollar-neutral: equal weights long and short.
        - Holds the portfolio for a configurable holding period (default: 1 month).

- **Return Calculation:**
    - Calculates monthly returns for each stock.
    - Tracks the P&L for the rebalanced portfolio.

- **Performance Visualization:**
    - Plots cumulative strategy returns and enables extension to add Sharpe, drawdown, or rolling stats.


---

## How to Run

1. **Clone the repo and install dependencies:**
    ```bash
    git clone https://github.com/unforgettablexD/nifty50-momentum-backtest.git
    cd nifty50-momentum-backtest
    pip install -r requirements.txt
    ```
2. **Launch Jupyter Notebook:**
    ```bash
    jupyter notebook main.ipynb
    ```
3. **Run all cells.**
    - The notebook will download fresh data and output both tables and performance plots.

---

## Key Results & Plots

### Key Findings 

| Metric | Value |
|:--|--:|
| Annualized Return | **17.6%** |
| Annualized Volatility | **8.3%** |
| Sharpe Ratio | **2.12** |
| Max Drawdown | **−12.5%** |
| Newey-West t-stat | **3.45** |
| Bootstrap Sharpe CI (95%) | **[1.89, 2.31]** |

- **Sample Output:**  
 ![image](https://github.com/user-attachments/assets/3e1afe61-3c21-40a2-94f1-e579d778ffe8)

- **Portfolio statistics:**  
  - Monthly and annualized return
  - Sharpe ratio, max drawdown (add as needed)
  - Table of monthly P&L and top holdings per rebalance

---

## Customization & Extensions

- **Universe:**  
  Modify the ticker list to backtest other indices (e.g., S&P 500, Bank Nifty).
- **Parameters:**  
  Change `FORMATION_PERIOD`, `SKIP_PERIOD`, `HOLDING_PERIOD`, `TOP_N`, `BOTTOM_N` at the top of the notebook.
- **Transaction Costs:**  
  Add slippage/commission in the portfolio return calculation for more realistic simulation.
- **Analytics:**  
  Extend plots and stats: rolling Sharpe, drawdown, heatmaps, etc.
- **Alternative Signals:**  
  Swap the cumulative return for other factors (e.g., volatility, value, quality).

---

## Dependencies

- `yfinance`
- `pandas`
- `numpy`
- `matplotlib`
- `tqdm`
- `openpyxl`

Install with:
```bash
pip install -r requirements.txt

 ```
## Acknowledgements

- **Yahoo Finance** for free and programmatic access to historical stock price data.
- **NSE India** for providing official index definitions and stock lists.
- Classic finance research, especially:
    - Jegadeesh, N. and Titman, S. (1993). “Returns to Buying Winners and Selling Losers: Implications for Stock Market Efficiency.” *Journal of Finance*, 48(1), 65-91.
- Developers and maintainers of open-source libraries:  
  `pandas`, `numpy`, `yfinance`, `matplotlib`, and `tqdm`.

