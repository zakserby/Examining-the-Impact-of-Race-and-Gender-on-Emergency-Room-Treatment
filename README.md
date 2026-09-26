# Examining the Impact of Race and Gender on Emergency Room Treatment

**Unmasking Disparities: Examining the Impact of Race and Gender on Emergency Room Treatment for Young Adults**

Zachary Serby and Irene Agusti · STA 230: Statistical Models for Machine Learning (Fall 2023) · Final project

**[Read the full report (PDF)](report/STA-230-final.pdf)** · **[Analysis code (R Markdown)](analysis/er_disparities_analysis.Rmd)** · **[Rendered results and figures (HTML)](https://htmlpreview.github.io/?https://github.com/zakserby/Examining-the-Impact-of-Race-and-Gender-on-Emergency-Room-Treatment/blob/main/analysis/Stats-230-Final-Project-1.html)**

## Overview

Do race and gender contribute to disparities in emergency room (ER) treatment, and if so, how?
We analyzed ten years (2013–2022) of ER visits from the U.S. Consumer Product Safety Commission's
**National Electronic Injury Surveillance System (NEISS)**, focusing on young adults aged 16–25 with
serious injuries. The outcome was the ER **disposition**: six treatment outcomes, compared across
seven race/ethnicity categories and two sexes.

## Data

- **Source:** [NEISS](https://www.cpsc.gov/Research--Statistics/NEISS-Injury-Data), U.S. Consumer Product Safety Commission, extracted with the NEISS Estimates Query Builder
- **Sample:** ages 16–25, 2013–2022, serious injuries (fractures, hemorrhage, lacerations, concussions, etc.; dental injuries excluded), all body parts, races and sexes
- **Variables:** Race (reference level: White), Sex (reference: Male), Disposition (treated and released, treated and transferred, admitted, held for observation, left without being seen, died in the ER), and the free-text injury Narrative
- Records are treated as individual observations; NEISS sample weights and primary sampling units were not used

The raw data files are not included in this repository. See [`data/README.md`](data/README.md) for how to obtain them.

## Methods

1. **Data cleaning:** read and combine the yearly Excel extracts, drop weight/PSU and sparse columns, convert Race, Sex and Disposition to factors
2. **Tests of association:** chi-squared test (Sex × Disposition) and Fisher's exact test with simulated p-value (Race × Disposition)
3. **Model comparison:** multinomial logistic regression, decision tree (`rpart`) and random forest (`randomForest`, 300 trees), compared on accuracy and one-vs-rest ROC/AUC for each disposition
4. **Inference:** exponentiated multinomial logistic regression coefficients (relative risk ratios) and coefficient p-values
5. **Exploratory analysis:** most frequent narrative words by sex and race, disposition composition by sex and race, and a sensitivity check re-fitting the model without the majority class (treated and released)

## Key findings

- Disposition was significantly associated with both **sex** (χ², p < 2.2e-16) and **race** (Fisher's exact test, p ≈ 0.0005).
- All three models had the same accuracy (0.902), equal to always predicting "treated and released," the majority class (~90% of visits). The decision tree made no splits.
- The multinomial logistic regression was the only model with AUC above 0.5, ranging from 0.54 to 0.70 across dispositions (tree and forest: 0.50 for all).
- Relative to White patients, **Black** patients were less likely to be treated and transferred, as were **female** patients relative to male patients. Coefficients for **Native Hawaiian/Pacific Islander**, **American Indian/Alaska Native**, **Black** and **female** patients were statistically significant. The Native Hawaiian/Pacific Islander coefficient for dying in the ER (p < 0.05) warrants further investigation.
- With "treated and released" removed, Race and Sex were less significant, suggesting much of the disparity lies in who is treated and released.

## Limitations and next steps

- Class imbalance: ~90% of visits share one outcome, so predictive performance is modest. Models were evaluated on the data they were fit to.
- NEISS is a stratified, weighted sample. A survey-weighted analysis may change the estimates.
- Future work: interactions between race and sex, adjusting for diagnosis, a wider age range, and examining whether the wording of injury narratives relates to treatment.

## Repository structure

```
.
├── analysis/
│   ├── er_disparities_analysis.Rmd    # full analysis (R Markdown)
│   └── Stats-230-Final-Project-1.html # knitted output with results and figures
├── report/
│   └── STA-230-final.pdf              # final written report
├── data/
│   └── README.md                      # how to obtain the NEISS data
└── README.md
```

## Reproducing the analysis

1. Download the data as described in [`data/README.md`](data/README.md).
2. Install the R packages:
   ```r
   install.packages(c("tidyverse", "readxl", "tidytext", "broom", "nnet", "rpart",
                      "rpart.plot", "randomForest", "pROC", "caret", "rmarkdown"))
   ```
3. Knit `analysis/er_disparities_analysis.Rmd` from the `analysis/` folder (originally run in R 4.3).

To view the knitted results without running anything, open the [rendered HTML](https://htmlpreview.github.io/?https://github.com/zakserby/Examining-the-Impact-of-Race-and-Gender-on-Emergency-Room-Treatment/blob/main/analysis/Stats-230-Final-Project-1.html) or download [`analysis/Stats-230-Final-Project-1.html`](analysis/Stats-230-Final-Project-1.html) and open it in a browser.

## References

- Blanchard, J. C., et al. "Disparities in Emergency Department Use by Race: A Methodological Critique of the Literature." *The Milbank Quarterly*, 81(2), 2003.
- U.S. Consumer Product Safety Commission, National Electronic Injury Surveillance System (NEISS). https://www.cpsc.gov/
