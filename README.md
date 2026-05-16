# Academic Reports and personal Projects.


# STAT 547 — Data Mining & Statistical Learning

Coursework from STAT 547 (Data Mining), covering supervised learning,
high-dimensional statistics, and advanced machine learning. All analyses
are implemented in R with reproducible RMarkdown reports.

---

## Projects

### 1 — Foundations: kNN & Discriminant Analysis
Visualization of the bias-variance tradeoff as a function of k.
Side-by-side comparison of k-NN and LDA decision boundaries on
overlapping Gaussian classes.  
**Tools:** `kknn`, `MASS`, `ggplot2`

---

### 2 — High-Dimensional Classification
**Dataset 1 — Prostate Cancer Genomics** (n ≪ p = 500 gene features):
Compared LDA, QDA, Naive Bayes, and SVM over 50 stratified random
splits. Analyzed rank deficiency, pooled vs. sample covariance structure,
and multivariate normality via Mardia's test.

**Dataset 2 — Spam Detection** (`MASS::spam`): Same classifier suite
with near-zero variance filtering and overfitting diagnostics.  
**Tools:** `MASS`, `e1071`, `pROC`, `MVN`, `fields`

---

### 3 — Regression & Image Classification

**Regression — `datafls`:** Benchmarked regression trees, Random Forest,
GBM, and Elastic Net on economic cross-section data. Evaluated with RMSE,
MAE, and R².

**Classification — MNIST (60,000 training / 10,000 test):** 10-class
handwritten digit recognition. Pipeline: PCA dimensionality reduction
(87 PCs → 90% variance), feature engineering (geometric moments,
centroids), model comparison (SVM RBF, Random Forest, GBM), and a
novel **Hybrid PCA + RF** approach. Includes Cover's Theorem
probability curve.  
Best test accuracy: **SVM RBF on PCA — 96.25%**  
**Tools:** `dslabs`, `e1071`, `randomForest`, `gbm`, `glmnet`, `pdp`

---

### 4 — Advanced Machine Learning

**Exercise 2 — Financial Fraud Detection** (n=10,000, p=50, severe class
imbalance): Engineered polynomial and interaction features, applied SMOTE
and inverse-frequency weighting. Trained Elastic Net, RF, XGBoost, SVM
(RBF), and MLP via Bayesian hyperparameter optimization. Uncertainty
quantified using three split-conformal prediction methods.

**Exercise 3 — Proteomics Cancer Regression** (p=200 > n, rank
deficient): Applied Ridge, Lasso, Elastic Net, Adaptive Lasso, SCAD,
Group Lasso, Horseshoe, and Spike-and-Slab. Variable selection
reproducibility assessed via pairwise Jaccard similarity across methods.

**Exercise 4 — Causal ML & Interpretable AI:** Implemented Double
ML / Debiased ML framework with cross-fitting to remove regularization
bias in causal effect estimation. Applied Causal Forests (heterogeneous
treatment effects) to fraud data. Used Knockoff Filters for
FDR-controlled variable selection on proteomics data.  
**Tools:** `grf`, `xgboost`, `nnet`, `glmnet`, `ncvreg`, `knockoff`,
`BAS`, `smotefamily`, `rBayesianOptimization`

---

## Structure

```
1-foundations/
2-high-dim-classification/
3-regression-and-mnist/
4-advanced/
```

## Requirements

All reports knit with R ≥ 4.3. Package installation lines are included
(commented) at the top of each `.Rmd` file.


# Google Trends Time Series Analysis
### SARIMA Forecasting — Taco & S&P 500 Search Interest

> **STAT 335 · Time Series Analysis · RIT, Spring 2026**  
> Ducheng Tan · Alexander Beckford

---

## Overview

This project applies **Seasonal ARIMA (SARIMA)** modeling to monthly Google
Trends search interest data for three keywords — *Bitcoin*, *S&P 500*, and
*Taco* — from January 2021 through April 2026.

The dataset is embedded directly in the RMarkdown source (no external files
required), making this project **fully self-contained and reproducible**.

---

## Results

| Series | Best Model | Forecast Coverage |
|--------|------------|:-----------------:|
| Taco ($T_t$) | ARIMA$(1,0,0)\times(1,0,1)_{12}$ | 4/4 actuals within 95% CI |
| log S&P 500 ($L_t$) | ARIMA$(0,1,2)$ | 4/4 actuals within 95% CI |

- All four January–April 2026 realized values fell within the model's 95%
  forecast intervals, validating reasonable out-of-sample predictive performance.
- Actuals consistently sat near the upper bound of the Taco CI, suggesting a
  mild structural uptick in search interest not captured in the 2021–2025
  training window.

---

## Methods

### Model Selection
- ACF / PACF diagnostics on raw and differenced series
- Candidate model grid evaluated by **AIC**, **BIC**, and Ljung-Box residual tests
- Log transform applied to S&P 500 to stabilise variance

### Theoretical Derivations

**Taco — ARIMA$(1,0,0)\times(1,0,1)_{12}$:**

$$T_{n+m}^n = \alpha + \phi_1 T_{n+m-1}^n + \Phi_1 T_{n+m-12} - \phi_1\Phi_1 T_{n+m-13} + \Theta_1 w_{n+m-12}$$

MSPE (bounded, geometric):
$$p_{n+m}^n = \sigma_w^2\!\left(\frac{1-\phi_1^{2m}}{1-\phi_1^2}\right), \quad m \leq 12$$

**S&P 500 — ARIMA$(0,1,2)$ on $L_t = \log S_t$:**

$$L_{n+m}^n = \begin{cases} \alpha + L_n + \theta_1 w_n + \theta_2 w_{n-1} & m=1 \\ \alpha + L_{n+1}^n + \theta_2 w_n & m=2 \\ \alpha + L_{n+m-1}^n & m \geq 3 \end{cases}$$

MSPE (grows linearly in $m$ — unit root effect):
$$p_{n+m}^n = \sigma_w^2\!\Big[1 + (1+\theta_1)^2\cdot\mathbf{1}_{m\geq 2} + (m-2)(1+\theta_1+\theta_2)^2\cdot\mathbf{1}_{m\geq 3}\Big]$$

The contrasting MSPE growth rates reflect a key structural difference: the
stationary Taco model has bounded forecast uncertainty, while the non-stationary
(differenced) S&P 500 model accumulates uncertainty linearly with horizon.

---


## Requirements

```r
install.packages(c("astsa", "ggplot2", "dplyr", "tidyr",
                   "lubridate", "knitr", "kableExtra"))
```

R ≥ 4.3 recommended.

---

## Data Source

[Google Trends](https://trends.google.com/trends/) — relative search interest
(0–100 scale) within the United States, monthly frequency.  
Training window: 2021-01 to 2025-12 · Hold-out: 2026-01 to 2026-04.

---

## References

1. Shumway, R. H., & Stoffer, D. S. (2019). *Time Series: A Data Analysis
   Approach Using R.* Chapman and Hall/CRC.
2. Google Trends. <https://trends.google.com/trends/> (accessed April 20, 2026).

