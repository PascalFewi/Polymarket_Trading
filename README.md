# Polymarket Bet Orderbook Dataset Documentation

Over the last six months, I have been archiving orderbooks on Polymarket for all Bitcoin-related prediction markets. My goal is to make this data (including slippage calculations) available until at least September 2025, possibly as a paid service (small fee).

In addition, I’ve built a comprehensive dataset designed for machine learning research and AI trading bot development on Polymarket. This dataset will also be released.

I am currently training machine learning models on this dataset when I have some spare time. I plan to share my insights and results as well on this page. Also, I want to share the code when a meaningful result is achieved.

For now, only the insights and statistical analyses are publicly available here. Enjoy exploring the data! If you have any questions, feel free to open an Issue 😊.

- [Polymarket Bet Orderbook Dataset Documentation](#polymarket-bet-orderbook-dataset-documentation)
  - [Overview](#overview)
  - [Use Case](#use-case)
  - [Columns](#columns)
  - [Dataset Size](#dataset-size)
  - [Notes](#notes)
- [Data Insights](#data-insights)
  - [Profit Opportunity Analysis](#profit-opportunity-analysis)
    - [Profit from Buying "NO" (profit\_buying\_no)](#profit-from-buying-no-profit_buying_no)
    - [Profit from Buying "YES" (profit\_buying\_yes)](#profit-from-buying-yes-profit_buying_yes)
    - [Commentary](#commentary)


## Overview

This dataset contains time series data from the Polymarket prediction market, specifically focused on binary Bitcoin markets. The data includes orderbook-derived prices and features, target prices for each market, and reference Bitcoin price data. The purpose of the dataset is to train machine learning models to predict short-term, persistent movements in the market price for buying and selling contracts, while minimizing the impact of transient noise (such as large one-off trades).

## Use Case

The main prediction targets are:

* `avg_buy_price_median_next5m`: The median market price to buy 100 shares (including slippage) in the next 5 minutes.
* `avg_sell_price_median_next5m`: The median market price to sell 100 shares (including slippage) in the next 5 minutes.

The focus is on learning to predict bet price changes that reflect genuine market movement, rather than random spikes caused by temporary orderbook depletion.

## Columns

* `id`: Unique identifier for each Polymarket contract/market.
* `timestamp`: Timestamp of the data point (UTC).
* `avg_buy_price`, `avg_sell_price`: Current "slippage" price for buying/selling 100 shares on the orderbook at this timestamp.
* `mode`: Type of market — either "reach by" or "above on".
* `finish_date`: Market expiry date.
* `target_price`: The BTC price target for the market (in USD).
* `time_left_seconds`: Seconds left until market close.
* `avg_buy_price_min_last5m`, `avg_buy_price_max_last5m`, `avg_buy_price_median_last5m`: Rolling window statistics for the buy price over the previous 5 minutes.
* `avg_sell_price_min_last5m`, `avg_sell_price_max_last5m`, `avg_sell_price_median_last5m`: Rolling window statistics for the sell price over the previous 5 minutes.
* `avg_buy_price_median_next5m`, `avg_sell_price_median_next5m`: The median market buy/sell price for the **following** 5 minutes (target variables for prediction).
* `unix`: Unix timestamp of the row.
* `symbol`: Ticker symbol for Bitcoin (e.g., BTC-USD).
* `close`: Bitcoin price at the current timestamp.
* `change_1min`, `change_2min`, `change_5min`, ..., `change_180d`: Relative changes in the Bitcoin price over various lookback windows (1 minute, 2 minutes, ..., 180 days).
* `relative_price`: The market's BTC price target divided by the actual BTC price at this timestamp.

## Dataset Size

* **Rows**: 3,827,161
* **Markets**: All Polymarket "will BTC reach/above" binary options with a known price target
* **Granularity**: Minute-level and tick-level for some features

## Notes

* "avg\_\*\_price" fields reflect **slippage prices** (the actual cost to fill a 100-share order at once).
* Target columns (`avg_buy_price_median_next5m`, `avg_sell_price_median_next5m`) are designed to ignore short-term market noise.
* "mode" indicates market type: "reach by" = price threshold by a date, "above on" = price threshold at a date.
* BTC price changes are from an external feed (not from Polymarket itself).
* All rows containing `NaN`s have been removed. 

---

# Data Insights

This section summarizes statistical insights and profitability analysis on the Polymarket Bet Orderbook Dataset.



## Profit Opportunity Analysis

### Profit from Buying "NO" (profit\_buying\_no)


* Profitable trades (>=0.00): **53,284** (1.17%)
* Profitable trades (>=0.01): **8,808** (0.19%)
* Profitable trades (>=0.03): **1,058** (0.02%)
* Profitable trades (>=0.05): **404** (0.01%)
* Negative trades (<0): **4,504,315** (98.83%)

**Distribution statistics:**

```
count    4,557,599
mean    -0.0166
std      0.0149
min     -0.89
25%     -0.02
50%     -0.012
75%     -0.01
max      0.972
```


### Profit from Buying "YES" (profit\_buying\_yes)


* Profitable trades (>=0.00): **53,575** (1.18%)
* Profitable trades (>=0.01): **8,425** (0.18%)
* Profitable trades (>=0.03): **1,373** (0.03%)
* Profitable trades (>=0.05): **556** (0.01%)
* Negative trades (<0): **4,504,024** (98.82%)

**Distribution statistics:**

```
count    4,557,599
mean    -0.0167
std      0.0150
min     -0.849
25%     -0.02
50%     -0.012
75%     -0.01
max      0.83
```


### Commentary

* The vast majority of round-trip trades (buying either YES or NO) are not profitable under current market conditions.
* Median and mean profits are both negative, consistent with expected orderbook spread and slippage costs.
* Profitable trades (>0.01 or 1%) are extremely rare (<0.2% of all cases).
* Large outlier profits are rare and likely correspond to special cases (market closure, sudden price moves, data edge cases).

!\[Add additional plots or insights as needed]

