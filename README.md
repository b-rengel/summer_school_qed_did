**To view this analysis in full, please visit the following links**
- Phase 1: survey-based evaluation: https://b-rengel.github.io/summer_school_qed_did/survey/
- Phase 2: evaluation of academic outcomes: https://b-rengel.github.io/summer_school_qed_did/outcomes/

 # Summer School Evaluation (Synthetic Example)

**Author:** Bek Rengel  
**Institution:** University of the West of England (UWE Bristol)  
**Date:** April–November 2025  

## Overview

This repository contains worked examples of the quantitative analysis workflow used to evaluate the **Access to UWE Bristol Summer School**, delivered as part of <a href="https://www.uwe.ac.uk/about/values-vision-strategy/equality-diversity-and-inclusivity/policies#a7060a79c-a095-4fac-b16d-bd2e100068a9">UWE Bristol’s Access and Participation Plan.</a>

The original evaluation used sensitive student data (survey responses and administrative outcomes) that cannot be shared. To comply with GDPR and ethical requirements, this repository uses **synthetic data** that are randomly generated to:

- preserve the structure of the original datasets (group sizes, variables, timing), and
- mimic key *patterns* observed in the real evaluation,
- while containing **no real or identifiable student information**.

The goal of this repository is to demonstrate an analysis workflow and methodological reasoning, not to report actual results.

---

## Repository contents

This repo contains two analysis phases, each provided as an R Markdown notebook and a rendered HTML report via GitHub Pages.

### Phase 1 — Survey-based evaluation (Quasi-experimental Difference-in-Difference analysis)

- **R Markdown source:** `summer_school_QED_survey_analysis.Rmd`  
- **Rendered HTML:** https://b-rengel.github.io/summer_school_qed_did/survey/

**Summary of workflow**
- Synthetic survey data generation (Likert 1–5, Pre/Post)
- Descriptive summaries and visualisation
- Non-parametric tests (Wilcoxon / Mann–Whitney)
- Difference-in-Difference modelling
  - Linear regression as a sense-check
  - Ordinal logistic regression (`MASS::polr`) as the primary approach
- Discussion of key assumptions (parallel trends, proportional odds)

**Design**
- Groups: intervention group (IG) attended the Summer School vs comparison group (CG) who did not attend
- Time: Pre-Summer School vs Post-Summer School
- Outcomes: survey items Q1–Q10 (Likert 1–5)

Items Q1–Q6 are drawn from validated TASO Access and Success scales. Items Q7–Q10 are an internal scale used to measure knowledge and confidence (not currently validated).

---

### Phase 2 — enrollment, continuation, and attainment outcomes

- **R Markdown source:** `summer_school_academic_outcomes.Rmd`  
- **Rendered HTML:** https://b-rengel.github.io/summer_school_qed_did/outcomes/

**Outcomes**
- **enrollment** at the institution of study
- **Continuation** into the following academic year (among enrolled participants)
- **Module attainment** for enrolled participants (average mark, average pass rate)

**Summary of workflow**
- Synthetic administrative dataset generation
- Descriptive summaries and visualisation
- Fisher’s exact tests for small-sample categorical outcomes
- Frequentist logistic regression (odds ratios with Wald confidence intervals)
- Bayesian logistic regression (`rstanarm`) as a robustness check
  - weakly informative priors
  - posterior odds ratios and 95% credible intervals
  - basic convergence diagnostics (Rhat, effective sample size, trace plots)
- Adjustment for a baseline survey covariate identified as a potential confounder

---

## How to run locally

### Requirements

This project is written in R. Core packages used include:

- dplyr
- tidyr
- ggplot2
- MASS (Phase 1)
- purrr (Phase 1)
- rstanarm (Phase 2)

Install any missing packages with:

```r
install.packages(c("dplyr", "tidyr", "ggplot2", "MASS", "purrr", "rstanarm"))
```

## Steps

- Open either .Rmd file in RStudio.
- Ensure required packages are installed.
- Click Knit to render the HTML report.

Synthetic data are generated within the notebooks, so no external data files are required.

## Notes and limitations

- All data in this repository are synthetic and do not contain real student information.
- Numerical values and inferential outputs will differ from official evaluation reports.
- The Bayesian models are included as robustness checks and to demonstrate workflow; results should be interpreted under the model assumptions and synthetic data limitations.
- For actual evaluation findings, please refer to UWE Bristol’s Access and Participation Plan pages under Widening Access and Raising Attainment (some reports forthcoming in 2026).

## Copyright and attribution

© University of the West of England, Bristol (UWE Bristol), 2025.
Original author: Bek Rengel. All rights reserved.


