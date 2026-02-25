# A/B Testing Analysis (User-Level Clean Experiment)

## Overview

This project performs an A/B test analysis on a Meta Campaign dataset containing:

- USER_ID  
- VARIANT_NAME (control / variant)  (campaign types)
- REVENUE  

The goal was to evaluate whether the variant increases the probability of purchase.

The analysis was done in Python using Jupyter Notebook.

---

## Key Problem Discovered

Initial inspection showed that some users appeared in both control and variant groups.

This violates a core A/B testing assumption:

> Each user must belong to exactly one variant.

If a user appears in both groups:
- Independence is broken
- Treatment contamination occurs
- Statistical tests become unreliable

So we cleaned the data before doing any hypothesis testing.

---

## Data Cleaning Steps

1. Identified users assigned to more than one variant.
2. Removed those users entirely.
3. Aggregated revenue at the user level.

After cleaning:
- 4,783 unique users remained
- Each user belonged to exactly one variant
- Each row represented one user

This restored a valid experimental structure.

---

## Revenue Distribution

Revenue was highly skewed:

- Median = 0
- 75% of users had zero revenue
- A few extreme outliers drove the mean

Because of this, testing average revenue directly would be unstable.

So instead of comparing mean revenue, we reframed the metric.

---

## Final Metric Used

Binary conversion:

- Converted = 1 if revenue > 0  
- Converted = 0 otherwise  

This allowed us to test:

> Does the variant increase probability of purchase?

---

## Hypothesis

Null Hypothesis (H₀):  
Conversion rate (control) = Conversion rate (variant)

Alternative Hypothesis (H₁):  
Conversion rate (variant) > Conversion rate (control)

One-tailed two-proportion z-test was used.

---

## Results

| Group    | Users | Conversions | Conversion Rate |
|----------|--------|-------------|-----------------|
| Control  | 2390   | 54          | 2.26%           |
| Variant  | 2393   | 42          | 1.76%           |

Z-statistic: 1.24  
P-value: 0.1069  

Result:  
We failed to reject the null hypothesis at the 5% significance level.

There is no statistical evidence that the variant increases conversion.

---

## Important Notes

- No hypothesis flipping was done after seeing results.
- No p-hacking.
- Independence assumption was validated before testing.
- Metric choice was driven by distribution analysis.


---

## Tools Used

- Python 3.11
- pandas
- numpy
- matplotlib
- seaborn
- statsmodels

