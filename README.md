# TweetStorm to Wall Street: Sentiment–Powered Stock Predictions

## Problem Statement

In the modern financial world, stock price movements are influenced not only by market data and technical indicators but also by real-world events, news cycles, and public sentiment. Traditional forecasting models often rely solely on historical stock prices, overlooking the emotional and psychological impact that news can have on investor behavior.

This gap between technical market signals and real-time sentiment makes it difficult to produce truly informed short-term predictions. Additionally, investors lack a unified platform that integrates live stock prices, financial news, and AI-driven predictions.

## Objective

The objective of this project is to design and deploy a fully automated, real-time data pipeline that:

- Collects live stock data (every 5 minutes) and financial news using APIs.
- Applies FinBERT-based sentiment analysis on news articles.
- Merges sentiment scores with technical indicators to build enriched datasets.
- Trains daily-updated SARIMAX models to predict stock prices for 15, 30, and 60-minute horizons.
- Presents actionable forecasts through an interactive, cloud-hosted dashboard.

The entire pipeline runs on AWS cloud services: EC2, S3, SageMaker, and DynamoDB.

## Summary of Results

- Developed a robust real-time data pipeline for 7 major tech stocks: AAPL, MSFT, AMZN, GOOGL, META, NVDA, TSLA.
- Integrated FinBERT-based sentiment analysis with technical stock data.
- Trained SARIMAX models on 60-day, 5-minute interval datasets enriched with engineered features.
- Stored live predictions and features in DynamoDB, and visualized them through Power BI and Streamlit dashboards.
- Enabled users to monitor real-time trends, predictions, and sentiment scores interactively.

## Data Engineering Lifecycle

### 1. Pipeline Overview

The pipeline integrates multiple AWS services and open-source tools to:

- Collect historical and real-time stock/news data.
- Perform sentiment analysis using FinBERT.
- Engineer features and train SARIMAX models.
- Generate real-time stock predictions.
- Visualize live trends and forecasts.

### 2. Data Generation

#### Historical Data

- **1-Year Daily Stock Data** (Yahoo Finance)
- **60-Day 5-Minute Interval Stock Data** (Yahoo Finance)
- **1-Year Financial News Data** (Financial Modeling Prep)

#### Real-Time Data

- **5-Minute Live Stock Data** and **News Updates** fetched continuously during market hours (9:30 AM – 4:00 PM ET).

### 3. Data Ingestion

- **Historical Data** stored in Amazon S3:
  - `/historical-data/stock_history_1y.csv`
  - `/historical-data/stock_60d_5m.csv`
  - `/news-data/news_history_1yr.csv`
- **Real-Time Data** ingested into Amazon DynamoDB:
  - `realtimedata`
  - `news_tracker`
  - `sentiment_tracker`
  - `realtime_predictions`

### 4. Data Transformation

- Aligned stock and news timestamps.
- Applied FinBERT for sentiment scoring.
- Engineered predictive features:
  - Moving Averages (EMA5, EMA12)
  - Volatility (30 min)
  - % Price Change
  - Lagged Closing Prices
  - Faded Sentiment Scores

### 5. Data Serving

- Real-time predictions displayed through an interactive Amazon QuickSight dashboard.
- Dashboard features:
  - Stock selector (AAPL, AMZN, MSFT, etc.)
  - Candlestick visualizations for 1-year trends.
  - Real-time 15/30/60-minute forecasts with sentiment indicators.

### 6. Machine Learning and Analytics

- SARIMAX models trained daily per stock using engineered features.
- Forecasted next 15, 30, and 60 minutes using real-time data.
- Model training automated on SageMaker, with outputs stored in S3.

## Limitations

- AWS costs can be significant without optimization.
- Processing lag due to model loading and inference time.
- SARIMAX may not capture extreme market volatility.
- Limited sentiment data sources (FinancialModelingPrep headlines only).
- No mid-day model retraining.
- QuickSight's real-time interactivity is limited.
- Cold starts from S3 model retrieval impact prediction speed.

## Future Work

- Migrate scheduled jobs to AWS Lambda or Fargate to reduce costs.
- Explore deep learning models like DeepAR, TFT, NeuralProphet for advanced forecasting.
- Integrate additional sentiment sources (Twitter, Reddit, Analyst Reports).
- Add drift detection for automatic model retraining triggers.
- Enhance dashboard with Streamlit or Dash for improved interactivity.

## Conclusion

This project demonstrates the practical construction of a real-time, end-to-end sentiment-powered stock prediction pipeline. Key learnings include:

- Managing batch and real-time data pipelines using AWS services.
- Preprocessing and engineering financial time series data.
- Building production-grade ML pipelines and managing model deployment lifecycles.
- Integrating real-time predictions into visual analytics for financial insights.

Through modular design and cloud-native services, we built a system that blends machine learning with market sentiment, offering a more holistic view of short-term stock behavior.

## References

- Financial Modeling Prep. (n.d.). [Financial Data API & Market News](https://site.financialmodelingprep.com/)
- yFinance. (n.d.). [Yahoo! Finance market data downloader](https://pypi.org/project/yfinance/)
- Amazon Web Services. (n.d.). [Getting Started with AWS](https://aws.amazon.com/getting-started/)
- Amazon Web Services. (n.d.). [AWS Documentation](https://docs.aws.amazon.com/)
- Amazon Web Services. (n.d.). [AWS Training and Certification](https://aws.amazon.com/training/)
- Dhar, A. (2020). [Time Series in Python – Exponential Smoothing and ARIMA Processes](https://medium.com/data-science/time-series-in-python-exponential-smoothing-and-arima-processes-2c67f2a52788)
- Selva Prabhakaran. (2019). [Time Series Forecasting with ARIMA, SARIMA, and SARIMAX](https://towardsdatascience.com/timeseries)
