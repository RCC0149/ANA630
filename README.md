# SECOM Semiconductor Manufacturing Quality Analysis

**Author:** Randall C. Crawford
**Course:** ANA 630 – Advanced Analytic Applications

---

## Project Overview

This project analyzes the **UCI SECOM semiconductor manufacturing dataset** to identify sensor features predictive of **process failures** and improve defect detection in a complex, high-dimensional manufacturing environment. The dataset presents two major challenges: **extreme feature dimensionality** and **pervasive missing data**, both of which were treated as potential sources of signal rather than obstacles.

A central theme of this work is the idea that **missingness itself can be predictive**, reflecting sensor usage patterns, process changes, or operational regimes within the manufacturing line.

---

## Dataset Description

* **Samples:** 1,567 production entities
* **Features:** 590 anonymous sensor measurements
* **Target:** Binary pass/fail outcome
* **Missingness:**

  * ~91% of features contain missing values
  * Missing values form **distinct “layers”** (e.g., 1 NA, 715 NA, 794 NA, … up to 1,429 NA)

Because no sensor metadata is provided, all insight is derived from **statistical structure, usage patterns, and relationships to yield outcomes** .

---

## Key Contributions

### 1. Missingness as Signal (Not Noise)

* Grouped features into **22 missingness layers** based on NaN counts
* Used **chi-square testing** to identify features whose missingness was **predictive of failure**
* Created **missingness indicator variables** only where statistically justified
* Engineered a **late-utilization indicator** capturing process behavior shifts after sample index ~800
* Added a **per-sample proportion-of-missingness feature**

This layered view revealed temporal and structural patterns that would be lost under standard preprocessing.

---

### 2. Strategic, Layer-Aware Imputation

Instead of blanket imputation, a **tailored strategy** was applied:

* **Pass/Fail mean imputation** for layers with predictive missingness
* **Hybrid imputation** for late-utilization layers (mean before index 800, class-conditional after)
* Conversion of **invalid zero values to NaN** using distributional and skew-based criteria
* Removal of features with **insufficient information content** (extreme NA or zero dominance)

This preserved meaningful class separation while avoiding leakage or artificial smoothing.

---

### 3. Comprehensive Feature Selection Exploration

Seven fundamentally different feature-selection approaches were evaluated:

* SelectKBest
* Mutual Information
* Recursive Feature Elimination (Logistic Regression)
* Recursive Feature Elimination (XGBoost)
* Random Forest importance
* L1 Regularization (Lasso)
* Variance Threshold + SelectKBest

Each method was:

* Reduced to comparable feature sets
* Evaluated via **VIF analysis** and **backward elimination**
* Compared using **F1-score**, prioritizing performance on the rare failure class

---

### 4. Venn Diagrams & Feature Consensus Analysis

To move beyond single-method bias:

* Feature overlaps across methods were analyzed
* **Venn diagrams** were used to expose:

  * Core features selected consistently
  * Method-specific strengths
  * Tradeoffs between interpretability and performance

This synthesis step proved critical in identifying **robust, non-accidental predictors**.

---

### 5. Final Modeling with XGBoost

* Multiple feature sets were modeled using **XGBoost**
* Extensive **threshold tuning** and **GridSearchCV optimization**
* **Boruta-selected features** produced the strongest results:

  * Best balance of precision, recall, and F1-score
* Cross-validation was favored over a fixed train/test split due to dataset imbalance and size

---

## Outcomes & Takeaways

* Demonstrated that **missing data patterns can encode operational knowledge**
* Showed how **imputation strategy directly affects feature selection and model behavior**
* Highlighted the importance of **multi-method agreement** over single-metric optimization
* Produced a defect-detection model suitable for **manufacturing quality insight**, not just prediction

This project bridges **30+ years of manufacturing quality experience** (APQP, Six Sigma, TRIZ) with modern **machine learning and feature-selection techniques**, illustrating how advanced analytics can enhance real-world process understanding.

---
