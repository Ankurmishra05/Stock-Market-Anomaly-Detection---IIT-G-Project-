# 📊 Stock Market Anomaly Detection
Capstone Project

## 1. Introduction
In this project, I worked on detecting **unusual stock days and market stress days** using only **daily price and volume data**.  
The aim is to identify extreme behavior such as sharp price drops, spikes, unusually high trading volume, or wide intraday ranges — **without using labels, news, or predictive models**.

The focus of this project is on:
- Proper time-series handling  
- Avoiding data leakage  
- Building an explainable and reliable anomaly detection pipeline  

This project strictly follows the given **Capstone Project specification**.

---

## 2. Dataset
- **Source:** Kaggle – Daily NASDAQ Stock Market Dataset  
- **Link:** https://www.kaggle.com/datasets/jacksoncrow/stock-market-dataset  
- **Data:** Daily OHLCV CSV files (one per ticker)  
- **Price used:** Adjusted Close (accounts for splits and dividends)  
- **Period:** Data available up to April 2020  

A limited set of liquid NASDAQ stocks was selected to keep the analysis focused and interpretable.

---

## 3. Feature Engineering
All features are computed **per stock** using **only past data**, ensuring no look-ahead bias.

| Feature | Description | Window |
|-------|------------|--------|
| `return` | Daily percentage return | — |
| `ret_z` | Rolling z-score of returns | 63 days |
| `vol_z` | Rolling z-score of log volume | 21 days |
| `range_pct` | Percentile of (High − Low)/Close | 63 days |

Rows without sufficient historical data were removed as part of the warm-up process.

---

## 4. Rule-Based Anomaly Detection
A transparent **rule-based detector** is used as the baseline.

A stock-day is flagged as anomalous if **any** of the following holds:
- `|ret_z| > 2.5`
- `vol_z > 2.5`
- `range_pct > 95%`

### Anomaly Labels
- **crash** → large negative return  
- **spike** → large positive return  
- **volume_shock** → unusually high volume  

Labels can co-exist (e.g., `crash + volume_shock`).

---

## 5. Clustering-Based Detection
Unsupervised clustering is used to complement the rule-based approach.

- **K-Means**
  - Applied on standardized features
  - Anomalies detected using distance from centroids
  - Flag rate controlled between 2–8%

- **DBSCAN**
  - Applied on a representative subset
  - Used to demonstrate density-based anomaly detection
  - Limited use due to computational complexity

---

## 6. Market-Level Anomaly Detection
To detect **systemic market stress**, daily aggregation is performed.

For each trading day:
- **Market return:** mean of stock returns  
- **Breadth:** fraction of stocks with positive returns  

A day is flagged as a **market anomaly** when:


## 7. Required Outputs
The following outputs are generated as per the capstone requirements:

1. **Daily Anomaly Card (`daily_anomaly_card.csv`)**  
   - Stock-level anomalies with explanations

2. **Market-Day Table (`market_day_table.csv`)**  
   - Market return, breadth, and market anomaly flag

3. **Date Query (interactive)**  
   - Query any date to view market status and anomalous stocks

4. **Monthly Mini-Report (`monthly_mini_report.csv`)**  
   - Summary of abnormal dates and reasons

---

## 8. Train / Validation / Test Split
The dataset is split chronologically:
- **Train:** 2018  
- **Validation:** 2019  
- **Test:** 2020 (Q1)

Thresholds are fixed after validation to avoid look-ahead bias.

---

## 9. Visualization
Visual analysis includes:
- Stock price plots with anomaly markers  
- Market-level anomaly timelines  

All visualizations are generated after producing the required outputs.

---

## 10. Conclusion
This project demonstrates that **simple, interpretable, and leakage-free techniques** can reliably detect abnormal behavior in financial markets.  
By combining rolling statistics, rule-based logic, and unsupervised clustering, both stock-level and market-level anomalies are identified in a reproducible way.

---

## 11. Disclaimer
This project is for **educational purposes only** and does **not** constitute financial or investment advice.
