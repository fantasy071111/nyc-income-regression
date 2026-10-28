# NYC Household Income: A Regression Analysis

This project explores how household income is associated with age, housing maintenance deficiencies, and the year a respondent moved to New York City. It was developed for Carnegie Mellon University's 36-202 statistics course.

## Research Question

How useful are these characteristics for understanding differences in household income and predicting income for a household with specified characteristics?

## Data

The analysis uses a course-provided sample of **299 households** from the **New York City Housing and Vacancy Survey**.

| Variable | Description |
|---|---|
| `Income` | Total household income in dollars |
| `Age` | Respondent's age in years |
| `MaintenanceDef` | Number of residential maintenance deficiencies during the 2002–2005 period |
| `NYCMove` | Year the respondent moved to New York City |

**Source:** [New York City Housing and Vacancy Survey — U.S. Census Bureau](https://www.census.gov/programs-surveys/nychvs.html)

## Methods

- Histograms and numerical summaries to explore individual variables
- Scatterplots to examine relationships between variables
- Cube-root transformation of income and natural-log transformation of age
- Multiple linear regression with an interaction term
- Variance inflation factors (VIFs) to assess multicollinearity
- Residual and normal Q–Q plots to evaluate model assumptions
- Model comparison and removal of a nonsignificant predictor

## Final Model

The final model includes log age, maintenance deficiencies, and their interaction:

```r
nyc$IncomeCubRt <- nyc$Income^(1/3)
nyc$LogAge <- log(nyc$Age)

Income_Final <- lm(
  IncomeCubRt ~ LogAge * MaintenanceDef,
  data = nyc
)
```

The interaction allows the association between maintenance deficiencies and transformed income to vary with age. `NYCMove` was removed because it was not statistically significant in the full interaction model.

## Results

| Measure | Final Model |
|---|---:|
| R-squared | 0.09220 |
| Adjusted R-squared | 0.08297 |
| Residual standard error | 6.894 |
| Interaction p-value | 0.0000862 |

The model explains approximately **9.22% of the variation in cube-root-transformed household income**. This does not mean that predictions are 9.22% accurate.

The significant interaction suggests that the relationship between maintenance deficiencies and transformed income depends on the respondent's age.

For a household with three maintenance deficiencies and a respondent aged 53, the predicted cube root of income is approximately **33.854**. Cubing this value produces a back-transformed point estimate of approximately **$38,801**. This back-transformed value is not generally an estimate of mean income in dollars.

## Limitations

- Most variation in transformed household income remains unexplained.
- The residual plot shows remaining curvature, and the Q–Q plot shows departures from Normality in the tails.
- High VIFs in the interaction model increase uncertainty in individual coefficient estimates.
- The analysis identifies associations and does not establish causation.
- Predictive performance has not been evaluated on a separate test sample.

## Files

| File | Purpose |
|---|---|
| `36202_project1.Rmd` | R Markdown analysis and written report |
| `nyc.csv` | Dataset used in the analysis |
| `README.md` | Project overview and reproduction instructions |

## How to Reproduce

1. Download or clone this repository.
2. Keep `nyc.csv` and `36202_project1.Rmd` in the same folder.
3. Open the R Markdown file in RStudio or Posit Cloud.
4. Install the required packages if needed:

```r
install.packages(c(
  "rmarkdown", "knitr", "kableExtra", "pander",
  "readr", "magrittr", "car", "leaps"
))
```

5. Select **Knit to HTML** to view the report. Knitting to PDF also requires a working LaTeX installation.

## Future Directions

Additional predictors, such as education, employment, and the number of household earners, could help explain income differences. Evaluating the model on a separate sample would also provide a better assessment of its predictive performance.

## Author

Lawrence Zhou  
Carnegie Mellon University  
Statistics and Machine Learning
