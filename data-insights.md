---
layout: default
title: Data Insights
---

# Data Insights

- **Sparse Signal Matrix**

  The signal matrix, structured with tickers as columns and dates as rows, exhibited a high degree of sparsity. This sparsity is expected, as not all stocks receive mentions in news articles or Reddit posts on a daily basis. Consequently, the strategy was designed to hold positions for only one day, aligning with the availability and recency of relevant signals. This temporal limitation helps mitigate the impact of infrequent signal occurrences and ensures that the portfolio remains responsive to the most recent market sentiments.

- **Noise**

  A substantial portion of the collected texts were filtered out by the Large Language Model (LLM) due to their lack of informative content. Notably, the filtering rate was higher for Reddit data compared to news data, suggesting that Reddit submissions may contain a greater prevalence of irrelevant or low-quality information. This noise reduction step was crucial in enhancing the quality of the dataset and ensuring that subsequent analyses were based on meaningful signals. By eliminating non-informative texts, we improved the signal-to-noise ratio, thereby increasing the reliability of the extracted sentiment and relevance scores.

- **Positive Sentiment Mean**

  The mean sentiment score across the dataset was approximately 0.2, indicating a slight positive bias in the sentiment scores derived from both news and social media sources. This positive skew suggests that the textual data predominantly reflect favorable market sentiments, which could influence the performance and behavior of the developed trading strategy. For instance, naively setting symmetric thresholds for long and short positions without accounting for this bias may lead to suboptimal trading decisions. Therefore, it is essential to adjust threshold parameters to account for the inherent sentiment bias to ensure balanced and effective strategy performance.

- **Ineffectiveness of Mean Reversion Based Strategy**

  The mean reversion strategy, which involves buying stocks when sentiment is low, proved ineffective in our analysis. This ineffectiveness may be attributed to several factors, including the persistent positive sentiment bias in the dataset, the high sparsity of signals, and the short holding period of one day. This insight suggests that alternative strategies, such as momentum-based approaches, may be more suitable given the characteristics of the dataset and the behavior of sentiment scores.
