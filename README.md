# International Student Proportion Prediction Model

Using multiple linear regression and forward selection to predict the proportion of international students at top-ranked universities worldwide.

## Overview

University rankings are built from a mix of factors — teaching quality, research output, industry income, international outlook, and more. This project asks whether those same ranking inputs can be used to accurately *predict* one specific outcome: the proportion of international students at a university.

**Research question:** Can the variables used to rank universities be used to build an accurate model that predicts the proportion of international students at a university?

The hypothesis was informed by existing research: a more balanced female-to-male ratio may attract more international students (Lee, 2009), teaching quality plays a major role in university choice (Rahman et al., 2021), and a strong international outlook score reflects institutional reputation abroad (Acar, 2022) — all plausible predictors of international student proportion.

## Data

- **Source:** [World University Rankings 2023](https://www.kaggle.com/datasets/alitaqi000/world-university-rankings-2023) (Kaggle, uploaded by Syed Ali Taqi), based on Times Higher Education's 2023 rankings
- **Original size:** 1,799 universities, 13 variables (rank, name, location, student count, students-per-staff, international student proportion, female-to-male ratio, and six 0–100 THE scores: overall, teaching, research, citations, industry income, international outlook)
- **Filtered to:** the **top 200 ranked universities** with non-missing female-to-male ratio data, leaving **173 observations** for analysis
- **Response variable:** `inter_prop` — proportion of international students

## Methods

- Data cleaned and type-converted (all variables were originally stored as character/string values)
- Split into training (~120 obs.) and test (~53 obs.) sets
- Fit a **full multiple linear regression model** using all 10 numeric predictors, to establish a baseline and check for multicollinearity
- Applied a **forward selection algorithm** to identify the optimal subset of predictors, using Mallow's Cp to choose model size
- Compared the full and reduced models on **adjusted R²**, **F-statistic**, and **RMSE** on the held-out test set

## Results

Forward selection identified a 4-variable model as optimal (lowest Cp), dropping number of students, students-per-staff, overall score, research score, and citations score as least relevant:

| Model | Predictors | Adjusted R² | F-statistic | RMSE (test set) |
|---|---|---|---|---|
| Full model | All 10 variables | 0.6448 | 22.78 (df: 10, 110), p < 2.2e-16 | 0.0947 |
| **Reduced model** | Female-to-Male Ratio, Teaching Score, Industry Income Score, International Outlook Score | **0.6571** | **58.49** (df: 4, 116), p < 2.2e-16 | **0.0935** |

The reduced 4-variable model outperformed the full model on both adjusted R² and out-of-sample RMSE, despite using less than half the predictors — indicating the extra variables in the full model were adding noise rather than predictive power.

## Conclusion

The four selected variables — female-to-male ratio, teaching score, industry income score, and international outlook score — can effectively predict a university's proportion of international students, and do so more accurately than a model using all available ranking variables. The result supports the initial hypothesis that reputation, teaching quality, and gender balance are meaningfully associated with international student enrollment.

**Limitations:** the model assumes a linear, additive relationship between predictors and the response, which may not hold if true interactions exist between variables. Forward selection can also be order-sensitive and prone to overfitting, and the model remains sensitive to outliers. Location was excluded as a predictor due to its complexity as a many-level categorical variable.

**Future work:** modeling variable interactions explicitly, or using LASSO/ridge regression for potentially better prediction accuracy and built-in regularization.

## Tech Stack

R · tidyverse (dplyr) · leaps (forward selection) · yardstick (RMSE)

## Repository Structure

```
├── FinalProject.html               # Rendered notebook: full analysis, code, and figures
├── World_University_Rankings_2023.csv  # Raw dataset
└── README.md
```

## References

- Acar, T. (2022). *Indicators Affecting the International Outlook of Universities.* SAGE Open, 12, 1–9.
- Lee, S. A., Park, H., & Kim, S. (2009). *Gender differences in international students' adjustment.* College Student Journal, 43, 1217–1227.
- Rahman, M. S., Rahman, S., & Quainoo, E. (2021). *A Review of Quality and Quantity of Foreign Students' Education under the Characteristics of the Popularized Times.* International Journal of Social Science and Humanity, 9, 155–161.
- Taqi, S. A. (2023). *World University Rankings 2023* [Data set]. Kaggle. https://doi.org/10.34740/KAGGLE/DSV/6394958

## Authors

Samreen Kaur and team — STAT 301, Group 25
