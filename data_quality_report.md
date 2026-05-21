# Data Quality Report

# Overview

This report summarizes the data quality assessment conducted during the Exploratory Data Analysis (EDA) phase of the D2C Customer Churn Intelligence project.

The primary objective of this audit was to evaluate dataset reliability, identify potential analytical risks, validate schema consistency, and detect issues that could negatively impact churn analysis, customer segmentation, or downstream predictive modeling.

Special attention was given to:

- temporal data leakage risks,
- duplicate-like business records,
- missing-value behavior,
- outlier detection,
- join consistency,
- and feature reliability.

---

# Datasets Evaluated

The following datasets were audited:

| Dataset | Description |
|---|---|
| `customers.csv` | Customer demographic and acquisition information |
| `orders.csv` | Order-level transaction history |
| `support_tickets.csv` | Customer support interactions |
| `web_events_snapshot.csv` | Website and app engagement metrics |
| `churn_labels.csv` | Churn targets and dataset splits |
| `intervention_history.csv` | Previous retention campaign history |

---

# Snapshot Integrity & Leakage Prevention

## Snapshot Date

`2025-09-30`

## Critical Leakage Rule

The dataset contains post-snapshot order records extending into the churn target window (`2025-10-01` to `2025-11-29`).

Quantified finding:

- post-snapshot order rows (order_date > 2025-09-30): **1,872**

These rows exist only for churn-label generation and must not be used as model features or behavioral inputs.

## Leakage Prevention Actions Taken

- Order records dated after `2025-09-30` were identified separately.
- Post-snapshot transactions were excluded from feature-related analysis.
- Churn labels were treated strictly as future outcomes.
- Temporal consistency was maintained throughout exploratory analysis.

## Risk if Ignored

Using future transaction activity as input features would create severe target leakage and artificially inflate model performance, resulting in unrealistic churn predictions in production environments.

---

# Missing Value Analysis

## Customers Dataset

The following missing values were identified:

| Column | Missing Values | Observation |
|---|---:|---|
| `loyalty_tier` | 1386 | Likely indicates customers not enrolled in the loyalty program rather than random missingness |
| `skin_type` | 401 | Self-reported field; missingness may reflect incomplete onboarding behavior |

### Observations

- Missing `loyalty_tier` values appear behaviorally meaningful and should not automatically be treated as data corruption.
- Null `skin_type` values may indicate lower customer profile completion rates.

### Recommended Treatment

- Treat missing `loyalty_tier` values as `"Not Enrolled"` during segmentation or modeling.
- Retain `skin_type` nulls carefully or create a separate `"Unknown"` category.

---

## Orders Dataset

| Column | Missing Values | Observation |
|---|---:|---|
| `rating` | 80 | Customers did not leave ratings for some orders |

### Observations

Missing ratings may represent:

- disengaged customers,
- low post-purchase interaction,
- or customers unwilling to provide feedback.

### Recommended Treatment

- Avoid aggressive imputation of ratings.
- Preserve missingness as a potentially meaningful behavioral signal.

---

## Other Datasets

Minimal or no missing values were detected in:

- `support_tickets.csv`
- `web_events_snapshot.csv`
- `churn_labels.csv`
- `intervention_history.csv`

---

# Duplicate & Duplicate-Like Record Analysis

## Exact Duplicate Check

Exact duplicate row analysis was performed across all datasets.

| Dataset | Exact Duplicate Rows |
|---|---:|
| customers | 0 |
| orders | 0 |
| support_tickets | 0 |
| web_events_snapshot | 0 |
| churn_labels | 0 |
| intervention_history | 0 |

No exact row-level duplicates were identified.

---

## Duplicate-Like Business Records

Although exact duplicates were not detected, the `orders.csv` dataset contains intentionally designed duplicate-like records.

### Identified Issue

Some `order_id` values contain the suffix:

```text
_DUP
```

These records simulate real-world duplicate transaction scenarios.

### Quantified Findings (from audit)

- `_DUP` row count (in full orders): **12**
- `_DUP` revenue impact (sum of `gross_amount`): **₹7,928.05**
- Pre-snapshot rows removed due to `_DUP`: **9 rows** (revenue removed: **₹6,093.36**)

### Business Risk

If duplicate-like records are aggregated without validation:

- customer spend may be overstated (by **₹7,928.05** across `_DUP` rows),
- order frequency may become inflated,
- RFM segmentation may become distorted,
- and churn features may become unreliable.

### Recommended Treatment

- Investigate duplicate-like order behavior before aggregation.
- Validate whether duplicated orders represent retries, accidental duplication, or legitimate business events.
- Apply controlled deduplication logic if necessary.

---

# Datatype Validation

Date-related columns were successfully converted into datetime format:

| Dataset | Date Columns |
|---|---|
| customers | `signup_date` |
| orders | `order_date` |
| support_tickets | `ticket_date` |
| web_events_snapshot | `snapshot_date` |
| churn_labels | `snapshot_date` |
| intervention_history | `snapshot_date` |

Numeric columns such as:

- `gross_amount`,
- `discount_pct`,
- `delivery_days`,
- `resolution_hours`,
- and engagement metrics

were validated successfully.

---

# Outlier Detection

## Gross Amount Outliers

The `gross_amount` column in `orders.csv` contains several unusually high transaction values.

### Observed Behavior

- Most transactions fall within a normal D2C purchase range.
- A small number of orders exceed expected consumer spending patterns.

### Potential Explanations

Possible reasons include:

- bulk purchases,
- premium product combinations,
- corporate gifting,
- or anomalous transactions.

### Business Risk

Extreme order values may:

- distort average revenue calculations,
- bias monetary segmentation,
- influence model scaling,
- and impact churn-risk interpretation.

### Recommended Treatment

- Investigate high-value orders before removal.
- Consider winsorization, percentile capping, or log transformation during modeling.
- Quantified capping reference used in audit: gross_amount capped at **p99 = 2,343**.
- Avoid removing legitimate high-value customers without business validation.

---

# Join Consistency Validation

The datasets were validated using `customer_id` as the primary join key.

## Observations

- Customer IDs were consistently formatted across datasets.
- The customer universe remained stable at 2,400 customers.
- Missing support-ticket records for some customers were expected and not treated as join failures.

## Join Strategy Used

All business datasets were treated as left joins from:

```text
customers.csv
```

This ensured full customer coverage during analysis.

---

# Data Consistency Observations

Additional validation checks confirmed:

- category labels were consistently formatted,
- acquisition channels were standardized,
- churn labels aligned correctly with customer IDs,
- and engagement metrics contained valid numeric ranges.

No major schema corruption issues were detected.

---

# Feature & Modeling Risks Identified

| Risk Area | Potential Impact |
|---|---|
| Post-snapshot leakage | Artificially inflated model performance |
| Duplicate-like orders | Distorted spend and frequency metrics |
| Missing loyalty tiers | Segmentation bias |
| Extreme revenue outliers | Skewed monetary analysis |
| Missing ratings | Reduced satisfaction visibility |
| Sparse engagement behavior | Noisy churn interpretation |

---

# Overall Assessment

The datasets were found to be suitable for exploratory analysis, customer segmentation, and predictive churn modeling.

However, several important analytical risks require careful handling:

- temporal leakage prevention,
- duplicate-like transaction validation,
- outlier treatment,
- and meaningful handling of behavioral missingness.

The datasets provide strong behavioral, transactional, and engagement signals capable of supporting advanced churn analysis when handled with appropriate business and modeling safeguards.