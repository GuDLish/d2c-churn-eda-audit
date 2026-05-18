# Data Quality Report

## Overview

This report summarizes the data quality assessment performed during the exploratory data analysis (EDA) phase of the D2C customer churn project.

The objective was to inspect dataset reliability, identify missing values, validate datatypes, and detect duplicate records.

---

## Datasets Evaluated

The following datasets were analyzed:

- customers.csv
- orders.csv
- support_tickets.csv
- web_events_snapshot.csv
- churn_labels.csv
- intervention_history.csv

---

## Missing Value Analysis

### Customers Dataset

Missing values identified:

- loyalty_tier → 1386 missing values
- skin_type → 401 missing values

Other columns contained no missing values.

---

### Orders Dataset

Missing values identified:

- rating → 80 missing values

Other columns contained no missing values.

---

### Other Datasets

Most datasets showed minimal or no missing values.

---

## Duplicate Record Analysis

Duplicate row checks were performed across all datasets.

Results:

- customers → 0 duplicate rows
- orders → 0 duplicate rows
- support_tickets → 0 duplicate rows
- web_events_snapshot → 0 duplicate rows
- churn_labels → 0 duplicate rows
- intervention_history → 0 duplicate rows

No major duplication issues were detected.

---

## Datatype Validation

Date columns were successfully converted to datetime format:

- signup_date
- order_date

Numeric columns such as quantity, revenue, discount percentage, and delivery days were validated successfully.

---

## Data Consistency Observations

- Customer IDs were consistently formatted across datasets.
- Revenue-related columns contained valid numeric values.
- Category labels were properly structured.

---

## Risks Identified

Potential risks affecting analysis quality:

- Missing loyalty tier information may reduce segmentation accuracy.
- Missing customer ratings may affect customer satisfaction analysis.

---

## Conclusion

Overall, the datasets were found to be suitable for exploratory data analysis and future predictive modeling tasks.

Minor missing value issues exist but do not critically affect the analysis process.