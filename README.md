# Cryptocurrency Reversal/Momentum Analysis 📊🚀

A comprehensive study investigating reversal and momentum phenomena across different time horizons using 4-hour cryptocurrency price data from Binance (2020-2022).

## 🎯 Project Overview

This project analyzes the transition from **reversal effects** at shorter time horizons to **momentum effects** at longer time horizons in cryptocurrency markets. Using rank-demeaned-normalized excess (XS) strategies, we examine how market behavior changes across various holding periods.

## 🔍 Research Question

**Do cryptocurrencies exhibit reversal at short time horizons and momentum at longer time horizons?**

## 📈 Methodology

### Data Collection
- **Source**: Binance API
- **Period**: January 2020 - December 2022
- **Frequency**: 4-hour intervals
- **Assets**: Multiple cryptocurrencies

### Strategy Construction

#### 1. Reversal Strategy Framework
For each time horizon (4, 8, 12, 16, 20, 24 hours):
- Calculate average returns from previous periods
- Create rank-demeaned-normalized XS portfolios
- Hold positions for subsequent 4-hour periods

#### 2. Example: 12-Hour Strategy
```
1. Calculate average return from previous 3 × 4-hour bars
2. Rank assets by performance
3. Create rank-demeaned-normalized portfolio
4. Hold for next 4 hours
```

#### 3. Lag Adjustment
- Apply 1 × 4-hour lag to reduce first-bar reversal noise
- Enhance visibility of momentum effects

## 🔧 Time Horizons Analyzed

| Strategy | Lookback Period | Hold Period |
|----------|----------------|-------------|
| 4H | 1 × 4H | 4H |
| 8H | 2 × 4H | 4H |
| 12H | 3 × 4H | 4H |
| 16H | 4 × 4H | 4H |
| 20H | 5 × 4H | 4H |
| 24H | 6 × 4H | 4H |

## 📊 Key Metrics

### Performance Evaluation
- **Sharpe Ratios** across different time horizons
- **Risk-adjusted returns** comparison
- **Reversal vs Momentum** identification

### Expected Findings
- **Short horizons (4-8H)**: Reversal effects dominate
- **Medium horizons (12-16H)**: Transition period
- **Long horizons (20-24H)**: Momentum effects emerge

## 🛠️ Technical Implementation

### Prerequisites
```bash
pip install pandas numpy matplotlib seaborn scipy binance-python
```

### Core Functions
```python
def calculate_returns(price_data):
    """Calculate 4-hour returns from price data"""
    return price_data.pct_change()

def rank_demean_normalize(returns, lookback):
    """Create rank-demeaned-normalized XS portfolio"""
    # Rank assets by performance
    ranks = returns.rolling(lookback).mean().rank(axis=1)
    
    # Demean and normalize
    demeaned = ranks.sub(ranks.mean(axis=1), axis=0)
    normalized = demeaned.div(demeaned.abs().sum(axis=1), axis=0)
    
    return normalized

def calculate_strategy_returns(weights, future_returns):
    """Calculate strategy returns with lag adjustment"""
    # Apply 1-period lag
    lagged_weights = weights.shift(1)
    return (lagged_weights * future_returns).sum(axis=1)
```

## 📈 Analysis Pipeline

### 1. Data Preparation
```python
# Download Binance data (2020-2022)
price_data = download_binance_data('2020-01-01', '2022-12-31')
ret = calculate_returns(price_data)
```

### 2. Strategy Construction
```python
sharpe_ratios = {}
for horizon in [4, 8, 12, 16, 20, 24]:
    lookback = horizon // 4  # Convert to 4H periods
    
    # Create XS portfolio
    weights = rank_demean_normalize(ret, lookback)
    
    # Calculate strategy returns with lag
    strategy_returns = calculate_strategy_returns(weights, ret)
    
    # Compute Sharpe ratio
    sharpe_ratios[horizon] = calculate_sharpe_ratio(strategy_returns)
```

### 3. Results Visualization
```python
import matplotlib.pyplot as plt

plt.figure(figsize=(12, 6))
plt.plot(sharpe_ratios.keys(), sharpe_ratios.values(), 'o-')
plt.xlabel('Time Horizon (Hours)')
plt.ylabel('Sharpe Ratio')
plt.title('Reversal/Momentum Effects Across Time Horizons')
plt.grid(True)
plt.show()
```

## 🔬 Research Insights

### Market Microstructure
- **Reversal at short horizons**: Price corrections due to market inefficiencies
- **Momentum at long horizons**: Trend-following behavior and delayed reactions

### Behavioral Finance
- **Overreaction**: Short-term reversals from excessive price movements
- **Underreaction**: Long-term momentum from gradual information processing

## 📊 Expected Results Pattern

```
Sharpe Ratio Profile:
4H-8H:  High positive (Strong reversal)
12H-16H: Declining (Transition zone)
20H-24H: Potentially negative or lower (Momentum emergence)
```

## 🚀 Getting Started

### 1. Clone Repository
```bash
git clone https://github.com/yourusername/crypto-reversal-momentum
cd crypto-reversal-momentum
```

### 2. Install Dependencies
```bash
pip install -r requirements.txt
```

### 3. Run Analysis
```bash
python main.py
```

## 🎯 Key Findings

### Reversal Evidence
- **Short horizons** show positive Sharpe ratios
- **First 4-hour bar** exhibits strongest reversal effects
- **Market inefficiencies** create short-term opportunities

### Momentum Evidence
- **Longer horizons** may show momentum characteristics
- **Lag adjustment** helps isolate momentum effects
- **Trend persistence** emerges over extended periods

## 📚 Academic Context

This analysis contributes to the understanding of:
- **Market efficiency** in cryptocurrency markets
- **Time-varying risk factors** in digital assets
- **Behavioral finance** phenomena in 24/7 markets

## 🔗 References

1. Jegadeesh, N., & Titman, S. (1993). Returns to buying winners and selling losers
2. De Bondt, W. F., & Thaler, R. (1985). Does the stock market overreact?
3. Chordia, T., & Shivakumar, L. (2002). Momentum, business cycle, and time-varying expected returns

## 📞 Contact

For questions about methodology or implementation, please open an issue or contact the contributors.

---

⭐ **Star this repository** if you find it useful for your crypto market research!

*This project demonstrates systematic analysis of market anomalies in cryptocurrency markets using quantitative trading strategies.*
