# Sneaker Resale Premium Analysis

Research project studying whether **brand identity, time since release, and buyer geography explain sneaker resale premiums**, and whether these signals are strong enough to support premium forecasting beyond descriptive analysis.

> **TL;DR** -- Off-White transactions carry much higher resale premiums than Yeezy transactions, premiums tend to decline as days since release increase, and major buyer regions differ in their premium distributions. The predictive part confirms the same structure: Ridge regression reaches **R2 = 0.671** for premium forecasting, while HistGradientBoostingClassifier reaches **ROC-AUC = 0.973** for high-premium classification.

---

## Overview

- **Window:** 2017-09-01 -> 2019-02-13
- **Dataset:** StockX Data Contest 2019 sneaker resale transactions
- **Raw sample:** 99,956 transaction rows
- **Working sample:** 97,116 rows after removing exact duplicates
- **Target:** resale premium over retail price, computed from sale price and retail price
- **Core features:** brand, sneaker model, sale price, retail price, shoe size, buyer region, order date, release date, and days since release
- **Modeling tasks:** continuous resale-premium prediction and high-premium transaction classification

---

## Research Hypotheses

The study tests three hypotheses at the 5% significance level.

| | Hypothesis | Verdict |
|---|---|---|
| **H1** | Off-White transactions have higher premium rates than Yeezy transactions. | Supported |
| **H2** | Resale premium is negatively related to days since release. | Supported |
| **H3** | Premium distributions differ across major buyer regions. | Supported |

---

## Methodology (in brief)

- **Data preparation:** price parsing, date parsing, duplicate removal, premium calculation, days-since-release calculation, and data-quality checks for missing values, outliers, and negative release gaps.
- **Statistical testing:** Welch two-sample t-test for the brand effect, Spearman rank correlation for the time effect, and Kruskal-Wallis test for regional premium differences.
- **Machine learning:** chronological 70/15/15 train-validation-test split, Ridge regression for premium forecasting, and HistGradientBoostingClassifier for high-premium classification.

---

## Key Results

**H1 -- Brand effect.** Off-White mean premium is **282.90%**, while Yeezy mean premium is **64.74%**. The Welch test rejects equality of means with a large gap of about **218 percentage points**, so the brand premium hypothesis is supported.

**H2 -- Time effect.** The Spearman correlation between premium and days since release is **rho = -0.1971** with a highly significant p-value. This supports the interpretation that resale premiums tend to decay over time after release.

**H3 -- Regional heterogeneity.** The Kruskal-Wallis test rejects equality of premium distributions across major buyer regions. This means geography is not interchangeable in the resale market: buyer-region demand conditions shift premium behavior.

**Prediction results.** The selected Ridge regression model reaches the following final holdout performance:

| Model | MAE | RMSE | R2 |
|---|---:|---:|---:|
| Ridge regression | 50.06 | 71.88 | 0.671 |

For the high-premium classification task, the selected HistGradientBoostingClassifier reaches **accuracy = 0.882**, **F1 = 0.811**, and **ROC-AUC = 0.973** on the final holdout sample.

---

## Repository Structure

```text
sneaker-resale-premium-analysis/
├── README.md
├── requirements.txt
├── notebooks/
│   └── final.ipynb
└── data/
    └── StockX-Data-Contest-2019-3 2.csv
```

---

## Data Sources

- **Resale transactions:** StockX Data Contest 2019 dataset, stored locally as `data/StockX-Data-Contest-2019-3 2.csv`
- **Analysis notebook:** `notebooks/final.ipynb`

---

## Author

**Shul Ilya** -- Group БПАД246, Faculty of Computer Science, HSE University
Supervisor: **Daria Bashminova**, Senior Lecturer
