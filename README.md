# Impact of Race and Gender on Emergency Room Treatment

R code for our STA 230 final project (Zachary Serby and Irene Agusti, Fall 2023): "Unmasking Disparities: Examining the Impact of Race and Gender on Emergency Room Treatment for Young Adults."

We used ten years (2013-2022) of NEISS emergency room data on patients aged 16-25 to test whether race and sex are associated with ER disposition (treated and released, transferred, admitted, held for observation, left without being seen, died in the ER). The multinomial logistic regression was the only model with AUC above 0.5. It found significant differences for Black, American Indian/Alaska Native, Native Hawaiian/Pacific Islander and female patients, relative to White and male patients.

Report: [report/STA-230-final.pdf](report/STA-230-final.pdf)

Knitted results with figures: [Stats-230-Final-Project-1.html](https://htmlpreview.github.io/?https://github.com/zakserby/Impact-of-Race-and-Gender-on-Emergency-Room-Treatment/blob/main/analysis/Stats-230-Final-Project-1.html)

Data:

The raw NEISS files aren't in this repo. They're public and can be downloaded from the Consumer Product Safety Commission. data/README.md has the exact query, the folder layout the code expects, and the codes for sex, race and disposition.

Analysis (chunks in analysis/er_disparities_analysis.Rmd):
- load-data reads each yearly NEISS_YYYY.XLSX file, adds a year column, combines them and drops unused columns.
- load-key reads the NEISS format key (code to label lookup).
- chisq-sex and fisher-race test whether disposition is independent of sex and of race.
- fit-models fits a multinomial logistic regression, a decision tree and a random forest (Disposition ~ Race + Sex).
- accuracy compares the accuracy of the three models.
- roc-multinom, roc-tree and roc-rf plot one-vs-rest ROC curves and compute the AUC for each disposition.
- multinom-summary gives the coefficients and exponentiated coefficients (relative risk ratios).
- multinom-pvalues plots coefficient p-values by disposition, with a line at 0.05.
- tree plots the decision tree. rf gives random forest variable importance and the error plot.
- words-sex and words-race find the most common words in the injury narrative by sex and by race.
- composition makes segmented bar charts of sex and race within each disposition.
- without-treatment1 refits the multinomial model without the majority disposition (treated and released).

Other files:
- analysis/Stats-230-Final-Project-1.html : knitted output of the analysis
- report/STA-230-final.pdf : final written report
- data/README.md : how to get the NEISS data

To run: put the data in data/ as described in data/README.md, then knit analysis/er_disparities_analysis.Rmd from the analysis folder. Written in R 4.3 with tidyverse, readxl, tidytext, broom, nnet, rpart, rpart.plot, randomForest, pROC and caret.
