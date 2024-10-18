---
layout: default
title: Appendix
---

# Appendix

## Performance Analysis Plots

### Cumulative Returns [In-sample: 2009-2019]

![Cumulative Returns (In-sample: 2009-2019)](assets/images/Cumulative_Returns.png)

### Drawdown Analysis [In-sample: 2009-2019]

![Drawdown Analysis (In-sample: 2009-2019)](assets/images/Drawdown.png)

### Annual Returns and Returns per Trade [In-sample: 2009-2019]

![Annual Returns (In-sample: 2009-2019)](assets/images/Annual_Return.png)
![Returns per Trade (In-sample: 2009-2019)](assets/images/Returns_per_Trade.png)

### Cumulative Returns [Out-of-sample: 2020-Jan 2024]

![Cumulative Returns (Out-of-sample: 2020-Jan 2024)](assets/images/Cumulative_Returns_oos.png)

### Drawdown Analysis [Out-of-sample: 2020-Jan 2024]

![Drawdown Analysis (Out-of-sample: 2020-Jan 2024)](assets/images/Drawdown_oos.png)

### Annual Returns and Returns per Trade [Out-of-sample: 2020-Jan 2024]

![Annual Returns (Out-of-sample: 2020-Jan 2024)](assets/images/Annual_Return_oos.png)
![Returns per Trade (Out-of-sample: 2020-Jan 2024)](assets/images/Returns_per_Trade_oos.png)

## Model Justification

The results below can be directly compared with the metrics in Table 1. The model utilizes the exponential function for threshold decision-making, integrates both news and Reddit data, and is applied to a selection of up to 500 stocks within the S&P 500 constituents.

### Key Statistics of Simple Threshold of 0.6 (vs Exponential Function) [In-sample: 2009-2019]

| Metric                     | Value   | Metric                      | Value    |
|----------------------------|---------|-----------------------------|----------|
| Runtime Days               | 4017    | Drawdown                    | 20.4%    |
| Turnover                   | 71%     | Probabilistic Sharpe Ratio  | 100%     |
| CAGR                       | 81.2%   | Sharpe Ratio                | 2.5      |
| Capacity (USD)             | 37M     | Sortino Ratio               | 3.2      |
| Trades per Day             | 5.8     | Information Ratio           | 2.2      |

### Key Statistics without Reddit Data (vs with Reddit Data) [In-sample: 2009-2019]

| Metric                     | Value   | Metric                      | Value    |
|----------------------------|---------|-----------------------------|----------|
| Runtime Days               | 4017    | Drawdown                    | 23.6%    |
| Turnover                   | 66%     | Probabilistic Sharpe Ratio  | 100%     |
| CAGR                       | 62.1%   | Sharpe Ratio                | 1.9      |
| Capacity (USD)             | 10M     | Sortino Ratio               | 2.5      |
| Trades per Day             | 3.4     | Information Ratio           | 1.2      |

### Key Statistics of 100 Stocks (vs 500 Stocks) [In-sample: 2009-2019]

| Metric                     | Value   | Metric                      | Value    |
|----------------------------|---------|-----------------------------|----------|
| Runtime Days               | 4017    | Drawdown                    | 18.2%    |
| Turnover                   | 58%     | Probabilistic Sharpe Ratio  | 99%      |
| CAGR                       | 42.3%   | Sharpe Ratio                | 1.7      |
| Capacity (USD)             | 3M      | Sortino Ratio               | 1.8      |
| Trades per Day             | 1.9     | Information Ratio           | 1.2      |

### Key Statistics of Mean Reversion (Long if ticker sentiment < -0.5) [In-sample: 2009-2019]

| Metric                     | Value    | Metric                      | Value    |
|----------------------------|----------|-----------------------------|----------|
| Runtime Days               | 2321     | Drawdown                    | 100.0%   |
| Turnover                   | 47%      | Probabilistic Sharpe Ratio  | 0%       |
| CAGR                       | 0.0%     | Sharpe Ratio                | -1.3     |
| Capacity (USD)             | 1.6M     | Sortino Ratio               | -0.7     |
| Trades per Day             | 1.9      | Information Ratio           | -1.5     |

*Note: Replace the table captions and references as needed based on your specific context.*

---

## Assets Organization

### 1. Images

Place all your images in the `assets/images/` directory. Ensure that the filenames match those referenced in the Markdown files. For example:

- `Cumulative_Returns.png`
- `Drawdown.png`
- `Annual_Return.png`
- `Returns_per_Trade.png`
- `Cumulative_Returns_oos.png`
- `Drawdown_oos.png`
- `Annual_Return_oos.png`
- `Returns_per_Trade_oos.png`

### 2. PDF Paper

Place your original LaTeX-generated PDF in the `assets/papers/` directory.

- `LLM-Driven-Sentiment-Momentum-Trading-Strategy.pdf`

This allows users to download the formatted paper directly from your website.

---

## Final Steps

1. **Commit and Push Your Changes**

   If you're using Git locally:

   ```bash
   git add .
   git commit -m "Add initial website content"
   git push origin main
