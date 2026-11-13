# Hate Crime Rates and State-Level Indicators in the U.S.

A regression analysis in R of how political, demographic, and economic indicators relate to state-level hate crime rates around the 2016 U.S. presidential election.

Group project, UW-Madison, April 2025.

## Research Questions

1. How is Trump vote share in the 2016 election related to hate crime rates across states?
2. How do demographic characteristics (high school graduation rate, white poverty rate, share of non-citizens) relate to hate crime rates?
3. How do economic conditions (median household income, unemployment, income inequality) relate to hate crime rates?
4. Which of these variables remain significant predictors when they are modeled together?

## Data

The analysis uses the [FiveThirtyEight hate crimes dataset](https://github.com/fivethirtyeight/data/tree/master/hate-crimes), which combines data from the Southern Poverty Law Center (SPLC), the FBI, the U.S. Census Bureau, the Kaiser Family Foundation, and the United States Elections Project. It has 51 rows (the 50 states plus D.C.) and 12 columns.

- **Outcome:** `hate_crimes_per_100k_splc`, hate crimes per 100,000 residents reported to the SPLC from November 9 to 18, 2016 (the ten days after the election)
- **Predictors:** `share_voters_voted_trump`, `share_population_with_high_school_degree`, `share_white_poverty`, `share_non_citizen`, `median_household_income`, `share_unemployed_seasonal`, `gini_index`

The predictors come from different years (mostly 2015 and 2016, while the high school degree measure is from 2009). See the [FiveThirtyEight README](https://github.com/fivethirtyeight/data/blob/master/hate-crimes/README.md) for exact definitions and sources. Some states are missing values for the outcome or for `share_non_citizen`, so the models use 45 to 47 states.

## Methods

- Fit a simple linear regression of the SPLC hate crime rate on each of the seven predictors, with scatter plots and fitted lines (ggplot2) and residual and normal Q-Q plots to check model assumptions.
- Fit a multiple linear regression with all seven predictors.
- Tested each coefficient with a t-test and each model with an overall F-test at α = 0.05.

## Results

**Simple linear regressions**

| Predictor | Coefficient | p-value | R² | n |
|---|---|---|---|---|
| Trump vote share | -1.444 | < 0.001 | 0.429 | 45 |
| High school degree share | 2.320 | 0.035 | 0.095 | 47 |
| White poverty share | -2.837 | 0.051 | 0.082 | 47 |
| Non-citizen share | 1.961 | 0.107 | 0.059 | 45 |
| Median household income (per $10,000) | 0.096 | 0.016 | 0.123 | 47 |
| Unemployment share | 2.098 | 0.578 | 0.007 | 47 |
| Gini index | 4.021 | 0.024 | 0.109 | 47 |

**Multiple linear regression (n = 45)**

The full model explains 57.6% of the variation in state hate crime rates (adjusted R² = 0.496, F = 7.19 on 7 and 37 df, p < 0.001). Three predictors are significant at α = 0.05:

| Predictor | Coefficient | p-value |
|---|---|---|
| Trump vote share | -1.255 | 0.012 |
| High school degree share | 4.437 | 0.014 |
| Gini index | 4.736 | 0.031 |

Median household income, unemployment, white poverty, and non-citizen share are not significant once the other variables are included.

## Limitations

- The data are cross-sectional and observational, so these results describe associations between states, not causes.
- The outcome counts incidents reported to the SPLC over a ten-day window, and both SPLC and FBI data rely on voluntary reporting. Differences in reporting across states may account for part of the pattern.
- D.C. has both the lowest Trump vote share and the highest hate crime rate in the data, so it has a large influence on the simple regression for Trump vote share.
- Coefficients are on each variable's original scale, so their sizes cannot be compared directly to rank predictors.
- The analysis covers a single election period, so it cannot show trends over time.

## Repository Structure

```
├── Hate_Crime_Analysis.Rmd    # analysis source (R Markdown)
├── Hate_Crime_Analysis.html   # rendered report
├── hate_crimes.csv            # FiveThirtyEight data
└── README.md
```

## Reproducing the Analysis

1. Install R and the required packages:

   ```r
   install.packages(c("rmarkdown", "ggplot2"))
   ```

2. Clone this repository and keep `hate_crimes.csv` in the same folder as the `.Rmd` file.

3. Render the report:

   ```r
   rmarkdown::render("Hate_Crime_Analysis.Rmd")
   ```

## Data Source

FiveThirtyEight, [hate-crimes](https://github.com/fivethirtyeight/data/tree/master/hate-crimes). Related article: [Higher Rates Of Hate Crimes Are Tied To Income Inequality](https://fivethirtyeight.com/features/higher-rates-of-hate-crimes-are-tied-to-income-inequality/).
