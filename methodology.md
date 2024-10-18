---
layout: default
title: Methodology
---

# Methodology

Our methodology involves several key steps:

1. **Data Collection**:
   - **Reddit**: Reddit data were retrieved in Academic Torrents, a platform that provides comprehensive submissions and comments by subreddit from 2005 to 2023. The following subreddits were utilized: UKInvesting, wallstreetbets, FuturesTrading, options, investing, ASX_Bets, Trading, ValueInvesting, Economics, and Forex. Only the submissions data were utilized in this study. The dataset is publicly available at

     [https://academictorrents.com/details/56aa49f9653ba545f48df2e33679f014d2829c10](https://academictorrents.com/details/56aa49f9653ba545f48df2e33679f014d2829c10).

   - **News**: For news data, we utilized FNSPID (Financial News and Stock Price Integration Dataset), a comprehensive financial dataset designed to enhance stock market predictions by integrating quantitative and qualitative data. FNSPID comprises 29.7 million stock prices and 15.7 million financial news records for 4,775 S&P 500 companies spanning from 1999 to 2023, gathered from four stock market news websites. The dataset is publicly available at

     [https://github.com/Zdong104/FNSPID_Financial_News_Dataset](https://github.com/Zdong104/FNSPID_Financial_News_Dataset).

2. **Data Processing**

   - **Reddit**: The Reddit data were initially compressed using the Zstandard (`.zst`) format. These files were decompressed into plain JSON text files using terminal commands. After decompression, the JSON files were processed and converted to CSV format. During this conversion, the following columns were retained for submissions data: `title`, `selftext`, `author`, `score`, `subreddit`, `num_comments`, `created_utc`, `permalink`, and `ups`.

   - **News**: The news data were stored in two extensive CSV files, sized at 5.7 GB and 23.2 GB respectively. Given the substantial file sizes, the data required splitting, merging, and partitioning to facilitate efficient processing. Despite these manipulations, the final output remains consistent with what could be achieved by processing the entire CSV files without segmentation.

   Due to limitations in computational resources and time, not all available news data were input into the Large Language Model (LLM); however, the Reddit data was fully processed. Despite this constraint, the data selection process was conducted without cherry-picking, allowing us to assume that the subset used is representative of the overall text population, thereby ensuring the reliability and validity of the extracted insights.

3. **Prompt Engineering**

   We designed a prompt to filter out text deemed irrelevant by the model, resulting in a binary classification (1 for relevant and 0 for irrelevant). Following this initial filtering, we developed customized prompts for OpenAI's ChatGPT-4o mini to extract sentiment and reliability scores from each text, as well as relevance scores for each pertinent ticker within each text entry. These prompts were structured to ensure that the model consistently generated organized JSON outputs, including dictionaries that map stock tickers to their corresponding relevance scores.

4. **Model Utilization**

   We employed OpenAI's ChatGPT-4o mini, a Large Language Model (LLM), to parse each text and extract the following metrics:
   - **Sentiment Score**: A quantitative measure of the text's sentiment, ranging from -1 (very negative) to +1 (very positive).
   - **Reliability Score**: A metric assessing the reliability of the text, ranging from 0 (not reliable) to +1 (highly reliable).
   - **Relevance Dictionary**: A mapping of stock tickers to their corresponding relevance scores, indicating the degree of pertinence of each ticker to the text. Relevance scores range from 0 (not relevant) to 1 (highly relevant).

   We utilized the LangChain library to engineer prompts and interact with the OpenAI API. All requests were processed in batches of 3,000.

5. **Signal Synthesis**

   We synthesized sentiment scores for each ticker by multiplying the relevance scores associated with each ticker by the overall sentiment score of the corresponding text entry. This method quantifies the sentiment of each relevant ticker, facilitating more targeted investment decisions.

6. **Aggregation**

   We aggregated sentiment scores at both the text and ticker levels on a daily basis using a weighted mean, with reliability scores serving as weights for each text entry. This aggregation resulted in the calculation of both overall market sentiment and individual sentiment scores for each relevant ticker. While aggregating the data, we did not differentiate between news and Reddit data sources, as reliability scores were assumed to effectively account for the varying credibility of the sources.

7. **Trading Strategy**:

   - **Universe**: We restricted the investment universe to the constituents of the SPY at each trading date.

   - **Sentiment Signal**: We implemented a trading strategy that selects stocks for long positions based on their individual sentiment scores surpassing a dynamically calculated threshold. This threshold is determined using the most recent unresolved market sentiment data to capture timely insights. For instance, if our analysis is conducted on a Monday with the last trading day being Friday, we incorporate sentiment scores from Friday, Saturday, and Sunday, under the assumption that these signals have not yet been reflected in the market.

     - **Threshold Calculation**: To adaptively adjust our selection criteria in response to overall market conditions, we utilized an exponential function for threshold calculation. This function is designed to increase the threshold as the overall market sentiment decreases, effectively making it more challenging for individual stocks to qualify for selection during bearish market conditions.

       The threshold is bounded below by the parameter `lower_bound`, ensuring that it does not decrease below this value. A higher `lower_bound` results in more stringent decision criteria.

       The use of an exponential function for dynamic thresholding parallels techniques in machine learning and signal processing. Similar to adaptive thresholding in image processing and anomaly detection, our threshold function adjusts based on current market sentiment. The exponential nature resembles activation functions like the sigmoid in neural networks, introducing non-linearity to capture complex relationships between inputs and outputs.

     - **Signal Decay**: We applied a decay factor to previous sentiment signals. This approach ensures that only recent and relevant signals influence the threshold, thereby maintaining the strategy's responsiveness to current market conditions. The concept of signal decay is rooted in time-series analysis and machine learning techniques like Exponential Moving Averages (EMAs) and memory mechanisms in Recurrent Neural Networks (RNNs) such as gating in LSTMs and GRUs.

   - **Momentum Signal**: We developed a momentum signal by comparing the current market sentiment to the average market sentiment over the preceding days. This method allowed us to effectively capture short-term market sentiment shifts, helping to mitigate potential downturns while reliably capitalizing on short-term gains for each ticker with lower risk.

   - **Position Decisions**: We initiated long positions in stocks when both sentiment and momentum signals aligned. To further differentiate between weak and strong signals, we incorporated the `weight_power` parameter, which adjusts the sentiment values by raising them to a specified power. When the power exceeds 1, stronger signals are amplified, while weaker ones are diminished. These adjusted sentiment values were then normalized, ensuring their total sum equaled 1, allowing us to calculate proportional weights for each stock in the portfolio based on their relative strength.

   - **Execution**: We executed buy orders for selected stocks at 9:31 AM, one minute after the market opened. Subsequently, we liquidated all positions at the next market open, precisely at 9:30 AM on the following trading day.

   - **Model Justification**: The model and dataset were carefully selected and optimized by comparing their performance against simpler alternatives. We incorporated additional instruments and data points when they demonstrated an improvement in overall performance. For more detailed information, please refer to the Model Justification and Parameter Optimization sections in the Appendix.

8. **Backtesting & Parameter Optimization**

   We implemented the trading strategy on QuantConnect, utilizing the universe of S&P 500 constituents. The backtesting process involved simulating trades based on historical data from 2009 to 2019 to assess the strategy's performance. We evaluated key performance metrics, including the Sharpe ratio, across various parameter combinations to identify optimal settings. This comprehensive analysis enabled us to determine the strategy's robustness and its ability to consistently outperform the S&P 500 index under different market conditions.

   - **Parameters**:
     - `steepness`: Controls the rate at which the threshold increases as overall market sentiment decreases.
     - `lower_bound`: The minimum value that the threshold can attain, ensuring that it does not decrease below this value. This parameter sets the baseline for the threshold, preventing it from becoming too low even in strongly positive sentiment conditions.
     - `weight_power`: This parameter acts as the exponent applied to sentiment values, enhancing the influence of stronger signals while diminishing weaker ones.
     - `momentum_lookback`: Defines the number of preceding days over which the market sentiment is averaged to calculate the momentum signal.

   - **Parameter Optimization**: We conducted parameter optimization using a simple grid search methodology to identify the optimal settings for our trading strategy.
