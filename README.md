# What Drives College Graduation Rates? (R + College Scorecard)
Author: Kevin Wu

Stack: R · tidyverse · ggplot2 · broom · infer · modelr

##  Summarize
I analyzed U.S. Department of Education College Scorecard data to explore which factors are associated with 4-year graduation/completion rates. After data cleaning, feature engineering (including a majority-race category), visualization, and a multiple linear regression, I found:
* Selectivity (lower admission rates) and higher SAT averages are associated with higher graduation rates.
* Annual cost shows a small positive association (likely capturing institutional resources rather than a direct causal effect).
* Patterns differ across majority-race enrollment groups.
* In my run, the model achieved R² ≈ 0.67 (association, not causation).


Findings are associative, not causal. Many factors (aid, academic support, student characteristics, etc.) are not modeled here.

## Project Goals
Identify factors associated with 4-year graduation/completion rates.


Visualize relationships across institutions and demographic profiles.


Build and diagnose a simple multiple linear regression.



## Data
* Source: U.S. Department of Education College Scorecard (institution-level).

* Core columns used:
    * C200_4 — 4-year completion/graduation rate
    * ADM_RATE — admission rate
    * SAT_AVG — average SAT score
    * COSTT4_A — average annual cost (attendance)
    * UGDS_* — enrollment shares (White, Black, Hispanic, Asian, AIAN, NHPI, Two+)
* Derived feature: college_race_type = majority race group among {White, Black, Hispanic, Asian}; otherwise “Other”.
## Methods
    * EDA: faceted scatter plots, boxplots by college_race_type, bivariate relationships.


Model: multiple linear regression

 completion_rate ~ admission_rate + SAT_avg + cost + college_race_type


Diagnostics: predicted vs. observed, residuals vs. predicted, QQ plot.


## Reproducibility
1) Requirements
R (latest recommended)


Packages: tidyverse, ggplot2, broom, infer, modelr, readr, dplyr

 install.packages(c("tidyverse","ggplot2","broom","infer","modelr"))
2) Get the data (two options)
Option A — Use a trimmed CSV (recommended for repo size)
 If you have a large Scorecard CSV, trim to only the needed columns and save an RDS:
library(readr); library(dplyr)
use <- c("UNITID","INSTNM","ADM_RATE","UGDS","UGDS_WHITE","UGDS_BLACK","UGDS_HISP",
         "UGDS_ASIAN","UGDS_AIAN","UGDS_NHPI","UGDS_2MOR","SAT_AVG","COSTT4_A","C200_4")

scorecard <- read_csv("path/to/MERGED_XXXX_YY_PP.csv", col_types = cols(), progress = FALSE)
college <- scorecard %>% select(any_of(use))
write_rds(college, "data/college.rds")

Option B — If you already have an .rds
 Place it at data/college.rds.
3) Render the analysis
Open in RStudio and knit, or run:
rmarkdown::render("final_project.Rmd")

Figures will be saved into figs/.

Key Results (brief)
Selectivity: Lower ADM_RATE ↔ higher C200_4.


Academics: Higher SAT_AVG ↔ higher C200_4.


Cost: COSTT4_A shows a small positive association with C200_4.


Demographics: Distributions and outcomes vary by college_race_type.


Model fit: ~0.67 R² in my run (may vary by year/file).



Limitations
Associations ≠ causation.


Some fields have missing values or “PrivacySuppressed.”


Unobserved confounders (aid, program mix, student prep, support services, etc.).



## Future Work
* Add financial aid, retention, and program-level features.
* Explore non-linear models (e.g., GAMs, tree-based models).
* Robustness checks across Scorecard years and subsets.
  

## How to Cite / Acknowledge
U.S. Department of Education, College Scorecard.


R packages: tidyverse, ggplot2, broom, infer, modelr.
