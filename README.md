# Holistic Data Preparer
### Customer Credit Risk Dataset — End-to-End Data Preprocessing & Feature Engineering

---

## 🎥 Video Explanation

[![Watch the Video Explanation](https://img.shields.io/badge/Watch-Video%20Explanation-red?style=for-the-badge&logo=google-drive)](https://drive.google.com/file/d/1bj9HHx_JPZF76drkek2qOkaCSMEHT8BL/view?usp=sharing)

---

## Overview

This project implements a complete data preparation pipeline on a **Customer Credit Risk Dataset**. It covers every major stage of preprocessing — from raw data ingestion through missing value handling, outlier treatment, feature engineering, scaling, and transformation — producing a clean, model-ready CSV as the final deliverable.

---

## Project Structure

```
Holistic_Data_Preparer.ipynb       # Main notebook
customer_credit_risk_dataset.csv   # Raw input dataset
customer_credit_risk_FINAL.csv     # Cleaned output dataset (generated)
report.html                        # YData Profiling report (generated)
viz_missing_values.png             # Missing values bar chart & heatmap
viz_outliers_boxplot.png           # Before/after winsorization boxplots
viz_encoding_results.png           # Encoding summary charts
viz_transformations.png            # Transformation skewness effects
viz_final_summary.png              # Final pipeline summary dashboard
```

---

## Pipeline Stages

### Part B — Data Acquisition
Loads data from four distinct sources to demonstrate multi-source ingestion:
- **CSV** — primary customer credit risk dataset
- **JSON** — supplementary loyalty/VIP scores
- **SQL (SQLite)** — in-memory repayment history table
- **API (simulated)** — macroeconomic indicators (inflation rate, GDP growth, unemployment)

### Part C — Data Understanding & Cleaning
- `df.info()`, `df.describe()`, and null counts for initial exploration
- **YData Profiling** (`ProfileReport`) for automated EDA, exported as `report.html`
- **Missing value visualization** — horizontal bar chart (% missing per column) and a heatmap over 80 sampled rows

**Imputation strategies applied (Task 6):**
| Strategy | Applied To |
|---|---|
| Mean imputation (`SimpleImputer`) | `age` |
| Most-frequent imputation | `gender`, `employment_type` |
| Missing indicator + random sample | `annual_income` |
| KNN Imputer (k=5) | `credit_score`, `loan_amount` |
| MICE / Iterative Imputer | All remaining numeric columns |

### Part D — Outlier Handling
Outliers detected and handled on `annual_income`, `loan_amount`, and `credit_score`:
- **Z-Score** (threshold |z| > 3)
- **IQR Method** (1.5× IQR rule)
- **Percentile clipping** (5th–95th)
- **Winsorization** — visualized with before/after boxplots

### Part E — Feature Engineering

**Datetime features (Task 8):**  
Extracted from `join_date`: year, month, day, weekday, and `tenure_days`.

**Encoding (Tasks 9 & 10):**
| Method | Applied To |
|---|---|
| Ordinal Encoding | `education_level` |
| Label Encoding | `gender` |
| One-Hot Encoding | `region`, `loan_purpose` |
| Equal-width Binning | `annual_income`, `repayment_history` |
| Binarization (threshold=700) | `credit_score` → `good_credit` |
| Quantile Binning (quartiles) | `transaction_count` |
| K-Means Binning (4 clusters) | `transaction_count` |

### Part F — Feature Scaling (Task 11)
Four scalers applied and compared on `annual_income`, `loan_amount`, `credit_score`, `spending_ratio`, and `age`:
- `StandardScaler`
- `MinMaxScaler`
- `MaxAbsScaler`
- `RobustScaler`

### Part G — Feature Construction & Transformation (Task 12)
**Constructed features:**
- `debt_to_income` = loan amount / annual income
- `avg_monthly_trans` = transaction count / 6
- `spending_income_ratio` = spending ratio × income / 100
- `high_credit_risk` = flag for credit score < 600
- `loan_burden` = loan amount × repayment history

**Transformations applied:**
| Method | Applied To |
|---|---|
| Log (`log1p`) | `spending_ratio` |
| Square Root | `spending_ratio` |
| Box-Cox | `loan_amount` |
| Yeo-Johnson | `annual_income` |

### Part H — Final Deliverable
- Drops non-numeric/identifier columns (`customer_id`, `join_date`, raw categoricals, binned columns)
- Converts booleans to integers; label-encodes any remaining object columns
- Exports clean dataset as **`customer_credit_risk_FINAL.csv`**

---

## Requirements

```
pandas
numpy
matplotlib
seaborn
scikit-learn
scipy
ydata-profiling
```

Install with:
```bash
pip install pandas numpy matplotlib seaborn scikit-learn scipy ydata-profiling
```

---

## Usage

1. Place `customer_credit_risk_dataset.csv` in the same directory as the notebook.
2. Open `Holistic_Data_Preparer.ipynb` in Jupyter Notebook or JupyterLab.
3. Run all cells (`Kernel → Restart & Run All`).
4. Outputs generated:
   - `customer_credit_risk_FINAL.csv` — model-ready dataset
   - `report.html` — full profiling report
   - Four visualization PNG files (see Project Structure above)

---

## Visualizations

| File | Description |
|---|---|
| `viz_missing_values.png` | Bar chart + heatmap of missing values before imputation |
| `viz_outliers_boxplot.png` | Side-by-side boxplots before and after winsorization |
| `viz_encoding_results.png` | Summary charts for all encoding methods applied |
| `viz_transformations.png` | Histogram comparison of transformation effects on skewness |
| `viz_final_summary.png` | Pipeline summary dashboard — feature counts, data types, missing value reduction |
