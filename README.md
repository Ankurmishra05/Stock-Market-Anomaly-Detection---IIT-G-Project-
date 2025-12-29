.

📊 Stock Market Anomaly Detection

Capstone Project — Educational Analytics

📌 Overview

This project implements a leakage-free, unsupervised anomaly detection framework to identify unusual stock days and market stress days using only daily price and volume data.
No labels, no news, and no complex predictive models are used.

The goal is to detect:

Stock-level anomalies (crashes, spikes, volume shocks)

Market-level stress events (systemic abnormal behavior)

This project follows the official Capstone Project specification exactly.

🎯 Motivation

Financial markets occasionally experience extreme movements driven by panic, liquidity shocks, or systemic risk.
Detecting such anomalies helps with:

Market monitoring

Risk diagnostics

Understanding abnormal behavior without hindsight bias

This project emphasizes correct time-series hygiene and interpretability.

📂 Dataset

Source: Kaggle — Daily NASDAQ Stock Market Data

Link: https://www.kaggle.com/datasets/jacksoncrow/stock-market-dataset

Data: Daily OHLCV CSVs (one per ticker)

Period: Up to April 2020

Price used: Adjusted Close (to account for splits/dividends)

🧩 Feature Engineering (Leakage-Safe)

Features are computed per ticker, using only past data:

Feature	Description	Rolling Window
return	Daily percentage return	—
ret_z	Return z-score	63 days
vol_z	Log-volume z-score	21 days
range_pct	Percentile of (High–Low)/Close	63 days

Warm-up periods are handled by dropping insufficient history rows.

🚨 Rule-Based Anomaly Detection

A stock-day is flagged as unusual if any of the following triggers:

|ret_z| > 2.5

vol_z > 2.5

range_pct > 95%

Semantic Labels

crash → large negative return

spike → large positive return

volume_shock → unusually high volume
(Labels may co-exist, e.g., crash + volume_shock)

🧠 Clustering-Based Detection (Unsupervised)

K-Means on standardized features (ret_z, vol_z, range_pct)

Anomalies identified using distance from centroid

Flag rate controlled within 2–8%

DBSCAN applied on a representative subset to demonstrate density-based detection

These methods complement the rule-based baseline.

🌍 Market-Level Anomaly Detection

For each trading day:

Market return: mean of stock returns

Breadth: fraction of stocks with positive returns

A market anomaly is flagged when:

breadth < 0.3

This captures systemic stress days.

📤 Required Outputs (Generated)

The project produces the following mandatory outputs:

Daily Anomaly Card (daily_anomaly_card.csv)

Stock-level anomalies with explanations

Market-Day Table (market_day_table.csv)

Daily market return, breadth, and stress flag

Date Query (interactive)

Input a date → market status + anomalous tickers

Monthly Mini-Report (monthly_mini_report.csv)

Summary of abnormal dates and causes

📈 Visual Analysis

Stock price series with anomaly markers

Market anomaly timeline

Visuals are used only after all required outputs are generated.

🧪 Train / Validation / Test Protocol

Train: 2018

Validation: 2019 (threshold calibration)

Test: 2020 Q1
Thresholds are fixed after validation to avoid look-ahead bias.

🔁 Reproducibility

Fully deterministic pipeline

No random labels or external signals

Dataset and parameters clearly documented

Anyone can rerun the notebook and reproduce results.

⚠️ Disclaimer

This project is for educational and analytical purposes only.
It does not constitute financial or investment advice.

🧠 Key Takeaway

This project demonstrates how simple, interpretable, and leakage-free techniques can reliably identify abnormal behavior in financial markets without complex models or hindsight bias.
