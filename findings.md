---
layout: default
title: Findings
---

# Findings

Our analysis reveals several key insights into the efficacy and practical implementation of leveraging Large Language Models (LLMs) for trading strategies aimed at outperforming the S&P 500 index.

## Superior Performance Metrics

| Metric                     | Value   | Metric                      | Value    |
|----------------------------|---------|-----------------------------|----------|
| Runtime Days               | 4017    | Drawdown                    | 23.9%    |
| Turnover                   | 68%     | Probabilistic Sharpe Ratio  | 100%     |
| CAGR                       | 88.2%   | Sharpe Ratio                | 2.6      |
| Capacity (USD)             | 11M     | Sortino Ratio               | 3.4      |
| Trades per Day             | 4.0     | Information Ratio           | 2.2      |

**Key Statistics [In-sample: 2009-2019]**

| Metric                     | Value   | Metric                      | Value    |
|----------------------------|---------|-----------------------------|----------|
| Runtime Days               | 1471    | Drawdown                    | 24.0%    |
| Turnover                   | 65%     | Probabilistic Sharpe Ratio  | 99%      |
| CAGR                       | 81.3%   | Sharpe Ratio                | 2.4      |
| Capacity (USD)             | 63M     | Sortino Ratio               | 2.6      |
| Trades per Day             | 8.9     | Information Ratio           | 2.3      |

The developed trading strategy exhibited exceptional performance throughout both the in-sample backtesting period from 2009 to 2019 and the out-of-sample period from 2020 to January 2024. The out-of-sample backtesting was conducted up to January 2024 due to the unavailability of signal data beyond that point. During the in-sample period, encompassing 4,017 trading days, the strategy achieved a Sharpe ratio of 2.6, significantly outperforming the S&P 500 index, which maintained a Sharpe ratio of 0.717 over the same timeframe. It realized a compound annual growth rate (CAGR) of 88.2%, underscoring its substantial return generation capabilities.

In the out-of-sample period, spanning 1,471 trading days, the strategy continued to demonstrate strong performance with a Sharpe ratio of 2.4 and a CAGR of 81.3%. The slight decrease in the Sharpe ratio and CAGR indicates consistent performance even in different market conditions. Even though the Sortino ratio decreased from 3.4 to 2.6, it still reflects effective management of downside risk across both periods.

The maximum drawdown remained relatively stable, increasing marginally from 23.9% to 24.0%, showcasing robust risk management practices during adverse market conditions. The Information Ratio improved slightly from 2.2 to 2.3, indicating an enhanced ability to generate excess returns relative to its benchmark.

Additionally, the Probabilistic Sharpe Ratio remained exceptionally high, at 100% in-sample and 99% out-of-sample, confirming the statistical significance and reliability of the performance metrics. The turnover rate decreased slightly from 68% to 65%, while the average number of trades per day increased from 4.0 to 8.9, suggesting a more active trading approach in the out-of-sample period.

Overall, the strategy's consistent and robust performance across both in-sample and out-of-sample periods reaffirms its effectiveness and resilience in varying market conditions.

## Implementability & Relevance

The implementation of the trading strategy is highly feasible within existing trading infrastructures, as demonstrated by its successful deployment on QuantConnect. Its operational simplicity ensures ease of implementation and long-term maintainability. Furthermore, trading costs, including transaction fees and slippage, were incorporated into the backtesting framework, ensuring that the reported performance metrics accurately reflect real-world trading conditions.
