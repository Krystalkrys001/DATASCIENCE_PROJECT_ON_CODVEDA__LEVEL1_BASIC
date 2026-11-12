# Task 2: Data Cleaning & Preprocessing Pipeline
**Codveda Technologies Data Science Internship — Level 1 (Basic)**

## Overview
A structured data hygiene and preprocessing pipeline built on the Iris dataset (`1) iris.csv`, 150 observations across 4 continuous morphological features and 1 categorical species target). The workflow audits schema integrity, filters statistical outliers using a non-parametric Interquartile Range (IQR) rule, standardizes numerical predictors to zero mean and unit variance, and encodes categorical class labels into machine-readable integers.

## Objectives
- Audit raw schema completeness, data types, and summary distributions before applying transformations
- Detect and remove extreme outliers in `sepal_width` using the $1.5 \times \text{IQR}$ fence rule
- Standardize continuous features via `StandardScaler` and encode `species` labels via `LabelEncoder` without introducing statistical distortion

## 1. Dataset Audit & Outlier Removal (IQR Method)
Initial inspection of `1) iris.csv` confirmed 150 complete rows with 0 missing values across `sepal_length`, `sepal_width`, `petal_length`, `petal_width`, and `species`. Summary statistics revealed visible tail spread in `sepal_width` (2.0 cm to 4.4 cm), motivating non-parametric outlier filtering before scaling:

| Parameter | Value | Calculation |
|---|---|---|
| First Quartile ($Q_1$) | 2.80 cm | 25th percentile of `sepal_width` |
| Third Quartile ($Q_3$) | 3.30 cm | 75th percentile of `sepal_width` |
| Interquartile Range ($\text{IQR}$) | 0.50 cm | $Q_3 - Q_1$ |
| Lower Bound | 2.05 cm | $Q_1 - 1.5 \times \text{IQR}$ |
| Upper Bound | 4.05 cm | $Q_3 + 1.5 \times \text{IQR}$ |

Filtering records outside `[2.05 cm, 4.05 cm]` removed 4 extreme observations (one at 2.0 cm and three at 4.1, 4.2, and 4.4 cm), leaving **146 clean records** (97.33% retention).

## 2. Feature Standardization (`StandardScaler`)
Even though all four predictors are measured in centimeters, `petal_length` ($\sigma = 1.76\text{ cm}$) carries more than four times the variance of `sepal_width` ($\sigma = 0.43\text{ cm}$). Applying `StandardScaler` after outlier removal centers every feature at $\mu = 0.00$ with unit variance ($\sigma = 1.00$), preventing higher-variance columns from dominating distance-based calculations downstream.

## 3. Categorical Encoding (`LabelEncoder`)
The target column `species` was mapped into discrete integer classes while retaining the original text label alongside it for transparent exploratory verification:

| Species | Encoded Class | Cleaned Record Count |
|---|---|---|
| `setosa` | `0` | 47 |
| `versicolor` | `1` | 49 |
| `virginica` | `2` | 50 |

## Data Quality Notes & Leakage Prevention
- **Order of operations matters:** Outlier removal was executed *before* fitting `StandardScaler` so extreme values did not inflate the mean and standard deviation used to compute z-scores.
- **Production ML boundary:** In a supervised train/test modeling workflow, both IQR fence thresholds and `StandardScaler` parameters must be fitted strictly on the training set and applied to the test set to avoid data leakage.

## Tech Stack
Python 3.12 (Anaconda AI Kernel), Pandas, NumPy, Scikit-learn (`StandardScaler`, `LabelEncoder`)

## Repository Structure
```text
├── data/
│   ├── 1) iris.csv
│   └── codveda_level1_task2_preprocessed.csv
├── notebooks/
│   └── task2_data_preprocessing.ipynb
├── reports/
│   └── Task2_Data_Cleaning_Preprocessing_Report.docx
└── README.md
```

## Key Takeaway
Clean preprocessing is not just calling `.fit_transform()`—it is getting the sequence right (audit $\rightarrow$ outlier filtering $\rightarrow$ scaling $\rightarrow$ encoding) so downstream statistics and models reflect genuine structure rather than outlier distortion.

## Author
**Ulasi Chukwuma Obinna**  
Data Analyst | Business Intelligence & Data Science  
Email: ulasistanley1@gmail.com  
GitHub: https://github.com/Krystalkrys001  
Medium: https://medium.com/@ulasistanley1  
LinkedIn: https://www.linkedin.com/in/chukwuma-obinna-ulasi-082198413/