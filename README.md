# Global Macro Breadth Engine v7.1 TURBO  
### *Robustness Restored • Hyper-Optimized • Fully Client-Side*

A **high-performance quantitative research engine** for  
**Market Breadth Analysis, Regime Detection, and Alpha Rotation**,  
running entirely in the browser with **HTML + JavaScript**.

Designed for **macro investors, systematic traders, and quant researchers**  
who want deep market insight *without backend infrastructure*.

**Tech Stack:** Tailwind • Chart.js • Plotly • MathJax  
**Data Source:** Yahoo Finance (5Y Daily)

</div>

---

## 🚀 What This Engine Does (In One Sentence)

> **Transforms raw multi-asset price data into actionable market regimes and optimized strategies using Market Breadth, Hyper-Parameter Grids, and Robustness Analysis.**

---

## ✨ Core Capabilities

### Market Intelligence
- Multi-asset **Market Breadth** (Stocks / ETFs / Crypto)
- **Composite Score v6** (Trend × Momentum × Volatility × Flow)
- Dynamic **Market Regime Detection** (Bull / Distribution / Bear)

### Strategy Research
- Index strategies (Switch, Scale, Regime, Contrarian)
- **Alpha Rotation** (Top-N momentum stock picking)
- Full **Grid Search**: Lookback × Rebalance
- Strategy matrix with live sorting & filtering

### Risk & Robustness
- CAGR, Sharpe, Sortino, Max Drawdown, Calmar
- **4D Hyper-Parameter Surface**
- **Robust Cluster Detection** (Neighbor-Weighted Stability)
- Distribution analysis with SD Zones
- Monte Carlo forward simulations

### Architecture
- 100% client-side (no server, no database)
- Async + chunked simulation (non-blocking UI)
- Single-file deployment (GitHub Pages ready)

---

## 🧠 System Workflow (High Level)

```text
1. Fetch Market Data (Yahoo Finance, 5Y)
2. Align Dates & Normalize Price Matrices
3. Precompute Indicators (ROC, MACD, Volatility)
4. Calculate Breadth (Multiple Weighting Schemes)
5. Run Hyper-Grid Simulation (LB × RB × Strategy)
6. Rank, Visualize, and Stress-Test Results
````

---

## 📦 Asset Universe

* **Presets**

  * QQQ: Top US Technology Leaders
  * SPY: US Market Leaders
  * Crypto: Top Market Cap Coins
* Manual symbol input supported
* Benchmark configurable (default: QQQ)

---

## 🧮 Mathematical Foundations

### 1. Rate of Change (ROC)

Used as the core momentum signal.

[
ROC_{t,n} = \frac{P_t - P_{t-n}}{P_{t-n}}
]

**Example**

* ( P_t = 110 )
* ( P_{t-20} = 100 )

[
ROC = \frac{110 - 100}{100} = 0.10 = 10%
]

---

### 2. Market Breadth (General Form)

[
Breadth_t = \frac{\sum_{i=1}^{N} w_i \cdot I(ROC_{i,t} > 0)}{\sum_{i=1}^{N} w_i}
]

Where:

* ( I(\cdot) ) = indicator function (true = 1, false = 0)
* ( w_i ) = asset weight

#### Weighting Methods

| Method      | Weight Definition        |           |   |
| ----------- | ------------------------ | --------- | - |
| Equal       | ( w_i = 1 )              |           |   |
| Volume      | ( w_i = Volume_{i,t} )   |           |   |
| Trend       | ( w_i =                  | ROC_{i,t} | ) |
| Inverse Vol | ( w_i = 1/\sigma_{i,t} ) |           |   |

---

### 3. Composite Score v6 (Market Regime Core)

A multi-factor normalized regime score:

[
Score_t =
0.3 \cdot Trend_t +
0.3 \cdot Momentum_t +
0.2 \cdot Volatility_t +
0.2 \cdot Flow_t
]

Where:

* **Trend**: MACD > Signal
* **Momentum**: ROC > 0
* **Volatility**: High-vol regime ratio
* **Flow**: Net positive volume dominance

#### Regime Mapping

|       Score | Regime       | Exposure |
| ----------: | ------------ | -------: |
|      ≥ 0.65 | Bull         |     100% |
| 0.40 – 0.65 | Distribution |      50% |
|      < 0.40 | Bear         |       0% |

---

## 📈 Trading Strategies Implemented

### 1. Index Switch (Binary)

```text
If Breadth > 50% → 100% Index
Else → 100% Cash
```

### 2. Index Scale (Linear)

[
Exposure = Breadth
]

### 3. Index Regime (Zone-Based)

```text
> 60%  → 100%
40–60% → 50%
< 40%  → 0%
```

### 4. Contrarian (Mean Reversion)

```text
Buy only when Breadth < 20%
```

### 5. Alpha Rotation (Stock Picking)

```text
Condition: Breadth > 50%
Select: Top-N assets ranked by ROC
Allocate: Equal capital per asset
```

---

## 💼 Portfolio Simulation Model

Portfolio Net Asset Value:

[
NAV_t = Cash_t + \sum Shares_{i,t} \cdot Price_{i,t}
]

* Rebalanced every **RB days**
* Transaction cost applied per rebalance
* Supports Index and Multi-Asset holdings

---

## 📊 Performance Metrics

### CAGR

[
CAGR = \left(\frac{Final}{Initial}\right)^{1/Y} - 1
]

### Sharpe Ratio

[
Sharpe = \frac{R_{ann} - R_f}{\sigma_{ann}}
]

### Max Drawdown

[
MDD = \min \left(\frac{NAV_t - Peak_t}{Peak_t}\right)
]

### Calmar Ratio

[
Calmar = \frac{CAGR}{|MDD|}
]

---

## 🧱 Robustness Analysis (v7.1)

To avoid overfitting, each grid point is scored using **neighbor-weighted stability**:

[
RobustScore_{i,j} =
\frac{\sum v_{k,l} \cdot e^{-d^2/2\sigma^2}}{\sum e^{-d^2/2\sigma^2}}
]

Where:

* ( d ) = distance in (LB, RB) grid
* Stable clusters rank higher than sharp isolated peaks

Displayed as:

* Top Robust Clusters Table
* Hyper-Surface Visualization
* Distribution SD Zones

---

## 🎲 Monte Carlo Forecast

Forward projection based on empirical returns:

[
NAV_{t+1} = NAV_t \cdot (1 + \mu + \sigma \cdot \epsilon)
]

* ( \epsilon \sim U(-0.5, 0.5) )
* 252 steps per year
* Multiple stochastic paths

---

## 🌐 Deployment

This project is **single-file deployable**.

**GitHub Pages**

1. Create a repository
2. Upload `index.html`
3. Enable Pages → `main / root`
4. Access via:

```
https://username.github.io/repository-name/
```

---

## ⚠️ Limitations

* No backend persistence
* Yahoo Finance rate limits may apply
* Not intended for live trading execution
* Research & educational use only

---

## 🗺️ Roadmap

* Save / Load strategy presets
* CSV / JSON research export
* Parallel Web Worker execution
* PWA (offline & installable)
* Optional server-side cache

---

## Disclaimer

This software is provided **for research and educational purposes only**.
It does **not** constitute financial advice.
Use at your own risk.
