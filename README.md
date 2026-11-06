# Missing Data Analysis in R: Comparing Imputation Methods Under MCAR, MAR and MNAR Mechanisms

## Overview

This project investigates the statistical consequences of missing data and evaluates the performance of different missing-data handling techniques in R. Using simulated datasets with controlled Missing Completely at Random (MCAR), Missing at Random (MAR) and Missing Not at Random (MNAR) mechanisms, the project compares complete-case analysis, mean imputation, regression imputation and multiple imputation.

The implementation includes Rubin’s rules for combining estimates and standard errors from multiple imputed datasets, implemented from scratch and compared with established R packages where appropriate.

The central objective is to determine how missing-data mechanisms and imputation choices affect parameter estimates, statistical bias, uncertainty and confidence interval coverage.

## Research Objectives

1. Simulate datasets under MCAR, MAR and MNAR missingness mechanisms.
2. Quantify the proportion and patterns of missing observations under each mechanism.
3. Implement and compare four missing-data handling approaches.
4. Derive and implement Rubin’s rules for combining multiple-imputation estimates.
5. Evaluate estimator bias, variance, confidence interval coverage and interval width.
6. Investigate the conditions under which common imputation methods produce misleading statistical conclusions.

## Missingness Mechanisms

The simulation distinguishes three mechanisms:

- **MCAR (Missing Completely at Random):** Missingness is independent of both observed and unobserved data.
- **MAR (Missing at Random):** Missingness depends on observed variables but not on the missing value itself, conditional on observed information.
- **MNAR (Missing Not at Random):** Missingness depends on the unobserved value, even after accounting for observed information.

## Methods Compared

### 1. Complete-Case Analysis

Observations containing missing values are excluded from the analysis. The resulting estimates are evaluated for bias, precision and information loss under each missingness mechanism.

### 2. Mean Imputation

Missing values are replaced with the observed sample mean. The analysis examines how this approach affects variance estimates, standard errors and confidence interval coverage.

### 3. Regression Imputation

Missing values are predicted using regression models fitted to observed data. The project evaluates the consequences of treating predicted values as if they were observed, particularly the resulting underestimation of uncertainty.

### 4. Multiple Imputation

Multiple plausible values are generated for each missing observation, producing several completed datasets. The analysis estimates the target parameter independently in each dataset before combining the results.

Rubin’s rules are implemented from scratch:

Let \(Q_m\) denote the estimate from imputed dataset \(m\), and \(U_m\) its estimated variance.

The pooled estimate is:

\[
\bar Q=\frac{1}{M}\sum_{m=1}^{M}Q_m
\]

The average within-imputation variance is:

\[
\bar U=\frac{1}{M}\sum_{m=1}^{M}U_m
\]

The between-imputation variance is:

\[
B=\frac{1}{M-1}\sum_{m=1}^{M}(Q_m-\bar Q)^2
\]

The total variance is:

\[
T=\bar U+\left(1+\frac{1}{M}\right)B
\]

The pooled standard error is:

\[
SE(\bar Q)=\sqrt{T}
\]

Confidence intervals are constructed using an appropriate degrees-of-freedom adjustment.

## Simulation Design

The simulation uses repeated datasets generated from a known statistical model, allowing estimated parameters to be compared against their true values.

The experimental design varies:

- Missingness mechanism: MCAR, MAR and MNAR.
- Missing-data proportion.
- Sample size.
- Number of imputations.
- Strength of association between explanatory variables and missingness.

Each method is evaluated using the same underlying simulated datasets to support fair comparisons.

## Evaluation Metrics

| Metric | Purpose |
|---|---|
| Bias | Measures systematic deviation from the true parameter. |
| Empirical variance | Measures variability across simulation repetitions. |
| Root Mean Squared Error (RMSE) | Combines squared bias and variance. |
| Confidence interval coverage | Measures how often intervals contain the true parameter. |
| Average interval width | Evaluates the precision of uncertainty estimates. |
| Convergence and failure rate | Identifies computational or estimation problems. |

## Expected Findings and Interpretation

The project investigates whether:

- Complete-case analysis performs adequately under MCAR but becomes biased under certain MAR and MNAR conditions.
- Mean imputation distorts variance and produces misleading standard errors.
- Regression imputation improves point predictions but understates uncertainty when imputation uncertainty is ignored.
- Multiple imputation provides better uncertainty quantification under appropriately specified MCAR and MAR models.
- Standard multiple-imputation procedures may remain biased under MNAR unless additional assumptions or sensitivity analyses are introduced.

These are hypotheses to be assessed through simulation, not results assumed in advance.

## Repository Structure

```text
missing-data-analysis/
│
├── README.md
├── LICENSE
├── renv.lock
│
├── R/
│   ├── 01_data_generation.R
│   ├── 02_missingness_mechanisms.R
│   ├── 03_complete_case.R
│   ├── 04_mean_imputation.R
│   ├── 05_regression_imputation.R
│   ├── 06_multiple_imputation.R
│   ├── 07_rubins_rules.R
│   ├── 08_evaluation_metrics.R
│   └── 09_simulation_experiments.R
│
├── tests/
│   ├── test_missingness.R
│   ├── test_imputation.R
│   └── test_rubins_rules.R
│
├── results/
│   ├── simulation_results.csv
│   └── summary_metrics.csv
│
├── figures/
│   ├── bias_comparison.png
│   ├── coverage_comparison.png
│   └── interval_width.png
│
└── reports/
    └── missing_data_analysis.qmd
```

## Reproducibility

The project is developed in R, with simulation settings, random seeds and package dependencies documented to support reproducibility. Automated tests verify the missingness mechanisms, imputation procedures and Rubin’s rules implementation.

Where possible, the from-scratch implementation is compared against established R packages such as `mice`, `stats` and `mitools`.

## Limitations

- Simulation findings depend on the assumed data-generating processes.
- MAR and MNAR mechanisms cannot generally be distinguished using observed data alone.
- Multiple imputation relies on appropriate imputation models and assumptions.
- Rubin’s rules do not automatically correct model misspecification.
- MNAR analysis requires additional assumptions and sensitivity analysis rather than a universally reliable imputation solution.

## Conclusion

This project provides a simulation-based statistical comparison of missing-data handling methods, connecting missingness theory, regression modelling, multiple imputation and statistical inference. By implementing Rubin’s rules from scratch and evaluating methods against known simulation parameters, it demonstrates how missing-data decisions influence bias, precision and the reliability of statistical conclusions.
