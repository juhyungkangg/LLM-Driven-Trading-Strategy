---
layout: default
title: LLM-Driven Sentiment-Momentum Trading Strategy
---

# LLM-Driven Sentiment-Momentum Trading Strategy

**Author:** Ju Hyung Kang  
**Affiliation:** Mathematics in Finance, Courant Institute of Mathematical Sciences, NYU  
**Date:** September 30, 2024

---

## Table of Contents

1. [Problem Statement](#problem-statement)
2. [Summary](#summary)
3. [Relevance](#relevance)
4. [Methodology](#methodology)
    - [Data Collection](#data-collection)
    - [Data Processing](#data-processing)
    - [Prompt Engineering](#prompt-engineering)
    - [Model Utilization](#model-utilization)
    - [Signal Synthesis](#signal-synthesis)
    - [Aggregation](#aggregation)
    - [Trading Strategy](#trading-strategy)
    - [Backtesting & Parameter Optimization](#backtesting--parameter-optimization)
5. [Data Insights](#data-insights)
6. [Findings](#findings)
    - [Superior Performance Metrics](#superior-performance-metrics)
    - [Implementability & Relevance](#implementability--relevance)
7. [Conclusion](#conclusion)
8. [Appendix](#appendix)
    - [Performance Analysis Plots](#performance-analysis-plots)
    - [Model Justification](#model-justification)

---

## Problem Statement

**“Can ChatGPT beat S&P 500?”**

This study seeks to examine whether Large Language Models (LLMs), specifically OpenAI's ChatGPT-4o mini, can be employed in conjunction with alternative data sources such as financial news and Reddit submissions to outperform the S&P 500 index. Our objectives are to extract sentiment and reliability scores from each text, as well as relevance scores related to individual stocks from these textual data sources, and to develop a trading strategy that leverages these insights to inform investment decisions.

## Summary

We generated signals from **4.2 million** news articles and **2.2 million** Reddit submissions using a graphical user interface (GUI) that we developed to streamline the management of prompts, models, chains, and the execution of models. By analyzing the sentiment and reliability scores of each text, as well as the relevance scores for each pertinent ticker generated by ChatGPT-4o mini, we constructed both overall market sentiment and individual ticker sentiment for each date. Utilizing these insights, we developed an investment strategy that targets stocks with high sentiment scores during periods of short-term momentum in market sentiment. Our backtesting results from **2009 to 2019** demonstrate that this strategy achieved a maximum Sharpe ratio of **2.6**, significantly outperforming the S&P 500 index over the same period.

## Relevance

The integration of Large Language Models (LLMs) for sentiment analysis in trading strategies represents a cutting-edge advancement beyond traditional Natural Language Processing (NLP) approaches. Our methodology uniquely amalgamates high-volume alternative data sources with sophisticated language models to generate actionable trading signals. This innovative strategy facilitates the precise capture of both overarching market sentiment and individual stock sentiments, thereby potentially providing a significant competitive advantage over conventional quantitative strategies that rely exclusively on historical price data or rudimentary sentiment analysis techniques.

## Methodology

Our methodology involves several key steps:

### Data Collection

1. **Reddit**:  
   Reddit data were retrieved from [Academic Torrents](https://academictorrents.com/details/56aa49f9653ba545f48df2e33679f014d2829c10), covering submissions and comments by various subreddits from 2005 to 2023. The following subreddits were utilized:
   - UKInvesting
   - wallstreetbets
   - FuturesTrading
   - options
   - investing
   - ASX_Bets
   - Trading
   - ValueInvesting
   - Economics
   - Forex  
   Only the submissions data were utilized in this study.

2. **News**:  
   For news data, we utilized [FNSPID (Financial News and Stock Price Integration Dataset)](https://github.com/Zdong104/FNSPID_Financial_News_Dataset), comprising **29.7 million** stock prices and **15.7 million** financial news records for **4,775 S&P 500** companies from **1999 to 2023**, gathered from four stock market news websites.

### Data Processing

1. **Reddit**:  
   - Decompressed `.zst` files into JSON.
   - Converted JSON to CSV, retaining relevant columns: `title`, `selftext`, `author`, `score`, `subreddit`, `num_comments`, `created_utc`, `permalink`, and `ups`.

2. **News**:  
   - Processed large CSV files (5.7 GB and 23.2 GB) by splitting, merging, and partitioning to facilitate efficient handling.
   - Not all available news data were input into the LLM due to computational constraints; however, Reddit data was fully processed.

### Prompt Engineering

Designed prompts to:
- Filter out irrelevant text, resulting in binary classification (1 for relevant, 0 for irrelevant).
- Extract sentiment and reliability scores from each text.
- Generate relevance scores for each pertinent ticker within each text entry.
- Ensure consistent JSON outputs mapping stock tickers to relevance scores.

### Model Utilization

Employed **OpenAI's ChatGPT-4o mini** to parse each text and extract:
- **Sentiment Score**: Range from -1 (very negative) to +1 (very positive).
- **Reliability Score**: Range from 0 (not reliable) to +1 (highly reliable).
- **Relevance Dictionary**: Maps stock tickers to relevance scores (0 to 1).

Used the **LangChain** library to engineer prompts and interact with the OpenAI API, processing requests in batches of **3,000**.

### Signal Synthesis

Synthesized sentiment scores for each ticker by multiplying relevance scores with overall sentiment scores of the corresponding text entry, quantifying sentiment for targeted investment decisions.

### Aggregation

Aggregated sentiment scores at both text and ticker levels daily using a weighted mean, with reliability scores as weights, resulting in overall market sentiment and individual ticker sentiment. Did not differentiate between news and Reddit data sources, assuming reliability scores account for varying credibility.

### Trading Strategy

1. **Universe**:  
   Restricted to constituents of the **SPY** at each trading date.

2. **Sentiment Signal**:  
   - Selected stocks for long positions based on individual sentiment scores surpassing a dynamically calculated threshold.
   - **Threshold Calculation**:  
     Utilized an exponential function, increasing the threshold as overall market sentiment decreases, making selection criteria more stringent during bearish conditions.
   - **Signal Decay**:  
     Applied a decay factor to previous sentiment signals, ensuring responsiveness to recent market conditions.

3. **Momentum Signal**:  
   Compared current market sentiment to the average sentiment over preceding days to capture short-term shifts and mitigate potential downturns.

4. **Position Decisions**:  
   Initiated long positions when both sentiment and momentum signals aligned. Incorporated the `weight_power` parameter to adjust sentiment values, amplifying stronger signals and diminishing weaker ones. Normalized adjusted sentiment values to calculate proportional weights for each stock.

5. **Execution**:  
   - Executed buy orders at **9:31 AM** (one minute after market open).
   - Liquidated all positions at **9:30 AM** the next trading day.

6. **Model Justification**:  
   Compared model and dataset performance against simpler alternatives, incorporating additional instruments and data points when beneficial.

### Backtesting & Parameter Optimization

Implemented the trading strategy on **QuantConnect** using the S&P 500 constituents. Backtested using historical data from **2009 to 2019** and **2020 to January 2024**. Evaluated key performance metrics, including Sharpe ratio, through parameter optimization via a grid search methodology to identify optimal settings.

#### Parameters

- **steepness**: Controls threshold rate as market sentiment decreases.
- **lower_bound**: Minimum threshold value.
- **weight_power**: Exponent applied to sentiment values.
- **momentum_lookback**: Number of preceding days for averaging market sentiment.

#### Parameter Optimization

Conducted using grid search to identify optimal settings for the trading strategy.

## Data Insights

- **Sparse Signal Matrix**:  
  High sparsity with tickers as columns and dates as rows, leading to one-day holding periods to align with signal availability and recency.

- **Noise**:  
  High filtering rate for Reddit data indicates more irrelevant or low-quality information compared to news data, enhancing dataset quality by improving the signal-to-noise ratio.

- **Positive Sentiment Mean**:  
  Mean sentiment score ~0.2, indicating a slight positive bias in sentiment scores, necessitating threshold adjustments to account for inherent bias.

- **Ineffectiveness of Mean Reversion Based Strategy**:  
  Mean reversion (buying stocks with low sentiment) proved ineffective due to sentiment bias, high sparsity, and short holding periods, suggesting momentum-based strategies are more suitable.

## Findings

### Superior Performance Metrics

| **Metric**                   | **Value** | **Metric**                  | **Value** |
|------------------------------|-----------|-----------------------------|-----------|
| Runtime Days                 | 4017      | Drawdown                    | 23.9%     |
| Turnover                     | 68%       | Probabilistic Sharpe Ratio  | 100%      |
| CAGR                         | 88.2%     | Sharpe Ratio                | 2.6       |
| Capacity (USD)               | 11M       | Sortino Ratio               | 3.4       |
| Trades per Day               | 4.0       | Information Ratio           | 2.2       |

| **Metric**                   | **Value** | **Metric**                  | **Value** |
|------------------------------|-----------|-----------------------------|-----------|
| Runtime Days                 | 1471      | Drawdown                    | 24.0%     |
| Turnover                     | 65%       | Probabilistic Sharpe Ratio  | 99%       |
| CAGR                         | 81.3%     | Sharpe Ratio                | 2.4       |
| Capacity (USD)               | 63M       | Sortino Ratio               | 2.6       |
| Trades per Day               | 8.9       | Information Ratio           | 2.3       |

The trading strategy achieved a **Sharpe ratio of 2.6** in-sample (2009–2019) and **2.4** out-of-sample (2020–January 2024), significantly outperforming the S&P 500 index, which had a Sharpe ratio of **0.717** in-sample. The Compound Annual Growth Rate (CAGR) was **88.2%** in-sample and **81.3%** out-of-sample, demonstrating strong and consistent performance.

### Implementability & Relevance

The strategy was successfully deployed on **QuantConnect**, showcasing high feasibility within existing trading infrastructures. Its operational simplicity ensures ease of implementation and long-term maintainability. Trading costs, including transaction fees and slippage, were incorporated into backtesting, ensuring reported metrics reflect real-world conditions.

## Conclusion

**“Yes, ChatGPT can beat S&P 500.”**

This study demonstrates that integrating Large Language Models (LLMs) with alternative data sources, such as Reddit submissions and financial news, can effectively generate trading signals that outperform traditional benchmarks like the S&P 500 index. By leveraging the advanced text interpretation capabilities of OpenAI’s ChatGPT-4o mini, we extracted nuanced and detailed sentiment information from extensive unstructured datasets. Utilizing this enriched sentiment data, we developed a momentum-based trading model that achieved a Sharpe ratio of **2.6** during the in-sample period (2009–2019) and maintained strong performance with a Sharpe ratio of **2.4** in the out-of-sample period (2020–January 2024), significantly surpassing the performance of the S&P 500 in both periods. These findings highlight the substantial potential of LLMs in quantitative finance, demonstrating their ability to adapt to varying market conditions and maintain effectiveness over time. This paves the way for the development of more sophisticated models capable of interpreting complex textual data to inform and enhance investment decisions.

## Appendix

### Performance Analysis Plots

![Cumulative Returns](images/Cumulative%20Returns.png)  
*Figure 1: Cumulative Returns [In-sample: 2009-2019]*

![Drawdown](images/Drawdown.png)  
*Figure 2: Drawdown Analysis [In-sample: 2009-2019]*

![Annual Returns](images/Annual%20Return.png)  
*Figure 3a: Annual Returns [In-sample: 2009-2019]*

![Returns per Trade](images/Returns%20per%20Trade.png)  
*Figure 3b: Returns per Trade [In-sample: 2009-2019]*

![Cumulative Returns OOS](images/Cumulative%20Returns%20oos.png)  
*Figure 4: Cumulative Returns [Out-of-sample: 2020-Jan 2024]*

![Drawdown OOS](images/Drawdown%20oos.png)  
*Figure 5: Drawdown Analysis [Out-of-sample: 2020-Jan 2024]*

![Annual Returns OOS](images/Annual%20Returns%20oos.png)  
*Figure 6a: Annual Returns [Out-of-sample: 2020-Jan 2024]*

![Returns per Trade OOS](images/Returns%20per%20Trade%20oos.png)  
*Figure 6b: Returns per Trade [Out-of-sample: 2020-Jan 2024]*

### Model Justification

The results below can be directly compared with the metrics in Table 1. The model utilizes the exponential function for threshold decision-making, integrates both news and Reddit data, and is applied to a selection of up to 500 stocks within the S&P 500 constituents.

#### Key Statistics of Simple Threshold of 0.6 (vs Exponential Function) [In-sample: 2009-2019]

| **Metric**                   | **Value** | **Metric**                  | **Value** |
|------------------------------|-----------|-----------------------------|-----------|
| Runtime Days                 | 4017      | Drawdown                    | 20.4%     |
| Turnover                     | 71%       | Probabilistic Sharpe Ratio  | 100%      |
| CAGR                         | 81.2%     | Sharpe Ratio                | 2.5       |
| Capacity (USD)               | 37M       | Sortino Ratio               | 3.2       |
| Trades per Day               | 5.8       | Information Ratio           | 1.2       |

#### Key Statistics without Reddit Data (vs with Reddit Data) [In-sample: 2009-2019]

| **Metric**                   | **Value** | **Metric**                  | **Value** |
|------------------------------|-----------|-----------------------------|-----------|
| Runtime Days                 | 4017      | Drawdown                    | 23.6%     |
| Turnover                     | 66%       | Probabilistic Sharpe Ratio  | 100%      |
| CAGR                         | 62.1%     | Sharpe Ratio                | 1.9       |
| Capacity (USD)               | 10M       | Sortino Ratio               | 2.5       |
| Trades per Day               | 3.4       | Information Ratio           | 1.5       |

#### Key Statistics of 100 Stocks (vs 500 Stocks) [In-sample: 2009-2019]

| **Metric**                   | **Value** | **Metric**                  | **Value** |
|------------------------------|-----------|-----------------------------|-----------|
| Runtime Days                 | 4017      | Drawdown                    | 18.2%     |
| Turnover                     | 58%       | Probabilistic Sharpe Ratio  | 99%       |
| CAGR                         | 42.3%     | Sharpe Ratio                | 1.7       |
| Capacity (USD)               | 3M        | Sortino Ratio               | 1.8       |
| Trades per Day               | 1.9       | Information Ratio           | 1.2       |

#### Key Statistics of Mean Reversion (Long if ticker sentiment < -0.5) [In-sample: 2009-2019]

| **Metric**                   | **Value** | **Metric**                  | **Value** |
|------------------------------|-----------|-----------------------------|-----------|
| Runtime Days                 | 2321      | Drawdown                    | 100.0%    |
| Turnover                     | 47%       | Probabilistic Sharpe Ratio  | 0%        |
| CAGR                         | 0.0%      | Sharpe Ratio                | -1.3      |
| Capacity (USD)               | 1.6M      | Sortino Ratio               | -0.7      |
| Trades per Day               | 1.9       | Information Ratio           | -1.5      |

---

**Note:** To fully utilize this README, ensure that all referenced images are placed in an `images` directory within your GitHub repository and named accordingly (e.g., `Cumulative Returns.png`). Update the image paths if necessary.

Feel free to reach out for any questions or further clarifications!

