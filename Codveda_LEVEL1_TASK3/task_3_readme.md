# Task 3: Exploratory Data Analysis (EDA)
**Codveda Technologies Data Science Internship — Level 1 (Basic)**

## Overview
A statistical and visual exploratory analysis of the sanitized Iris dataset (`codveda_level1_task2_preprocessed.csv`, $N = 146$). This module verifies post-cleaning distributional symmetry, quantifies pairwise linear correlations across morphological features, and identifies the primary predictors driving species separation.

## Objectives
- Validate post-standardization summary statistics and Fisher-Pearson skewness across all four numerical predictors
- Compute a full Pearson correlation matrix to detect multicollinearity and target-feature relationships
- Render publication-quality diagnostic visualizations (correlation heatmap and standardized boxplots) using Seaborn and Matplotlib

## 1. Univariate Distributional Profile
Post-scaling verification confirmed zero-centered distributions ($\mu \approx 0.00$, $\sigma = 1.0034$) with skewness coefficients comfortably inside the $[-0.5, +0.5]$ symmetry range:

| Feature | Median (50%) | IQR (25% – 75%) | Min / Max (Z-Score) | Skewness |
|---|---|---|---|---|
| `sepal_length` | -0.0684 | -0.9105 to +0.6534 | -1.8729 / +2.4580 | +0.2784 (Mild Right) |
| `sepal_width` | -0.0939 | -0.6018 to +0.6679 | -2.1255 / +2.4455 | +0.1408 (Symmetric) |
| `petal_length` | +0.3383 | -1.2607 to +0.7381 | -1.6033 / +1.7660 | -0.3236 (Mild Left) |
| `petal_width` | +0.1058 | -1.2139 to +0.7656 | -1.4779 / +1.6894 | -0.1488 (Symmetric) |

## 2. Multivariate Correlation Findings

| Feature | `sepal_length` | `sepal_width` | `petal_length` | `petal_width` | `species_encoded` |
|---|---|---|---|---|---|
| `sepal_length` | 1.00 | -0.11 | 0.88 | 0.82 | 0.79 |
| `sepal_width` | -0.11 | 1.00 | -0.40 | -0.33 | -0.40 |
| `petal_length` | 0.88 | -0.40 | 1.00 | 0.96 | 0.95 |
| `petal_width` | 0.82 | -0.33 | 0.96 | 1.00 | 0.96 |
| `species_encoded` | 0.79 | -0.40 | 0.95 | 0.96 | 1.00 |

## Key Insights
- **Petal measurements carry the strongest discriminative signal:** `petal_width` ($r = 0.96$) and `petal_length` ($r = 0.95$) correlate near-linearly with `species_encoded`, making them the primary drivers for separating *setosa*, *versicolor*, and *virginica*.
- **Strong predictor multicollinearity:** `petal_length` and `petal_width` exhibit a Pearson correlation of $r = 0.9618$, and both correlate strongly with `sepal_length` ($r = 0.88$ and $r = 0.82$). While decision-tree ensembles handle this collinearity naturally, linear classifiers would benefit from L2 regularization or PCA dimensionality reduction.
- **Inverse sepal-width relationship:** `sepal_width` correlates negatively with `petal_length` ($r = -0.40$) and `species_encoded` ($r = -0.40$), reflecting that *setosa* specimens combine broader sepals with compact petals.

## Data Quality Notes & Limitations
- Pearson correlation captures strictly linear monotonic relationships; non-linear class boundaries across species pairs should be inspected further using class-stratified pairplots or kernel density estimates before model selection.

## Tech Stack
Python 3.12 (Anaconda AI Kernel), Pandas, NumPy, Matplotlib, Seaborn

## Repository Structure
```text
├── data/
│   └── codveda_level1_task2_preprocessed.csv
├── notebooks/
│   └── task3_exploratory_data_analysis.ipynb
├── reports/
│   └── Task3_Exploratory_Data_Analysis_Report.docx
├── docs/
│   └── task3_eda_charts.png
└── README.md
```

## Key Takeaway
Combining non-parametric outlier filtering with Z-score standardization yielded symmetric, well-behaved feature distributions and revealed clear, interpretable morphological structure—establishing an empirical baseline before introducing any machine learning complexity.

## Author
**Ulasi Chukwuma Obinna**  
Data Analyst | Business Intelligence & Data Science  
Email: ulasistanley1@gmail.com  
GitHub: https://github.com/Krystalkrys001  
Medium: https://medium.com/@ulasistanley1  
LinkedIn: https://www.linkedin.com/in/chukwuma-obinna-ulasi-082198413/