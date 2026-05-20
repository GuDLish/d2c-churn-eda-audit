# D2C Customer Churn Intelligence — Part 1: Data Audit, EDA & Business Understanding

## Business Problem

A Direct-to-Consumer (D2C) personal-care brand wants to reduce customer churn without relying on blanket discount campaigns that reduce profitability.

The company needs a data-backed understanding of:

- which customers are most likely to churn,
- what behavioral patterns indicate churn risk,
- which acquisition channels produce lower-retention customers,
- how customer support experiences affect retention,
- and where retention efforts should be prioritized before building predictive models.

This repository focuses on the **Exploratory Data Analysis (EDA)** and **business understanding** phase of the churn intelligence pipeline.

---

# Project Objectives

The primary objectives of this project are to:

- audit and validate raw business datasets,
- identify data-quality and modeling risks,
- prevent temporal data leakage,
- investigate customer behavior patterns,
- analyze churn-related business signals,
- generate evidence-backed churn hypotheses,
- and provide strategic recommendations for retention planning.

---

# Dataset Overview

The project uses multiple business datasets covering customer profiles, transaction history, support interactions, digital engagement behavior, and churn outcomes.

| Dataset | Description |
|---|---|
| `customers.csv` | Customer demographic and acquisition information |
| `orders.csv` | Order-level transaction history |
| `support_tickets.csv` | Customer support interactions and sentiment |
| `web_events_snapshot.csv` | Website/app engagement metrics |
| `churn_labels.csv` | Churn target labels and train/validation/test splits |
| `intervention_history.csv` | Previous retention campaign history |

---

# Snapshot Integrity & Leakage Prevention

This project follows strict temporal integrity rules to prevent data leakage.

## Snapshot Date

`2025-09-30`

## Churn Target Window

`2025-10-01` → `2025-11-29`

## Leakage Prevention Rules Applied

- Only data available on or before the snapshot date was used for behavioral analysis and future modeling preparation.
- Post-snapshot orders were identified and excluded from feature-related analysis.
- Churn labels were treated as future outcomes and never used as input features.

This ensures that future information does not unintentionally influence churn analysis or downstream predictive modeling.

---

# Key Analytical Areas Covered

## Data Quality Audit

The project includes a detailed audit covering:

- missing values,
- duplicate-like business records,
- datatype validation,
- outlier detection,
- join consistency validation,
- and feature reliability assessment.

Special attention was given to:

- duplicate-like order IDs (`_DUP`),
- missing customer ratings,
- loyalty enrollment null behavior,
- and unusually high transaction values.

---

## Exploratory Data Analysis (EDA)

The EDA phase investigates:

- customer acquisition quality,
- churn distribution,
- order and revenue behavior,
- support-ticket dissatisfaction patterns,
- digital engagement decline,
- campaign engagement trends,
- and customer inactivity signals.

---

## Churn-Risk Hypothesis Investigation

The project evaluates multiple evidence-backed churn hypotheses, including:

- low-engagement customers are more likely to churn,
- customers with negative support experiences show higher churn risk,
- acquisition channels vary significantly in retention quality,
- highly inactive customers exhibit elevated churn behavior,
- and customers with repeated abandoned-cart behavior may require intervention.

Each hypothesis is supported using visual analysis and dataset-driven reasoning.

---

# Major Business Insights

Some key findings from the analysis include:

- Overall churn levels are operationally significant and require proactive retention planning.
- Organic and referral customers demonstrate stronger long-term retention behavior than some paid acquisition channels.
- Support dissatisfaction signals such as negative sentiment and ticket reopening behavior may correlate with elevated churn risk.
- Engagement decay indicators such as low sessions and high inactivity appear strongly associated with churn behavior.
- Certain acquisition channels may generate customer volume but weaker long-term retention quality.

---

# Repository Structure

```bash
d2c-churn-eda-audit/
│
├── eda_audit.ipynb
├── data_quality_report.md
├── business_memo.md
├── README.md
├── requirements.txt
```

---

# Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Jupyter Notebook

---

# Installation & Setup

## Clone Repository

```bash
git clone https://github.com/GuDLish/d2c-churn-eda-audit.git
```

## Install Dependencies

```bash
pip install -r requirements.txt
```

## Run Notebook

Open the notebook:

```bash
eda_audit.ipynb
```

using:

- Jupyter Notebook
- VS Code
- or JupyterLab

---

# Outputs Included

This repository contains:

- exploratory analysis notebook,
- business-focused EDA,
- churn-risk investigations,
- data quality assessment,
- visual analysis,
- and strategic retention recommendations.

---

# Future Scope

This project serves as the foundation for later stages of the churn intelligence pipeline:

- RFM customer segmentation,
- churn prediction modeling,
- retention prioritization,
- and FastAPI deployment for churn scoring services.

---

# Author

Prateek Parmar