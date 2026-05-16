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
hw1-foundations/
hw2-high-dim-classification/
hw3-regression-and-mnist/
hw4-advanced/
```

## Requirements

All reports knit with R ≥ 4.3. Package installation lines are included
(commented) at the top of each `.Rmd` file.
