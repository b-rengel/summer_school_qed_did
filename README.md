**To view this analysis in full, please visit: https://b-rengel.github.io/summer_school_qed_did/**

# Summer School QED Analysis (Synthetic Example)

**Author:** Bek Rengel  
**Institution:** University of the West of England (UWE Bristol)  
**Date:** April 2025  

---

## Overview

This repository contains a worked example of the analysis process used to evaluate the **Access to UWE Summer School**, as part of UWE Bristol's Access and Participation Plan. The workflow presented here forms part of a larger evaluation of this project. 

The original evaluation used survey data from an intervention group (students who attended the summer school) and a comparison group (students who did not), with questionnaires completed **before** (Pre) and **after** (Post) the summer school period. Items were measured on **Likert scales (1–5)**.

Because the original data are sensitive and cannot be shared, this repository uses **synthetic data** that:

- preserve the **structure** of the real dataset (same number of participants, same items, same Pre/Post design), and  
- mimic the **patterns** observed in the real evaluation (e.g. stronger improvements for the intervention group on selected items),  

but contain **no real student data**.

The goal of this project is to demonstrate the **analysis workflow**, not to report actual results.

---

## Files in this Repository
- `summer_school_QED_survey_analysis.Rmd`
    - Main R Markdown file containing:
        - synthetic data generation,
        - descriptive summaries,
        - Wilcoxon/Mann–Whitney tests,
        - Difference-in-Difference models (linear and ordinal).
- `README.md`
    - This documentation.

---

## How to Run
### Requirements
This analysis was written in R and uses the following packages:
- dplyr
- tidyr
- ggplot2
- MASS
- purrr

You can install any missing packages with:

```
install.packages(c("dplyr", "tidyr", "ggplot2", "MASS", "purrr"))
```

### Steps
- Open summer_school_QED_survey_analysis.Rmd in RStudio (or another R Markdown-compatible editor).
- Ensure all required packages are installed.
- Click Knit to generate the HTML report.
Because the data are generated synthetically inside the Rmd itself, there are no external data files required.

---

## Notes
All data in this repository are synthetic and contain no real student information.

For actual evaluation findings, please refer to the official reports on UWE Bristol's Access and Participation Plan webpages, under the "Widening Access and Raising Attainment" section (some reports forthcoming in 2026).

---

## Study Design (Quasi-Experimental DiD)

The evaluation uses a **quasi-experimental, Difference-in-Difference (DiD)** design:

- **Groups**
  - CG: Comparison group (did not attend the summer school)
  - IG: Intervention group (attended the summer school)

- **Time**
  - Pre: Pre-activity survey
  - Post: Post-activity survey

- **Outcomes**
  - Survey items Q1–Q10, measured on Likert scales (1–5)
  - Survey items Q1–Q3 and Q4–Q6 are validated scales from TASO's Access and Success Questionnaire. Q7–Q10 are an internal scale used to measure knowledge and confidence of participants taking part in widening access initiatives. This scale is not currently validated.

The key causal question is:

> Did the summer school cause a larger positive change in outcomes for the intervention group (IG) than for the comparison group (CG)?

This is addressed using a **Difference-in-Difference** approach, implemented via regression models.

---

## Synthetic Data Generation

The script:

1. Creates a **long format** dataset `dfDID` with:
   - `id` (synthetic participant ID),
   - `survey_stage` (Pre / Post),
   - `Treatment` (CG / IG),
   - survey items `Q1`–`Q10` (Likert 1–5).

2. Creates a **wide format** dataset `dfWide` with:
   - `id`, `Treatment`,
   - `PRE_Q*`, `POST_Q*` (pre and post responses),
   - `DIFF_Q* = POST_Q* - PRE_Q*` (difference scores).

3. Simulates each Likert item via an underlying **latent normal variable**, with:
   - baseline means,
   - a small general time shift (everyone improves slightly from Pre to Post),
   - an additional **treatment effect for IG at Post** for selected items (Q8, Q9, Q5),
   - random noise.

4. Converts latent scores to **ordinal responses (1–5)** using fixed cut-points.

The synthetic data are tuned so that Q8 and Q9 show clear treatment effects, and Q5 shows a near-significant effect, mirroring the pattern in the real evaluation.

---

## Analyses

### 1. Descriptive Statistics

For each group (IG, CG), the script computes:

- Mean, standard deviation, and sample size for:
  - PRE_ variables,
  - POST_ variables,
  - DIFF_ (Post – Pre) variables.

These descriptives provide a sense-check of the synthetic data and illustrate how summary statistics were obtained in the original study.

---

### 2. Non-Parametric Tests (Wilcoxon & Mann–Whitney)

Examples are provided for:

- **Within-group changes (paired Wilcoxon tests)**  
  - IG: PRE vs POST for selected items  
  - CG: PRE vs POST for selected items  

- **Between-group differences (Mann–Whitney tests)**  
  - IG vs CG on PRE scores  
  - IG vs CG on POST scores  
  - IG vs CG on DIFF scores  

These tests treat the Likert data as ordinal and give simple checks of:

- “Did each group change over time?”
- “Did the intervention group change more than the comparison group?”

These tests illustrate the process followed in the original study, and only some examples are supplied.
 
---

### 3. Difference-in-Difference (DiD) Modelling

The main causal analysis is conducted at the **item level** (Q1–Q10) using:

1. **Linear regression (OLS)** as a quick sense-check: 
```
lm(Qj ~ survey_stage * Treatment, data = dfDID)
```

2. **Ordinal logistic regression** as the primary model: 
```
polr(Qj_ord ~ survey_stage * Treatment, data = dfDID, Hess = TRUE)
```

Where:
- survey_stage has levels: pre (reference), post
- Treatment has levels: CG (reference), IG
- survey_stagePost:TreatmentIG is the DiD effect. This interaction term quantifies the additional pre–post change in the intervention group over and above the change observed in the comparison group.

For key items (Q8, Q9, Q5), the script:
- Visualises group means over time with error bars
- Fits the ordinal model (MASS::polr)
- Extracts:
    - log-odds coefficient (β) for the DiD term,
    - p-value,
    - odds ratio (OR = exp(β)),
    - 95% confidence interval for OR.
In the real evaluation (not reproduced numerically here), Q8 and Q9 showed strong positive DiD effects, with Q5 showing a relatively large but borderline-significant effect.

**Parallel Trends Assumption (DiD)**
The DiD design assumes that, in the absence of the intervention, the outcomes for IG and CG would have evolved in parallel over time. With only a single pre-period, this cannot be tested statistically, so the analysis instead:
- checks similarity of group means at baseline (Pre),
- visually inspects changes over time.

**Proportional Odds (Ordinal Regression)**

Ordinal logistic regression assumes proportional odds, meaning that the effects of predictors are constant across thresholds of the ordinal outcome. This assumption is not formally tested in this example, and the model is used as a practical approximation for Likert-type items.
