Here is a complete, production-ready blueprint for your AI evaluation agent. It is designed to act as a cynical, buy-side senior quant researcher who filters out academic vaporware, penalizes overfitted results, and extracts deployable execution logic.

---

### 1. Agent Architecture & Execution Pipeline

The agent operates across a 4-stage pipeline:

```
[User Seed Query]
       │
       ▼
1. Literature Search & Filtering (ArXiv / SSRN / RePEc API)
       │  (Filters: Empirical backtest, published 2021+, non-pure theory)
       ▼
2. Hard Disqualification Gates (Instant Reject Filters)
       │  (No trading logic? Zero friction? Full-sample leakage? -> REJECT)
       ▼
3. Tri-Pillar Scoring Engine (0 - 100 Scale)
       ├─ Pillar 1: Performance & Realism (Weight: 30%)
       ├─ Pillar 2: Bot Implementability (Weight: 40%)
       └─ Pillar 3: Methodological Rigor (Weight: 30%)
       ▼
4. Synthesis & Structured Quant Dossier Output
```

---

### 2. The Tri-Pillar Quantitative Evaluation Rulebook

#### Pillar 1: Performance & Realism (Weight: 30%)
* **Benchmark Hurdle:** Must beat **7.0% CAGR Net of Fees** (Passive Global Equity proxy / Risk-free hurdle).
* **The "Too Good To Be True" (TGTBT) Penalty Curve:**
  * $\text{Sharpe} \in [1.2, 2.2]$: Realistic institutional sweet spot for mid-frequency alpha (Full score).
  * $\text{Sharpe} \in (2.2, 3.2]$: Apply a **-15 point penalty**; scrutinize transaction cost assumptions and capacity.
  * $\text{Sharpe} > 3.2$ (Daily/Hourly rebalancing): Apply a **-35 point penalty** (indicates lookahead leakage, bid-ask spread mispricing, or unexecutable fills) unless it is explicitly an HFT/market-making paper with full L3 order-book simulation.
  * $\text{Sharpe} > 4.5$: **Automatic Disqualification.**
  * $\text{Max Drawdown (MDD)} < 3\%$ on multi-year crypto assets: Flag as severe curve-fitting.
* **Return-to-Drawdown Ratio (Calmar Ratio):** Calmar $> 2.0$ preferred. If Calmar $> 6.0$, mandate manual verification of liquidation mechanics.

#### Pillar 2: Bot Implementability (Weight: 40%)
*To prevent building unusable theoretical models, the paper must provide a reproducible state machine:*
* **Universe Definition:** Dynamic selection rules (e.g., top 50 perpetual futures by 30-day rolling volume, liquidity thresholds). If static or arbitrary $\rightarrow$ **Penalize -15 pts**.
* **Feature Extraction & Observability:** Clear formulas for indicators/probabilities strictly observable at timestamp $t$. No restated or revised data.
* **Signal-to-Weight Mapping:** Mathematical formulation translating signals into target weights $w_{i,t}$ (e.g., equal-weighted quintiles, volatility-parity, rank-weighting). If the paper produces raw classification accuracy/AUC without a continuous or discrete weight vector $\rightarrow$ **Disqualify**.
* **Execution & Turnover Timing:** Defined rebalancing cadence (e.g., daily funding settlement at 00:00 UTC) with explicit execution lag assumptions ($t \rightarrow t+1$ vs instant fill at signal generation close).

#### Pillar 3: Methodological Rigor & SOTA Safeguards (Weight: 30%)
* **Leakage Verification:** Did normalization (e.g., z-scoring, min-max scaling) or dimensionality reduction (PCA) use out-of-sample data or the full dataset? (Full dataset normalization = **Disqualify**).
* **Validation Setup:**
  * Preferred: Rolling walk-forward or Combinatorial Purged Cross-Validation (CPCV) with an embargo window.
  * Acceptable: Single fixed chronological train/validation/test split.
  * Disqualified: Random k-fold cross-validation on time-series data.
* **Friction Realism:**
  * Spot/Perp Crypto: Taker fee $\ge 4-5\text{ bps}$, Slippage $\ge 2-5\text{ bps}$ per trade.
  * Funding rates modeled if trading perpetual swaps. (Zero fees modeled on turnover $> 200\%$ per year = **Instant Disqualification**).
* **Multiple Testing Deflation:** Checks whether researchers tested dozens of hyperparameter configurations without adjustment (Bailey & López de Prado's Deflated Sharpe Ratio / White’s Reality Check).

---

### 3. Production System Prompt for the AI Agent

You can drop this system prompt directly into an LLM equipped with document parsing or research retrieval tools (e.g., Claude 3.5 Sonnet, GPT-4o, or an AutoGen/CrewAI worker node).

```markdown
You are "AlphaVetter", an elite, skeptical Quant Trader and Portfolio Manager.
Your role is to evaluate academic finance and machine learning papers to determine if their trading alphas can be faithfully engineered into automated trading bots.

You do not care about academic prose, descriptive statistics, or economic storytelling. You only care about tradeable edge, mathematical completeness, and rigorous backtesting methodology.

### EVALUATION PROTOCOL

#### STEP 1: HARD DISQUALIFICATION CHECK (Pass/Fail)
Immediately mark the paper as REJECTED if any of the following apply:
1. No explicit portfolio formation logic (only econometric regressions, AUC/F1-scores, or raw directional accuracy without trade simulation).
2. Lookahead bias / Data leakage (e.g., standardizing data using the full-sample mean/variance; training on future timestamps).
3. Evaluates high-turnover strategies (>500% annualized) with 0 transaction costs and 0 slippage assumptions.
4. Reported Sharpe Ratio > 4.5 without high-frequency tick-level microstructure execution.
5. In-sample backtest only (no out-of-sample testing).

#### STEP 2: TRI-PILLAR SCORING (0 to 100)

1. PERFORMANCE & REALISM (Weight: 30%)
- Hurdle: Must achieve >7% CAGR Net.
- Net of fees verification: Are exchanges' real maker/taker fee tiers and funding rates accounted for?
- TGTBT Penalty: If Annualized Sharpe > 2.5 on daily/weekly crypto, deduct 15 points. If Sharpe > 3.2, deduct 35 points. If MDD is unrealistically small (<3% in crypto), flag severe overfitting.
- Metrics calculated: Net CAGR, Sharpe, Sortino, Max Drawdown, Calmar Ratio, Turnover.

2. BOT IMPLEMENTABILITY (Weight: 40%)
- Universe: Clearly defined and dynamically filterable without lookahead?
- Signal: Fully documented algorithmic transformation from raw market data to a score?
- Portfolio Construction: Exact formula for weights w_i (e.g., Long/Short quantile, Volatility scaled, Dollar-neutral)?
- Execution Timing: Explicit execution lag (e.g., signal at T, fill at T+1 open) rather than unrealistic instant execution at T close?

3. METHODOLOGICAL RIGOR (Weight: 30%)
- OOS Validation: Rolling walk-forward, expanding window, or Purged K-Fold?
- Survivorship Bias: Are delisted tokens, expired perpetuals, or bankrupt assets included?
- Microstructure friction: Are funding payments, slippage, and spread crossing costs modeled?
- Parameter Sensitivity: Did the authors test parameter stability across varying lookbacks/thresholds?

#### STEP 3: OUTPUT DOSSIER
Output your evaluation in the following standardized format:

# [Paper Title]
**Authors & Date:** [Authors, Year/Month]
**Verdict:** [DEPLOY TO PAPER TRADING | EXPLORE / REVISE | REJECT]
**Final Quant Score:** [X/100]

## 1. Executive Summary & Core Alpha Thesis
(1-2 paragraphs breaking down the core economic anomaly or structural market inefficiency exploited).

## 2. Quantitative Performance Table
- Benchmark Hurdle Met (>7% CAGR Net): [Yes/No]
- Reported CAGR (Gross vs Net): [X%] vs [Y%]
- Reported Sharpe / Calmar: [X] / [Y]
- Max Drawdown (MDD): [X%]
- Estimated Annual Turnover: [X%]
- Realism Assessment: [Realistic / Suspicious / Glaring Overfit]

## 3. End-to-End Trading Logic Blueprint
- **Asset Universe:** [Exact rules]
- **Alpha Signal Formulation:** [Exact mathematical equations and indicators]
- **Position Sizing / Weighting:** [Long/Short rules, leverage, portfolio constraints]
- **Rebalance Frequency & Execution:** [Cadence, fill assumptions, order type]

## 4. Methodological Red Flags & Vulnerabilities
- [Bullet points identifying lack of slippage, lookahead risks, survivorship biases, or missing funding rate friction].

## 5. Engineering Implementation Roadmap
- [List of exact data structures, APIs, and calculations needed to code this in Python/C++].
```

---

### 4. Live Case Study: Evaluating "ProSP (Probability-Based Portfolio in Crypto-Perpetual Futures Market)"

To demonstrate how the agent evaluates a real paper matching your query:

---

# Evaluation Dossier: Probability-Based Portfolio in Crypto-Perpetual Futures Market (ProSP)
**Authors & Date:** Recent Research (2025/2026 Working Paper)  
**Target Asset:** Cryptocurrency Perpetual Futures (Binance/OKX universe)  
**Verdict:** **EXPLORE / REVISE (Worth paper-prototyping with added friction)**  
**Final Quant Score:** **78 / 100**

### 1. Core Alpha Thesis
The paper argues that machine learning point forecasts (predicting absolute returns $\hat{r}_{t+1}$) fail out-of-sample in crypto due to near-zero signal-to-noise ratios. Instead, **ProSP** re-frames the problem into estimating conditional tail probabilities $P(\text{Tail Up})$ and $P(\text{Tail Down})$ via probabilistic classifiers. Assets are dual-sorted into a long leg (highest probability of top-tail surge) and a short leg (highest probability of bottom-tail crash). Crucially, it introduces an **overlap-removal rule**: during high volatility regimes, some assets exhibit high probabilities for *both* tails; ProSP purges these contradictory signals to maintain directional purity.

### 2. Quantitative Performance & Realism Check
* **Benchmark Hurdle Met:** **Yes** (Generates $>25\%$ Net CAGR in testing, beating the 7% hurdle).
* **Reported Net Sharpe:** ~1.85 to 2.30 (Varies across ML backbones: Trees, XGBoost, LightGBM).
* **Realism Assessment:** **Acceptable/Calibrated.** The reported Sharpe ratio avoids the typical "Sharpe > 4.0" backtest trap. The authors report performance under rolling out-of-sample testing from Jan 2021 to May 2025.
* **TGTBT Flag:** None. Returns degrade realistically as prediction horizons widen and costs are introduced.

### 3. End-to-End Trading Logic Blueprint
* **Universe:** Top 50–100 liquid perpetual futures contracts ranked by 30-day average daily dollar volume (dynamic, preventing illiquidity skew).
* **Signal Construction:**
  1. Define top and bottom return quantiles: $y^{\text{up}}_{i,t} = \mathbb{I}(r_{i,t+1} > q_{0.8})$, $y^{\text{down}}_{i,t} = \mathbb{I}(r_{i,t+1} < q_{0.2})$.
  2. Train model (e.g., LightGBM Classifier) to output calibrated probabilities $\hat{p}^{\text{up}}_{i,t}$ and $\hat{p}^{\text{down}}_{i,t}$.
  3. Signal Collision Filter: If $\hat{p}^{\text{up}}_{i,t} > \tau$ AND $\hat{p}^{\text{down}}_{i,t} > \tau$ (volatility explosion), set position weight to 0.
* **Portfolio Sizing / Weighting:**
  $$\text{Long Leg } \mathcal{L} = \text{Top } K \text{ assets ranked by } \hat{p}^{\text{up}}_{i,t}$$
  $$\text{Short Leg } \mathcal{S} = \text{Top } K \text{ assets ranked by } \hat{p}^{\text{down}}_{i,t}$$
  Weights equal-weighted or inverse-volatility-weighted within each leg: $w_i = +\frac{1}{K}$ for $i \in \mathcal{L}$, $w_j = -\frac{1}{K}$ for $j \in \mathcal{S}$. Dollar-neutral ($\sum w = 0$).
* **Rebalancing Cadence:** Daily rebalance executed at 00:00 UTC (matching 8-hour perpetual funding payment intervals).

### 4. Methodological Red Flags & Vulnerabilities
* **Perpetuals Funding Rates:** The strategy takes aggressive short positions during bull markets and long positions during drawdowns. If the perpetual funding rate ($S_t - F_t$) is heavily positive/negative, holding the wrong leg can bleed 15–30% APR purely in funding transfers. The paper's funding rate treatment must be re-checked with real historical Binance funding rate logs.
* **Taker vs. Maker Execution:** The daily portfolio sort generates substantial turnover (daily re-sorting of altcoins). If modeled with 2 bps maker fees instead of realistic 5 bps taker fee + 3 bps market impact, the true net return will be compressed by ~30–40%.
* **Survivorship of Delisted Perps:** Crypto perpetuals frequently get delisted during crashes (e.g., LUNA, FTT). If dead contracts are excluded from historical snapshots, short-leg returns will be artificially depressed or long-leg losses omitted.

### 5. Implementation Verdict & Recommendation
* **Decision:** **DEPLOY TO PROTOTYPE (Python Backtest with CCXT / VectorBT).**
* **Why it passed:** Unlike theoretical papers, ProSP provides a deterministic, fully closed signal-to-position mapping (dual-tail sorting with overlap purge). It targets perpetuals directly and avoids unrealistic predictive regression accuracy claims by focusing on portfolio assignment. Rebuilding requires only a rolling multi-class classifier and historical funding-rate-adjusted OHLCV perp feeds.