# Global Macro Breadth Engine v7.0 TURBO

A high-performance, browser-based quantitative research engine for **Market Breadth Analysis**, **Strategy Optimization**, and **Alpha Rotation**, implemented entirely in **HTML + JavaScript** and deployable as a **static web application**.

This project is designed for **macro investors, quantitative researchers, and systematic traders** who want to analyze market regimes, optimize strategy parameters, and evaluate robustness without relying on backend infrastructure.

---

## 1. Key Features

- Market Breadth computation across customizable asset universes
- Multi-strategy backtesting (Index-based & Stock-level Alpha)
- Hyper-parameter grid simulation (Lookback × Rebalance)
- Risk-adjusted performance evaluation
- Monte Carlo forward projection
- Distribution & robustness analysis
- Fully client-side (no backend, no database)

---

## 2. System Architecture (High Level)

```text
User Input
   ↓
Market Data Fetch (Yahoo Finance)
   ↓
Indicator Matrix Preparation
   ↓
Breadth Calculation
   ↓
Strategy Simulation (Grid Search)
   ↓
Performance Metrics
   ↓
Visualization & Ranking
````

---

## 3. Data Source

* **Yahoo Finance (5 years, daily)**
* Symbols:

  * Equities: AAPL, MSFT, NVDA, etc.
  * Index ETFs: QQQ, SPY
  * Crypto (optional): BTC-USD, ETH-USD

Data is fetched via CORS proxy and aligned to a unified trading calendar.

---

## 4. Market Breadth Calculation

Market Breadth measures the **percentage of assets in an uptrend or positive momentum** within a universe.

### 4.1 Rate of Change (ROC)

For each asset:

[
ROC_t = \frac{P_t - P_{t-n}}{P_{t-n}}
]

**Example**

* Price today = 110
* Price 20 days ago = 100

[
ROC = (110 - 100) / 100 = 0.10 = 10%
]

---

### 4.2 Breadth (Equal Weight)

[
Breadth_t = \frac{\text{Number of assets with } ROC_t > 0}{\text{Total assets}}
]

**Example**

* Universe size = 10 stocks
* Stocks with positive ROC = 6

[
Breadth = 6 / 10 = 0.60 = 60%
]

---

### 4.3 Weighted Breadth Variants

| Method      | Weight Formula             |           |   |
| ----------- | -------------------------- | --------- | - |
| Equal       | ( w_i = 1 )                |           |   |
| Volume      | ( w_i = Volume_{i,t} )     |           |   |
| Trend       | ( w_i =                    | ROC_{i,t} | ) |
| Inverse Vol | ( w_i = 1 / \sigma_{i,t} ) |           |   |

[
Breadth_t = \frac{\sum w_i \cdot I(ROC_i > 0)}{\sum w_i}
]

---

## 5. Composite Breadth Score (v6)

A multi-factor regime score combining trend, momentum, volatility, and flow.

[
Score_t =
0.3 \cdot Trend +
0.3 \cdot Momentum +
0.2 \cdot Volatility +
0.2 \cdot Flow
]

Where each component is normalized to ([0,1]).

### Regime Mapping

| Score       | Market Regime | Exposure |
| ----------- | ------------- | -------- |
| ≥ 0.65      | Bull          | 100%     |
| 0.40 – 0.65 | Distribution  | 50%      |
| < 0.40      | Bear          | 0%       |

**Example**

* Trend = 0.7
* Momentum = 0.6
* Volatility = 0.5
* Flow = 1

[
Score = (0.3×0.7) + (0.3×0.6) + (0.2×0.5) + (0.2×1)
= 0.21 + 0.18 + 0.10 + 0.20
= 0.69 \Rightarrow Bull Regime
]

---

## 6. Trading Strategies

### 6.1 Index Switch (Binary)

[
Exposure =
\begin{cases}
1 & \text{if Breadth} > 50% \
0 & \text{otherwise}
\end{cases}
]

---

### 6.2 Index Scale (Linear)

[
Exposure = Breadth
]

**Example**

* Breadth = 70% → Invest 70% in index, 30% in cash

---

### 6.3 Index Regime (Zone-based)

| Breadth | Exposure |
| ------- | -------- |
| > 60%   | 100%     |
| 40–60%  | 50%      |
| < 40%   | 0%       |

---

### 6.4 Contrarian Strategy

[
Buy\ Index \text{ if Breadth} < 20%
]

Used to exploit extreme oversold conditions.

---

### 6.5 Alpha Rotation (Stock-Level)

1. Check market condition:
   [
   Breadth > 50%
   ]
2. Rank stocks by ROC
3. Select **Top N**
4. Allocate equal capital per stock

**Example**

* Capital = $10,000
* Top 5 stocks → $2,000 per stock

---

## 7. Portfolio Simulation

Portfolio value evolves as:

[
NAV_t = Cash_t + \sum Shares_i \times Price_{i,t}
]

Rebalancing occurs every **RB days**.

Transaction cost applied:

[
Effective\ Capital = Capital \times (1 - Fee)
]

---

## 8. Performance Metrics

### 8.1 CAGR

[
CAGR = \left(\frac{Final}{Initial}\right)^{1/Y} - 1
]

**Example**

* Initial = 10,000
* Final = 20,000
* Years = 3

[
CAGR = (2)^{1/3} - 1 = 26.0%
]

---

### 8.2 Sharpe Ratio

[
Sharpe = \frac{R - R_f}{\sigma}
]

Where:

* (R_f = 2%)
* Returns annualized

---

### 8.3 Max Drawdown (MDD)

[
MDD = \min \left(\frac{NAV_t - Peak}{Peak}\right)
]

---

### 8.4 Calmar Ratio

[
Calmar = \frac{CAGR}{|MDD|}
]

---

## 9. Monte Carlo Simulation

* Empirical mean & volatility estimated from strategy returns
* Simulate 252 trading days forward
* Generate multiple stochastic paths

[
NAV_{t+1} = NAV_t \cdot (1 + \mu + \sigma \cdot \epsilon)
]

Where:

* (\epsilon \sim U(-0.5, 0.5))

---

## 10. Deployment (GitHub Pages)

1. Create a GitHub repository
2. Upload `index.html`
3. Enable GitHub Pages:

   * Branch: `main`
   * Folder: `/root`
4. Access via:

```
https://username.github.io/repository-name/
```

---

## 11. Limitations

* No backend persistence
* Yahoo Finance rate limits may apply
* Not intended for live trading execution
* For research and educational use only

---

## 12. Roadmap (Optional)

* Save / Load configurations
* Export full backtest results
* Web Worker parallelization
* PWA support
* Server-side optimization

---

## Disclaimer

This software is provided for **research and educational purposes only**.
No financial advice is given. Use at your own risk.

