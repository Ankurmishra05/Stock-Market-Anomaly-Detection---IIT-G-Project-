📊 Stock Market Anomaly Detection

Capstone Project

1. Introduction

In this project, I worked on detecting unusual stock days and market stress days using only daily price and volume data.
The idea was to identify extreme behavior such as sharp price drops, spikes, unusually high trading volume, or very wide intraday ranges — without using labels, news, or complex prediction models.

The focus of this project is on:

correct time-series handling

avoiding data leakage

building an explainable and reliable anomaly detection pipeline

This project strictly follows the given Capstone Project specification.

2. Dataset

Source: Kaggle – Daily NASDAQ Stock Market Dataset

Link: https://www.kaggle.com/datasets/jacksoncrow/stock-market-dataset

Data type: Daily OHLCV data (one CSV per ticker)

Price used: Adjusted Close (to account for splits and dividends)

Time period: Data available up to April 2020

A small universe of liquid NASDAQ stocks was selected to keep the analysis focused and interpretable.

3. Feature Engineering

All features are computed per stock using only past information, ensuring there is no look-ahead bias.

The following features were engineered:

Feature	Description	Window
return	Daily percentage return	—
ret_z	Rolling z-score of returns	63 days
vol_z	Rolling z-score of log volume	21 days
range_pct	Percentile of (High − Low)/Close	63 days

Rows without sufficient historical data were dropped as part of the warm-up period.

4. Rule-Based Anomaly Detection

A rule-based detector was implemented as a transparent baseline.

A stock-day is flagged as anomalous if any of the following conditions are met:

|ret_z| > 2.5

vol_z > 2.5

range_pct > 95%

Anomaly Type Labels

crash: large negative return

spike: large positive return

volume_shock: unusually high trading volume

Multiple labels can co-exist (e.g., crash + volume_shock).

5. Clustering-Based Detection

To complement the rule-based approach, unsupervised clustering methods were used:

K-Means clustering on standardized features

Anomalies identified based on distance from cluster centroids

Threshold chosen to keep the anomaly rate within 2–8%

DBSCAN was applied on a representative subset

Used only for demonstration due to computational complexity

These methods help capture multivariate abnormal behavior.

6. Market-Level Anomaly Detection

To identify systemic market stress, stock-level signals were aggregated daily.

For each day:

Market return: mean of stock returns

Breadth: fraction of stocks with positive returns

A market day is flagged as anomalous when:

breadth < 0.3

This captures days where a large part of the market moves abnormally.

7. Required Outputs

The following outputs were generated as required by the capstone specification:

Daily Anomaly Card (daily_anomaly_card.csv)

Stock-level anomalies with reasons (why)

Market-Day Table (market_day_table.csv)

Market return, breadth, and market anomaly flag

Date Query (interactive)

Input a date to view market status and anomalous stocks

Monthly Mini-Report (monthly_mini_report.csv)

Summary of abnormal dates and their causes

8. Train / Validation / Test Setup

The dataset is split temporally:

Train: 2018

Validation: 2019

Test: 2020 (Q1)

Thresholds were fixed after validation to avoid look-ahead bias during testing.

9. Visualization

Visualizations were added to support interpretation:

Stock price plots with anomaly markers

Market-level anomaly timelines

These are used only after generating all required outputs.

10. Conclusion

This project shows that simple, interpretable, and leakage-free methods can effectively detect abnormal behavior in financial markets.
By combining rolling statistics, rule-based logic, and unsupervised clustering, both stock-level and market-level anomalies can be identified in a reliable and reproducible way.

11. Disclaimer

This project is created for educational purposes only and does not provide financial or investment advice.
