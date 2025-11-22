FinalRmarkdownproject
================
Jordyn Blauer
2025-11-12

- [ABSTRACT](#abstract)
- [BACKGROUND](#background)
- [STUDY QUESTION and HYPOTHESIS](#study-question-and-hypothesis)
  - [Questions](#questions)
  - [Hypothesis](#hypothesis)
  - [Prediction](#prediction)
- [METHODS/RESULTS](#methodsresults)
  - [1st Analysis- Linear Regression.](#1st-analysis--linear-regression)
  - [2nd Analysis - Perason
    correlation](#2nd-analysis---perason-correlation)
  - [3rd Analysis- Shapiro-Wilk Normality
    Tests](#3rd-analysis--shapiro-wilk-normality-tests)
- [DISCUSSION](#discussion)
  - [Interpretation of 1st analysis- Linear
    Regression](#interpretation-of-1st-analysis--linear-regression)
  - [Interpretation of 2nd analysis- Pearson
    Correlation.](#interpretation-of-2nd-analysis--pearson-correlation)
  - [Interpretation of 3rd analysis- Shapiro-Wilk
    Test](#interpretation-of-3rd-analysis--shapiro-wilk-test)
- [CONCLUSION](#conclusion)
- [REFERENCES](#references)

# ABSTRACT

Asthma is a chronic respiratory disease that can be worsened by many
factors, one such factor being air quality. The data came from the U.S.
Environmental Protection Agency and the CDC to find asthma rates and
poor air quality days across all 50 states in the U.S. The tests
performed were a linear regression line, a pearson correlation test, and
a shapiro-wilks test. The results showed a weak negative correlation
between poor air quality days and asthma prevalence with an r value of
-.279 and a p value of .0497. Both of the variables found a normal
distribution. The findings do not support the original hypothesis that
California would have the highest rates of asthma due to them having the
poorest air quality. Our results show that air quality does not affect
the rates of asthma due to the negative correlation, showing that there
is likely additional factors that contribute to asthma rates such as
climate, genetics, and altitude.

# BACKGROUND

Asthma is a lung condition that causes difficulty in breathing due to
constricted airways in the lungs. There are many different kinds of
asthma such as; exercise-induced asthma, occupational asthma, and
allergy-induced asthma (Asthma). Those with occupational asthma can have
attacks triggered by things such as gases, fumes, and dust, similar to
that of air pollution (Asthma). Severity of the condition differs from
person to person with some only experiencing minor inconveniences while
others can experience debilitating and life-threatening challenges from
the disease (Asthma). Asthma is a very common disease in the U.S., about
28 million people in the U.S. suffer from asthma (Asthma facts).

Poor air quality can impact those who suffer from asthma by causing
their condition to worsen and trigger attacks (Tiotiu). It has been
found in epidemiological studies that greater levels of air pollution
has been associated with childhood asthma and that following an
improvement of air quality, childhood asthma incidence had lowered
(Jama). The state with the worst air quality in the U.S. is California
due to it’s high air pollution.

The graph shown below “Adults with Current Asthma vs Poor-Air Quality
Days by State (Ranked by Asthma %),” shows that the state that has the
highest prevalence of asthma in adults is California. Due to this, it is
believed that air quality can affect the prevalence of asthma per state.
We hypothesize that states that have a higher amounts of poor air
quality days will have a higher prevalence of asthma. We predict that
California will have the most poor air quality days and because of that
they will also have the highest rates of asthma compared to other
states.

``` r
# Load the data
air_data <- data.frame(
  State = c("Alabama", "Alaska", "Arizona", "Arkansas", "California", "Colorado", "Connecticut", "Delaware", "Florida", "Georgia", "Hawaii", "Idaho", "Illinois", "Indiana", "Iowa", "Kansas", "Kentucky", "Louisiana", "Maine", "Maryland", "Massachusetts", "Michigan", "Minnesota", "Mississippi", "Missouri", "Montana", "Nebraska", "Nevada", "New Hampshire", "New Jersey", "New Mexico", "New York", "North Carolina", "North Dakota", "Ohio", "Oklahoma", "Oregon", "Pennsylvania", "Rhode Island", "South Carolina", "South Dakota", "Tennessee", "Texas", "Utah", "Vermont", "Virginia", "Washington", "West Virginia", "Wisconsin", "Wyoming"),
  Poor_Air = c(0.27, 0.12, 0.33, 0.3, 0.46, 0.25, 0.22, 0.31, 0.24, 0.3, 0.03, 0.28, 0.34, 0.25, 0.27, 0.29, 0.23, 0.29, 0.08, 0.19, 0.23, 0.24, 0.14, 0.31, 0.22, 0.19, 0.1, 0.25, 0.1, 0.27, 0.22, 0.16, 0.2, 0.17, 0.25, 0.34, 0.29, 0.31, 0.16, 0.25, 0.15, 0.23, 0.33, 0.29, 0.13, 0.12, 0.24, 0.16, 0.2, 0.15),
  Asthma = c(9.6, 10.7, 9.7, 10.5, 8.7, 10.8, 12.4, 9.9, 9.3, 9.6, 9.1, 11.1, 8.7, 11, 9.7, 10.7, 10.8, 10, 13.1, 10.4, 11.3, 11.9, 9.9, 9.4, 10.4, 11.7, 8.1, 10.1, 13.1, 8.9, 10.4, 10.3, 9.2, 10.4, 11.4, 12.3, 11.5, 10.1, 13.3, 9, 8.3, 11.7, 7.9, 11, 12.9, 9.9, 10.9, 12.9, 10.9, 10.5)
)
```

``` r
read.csv("aqi_table.csv")
```

    ##    Color               Level.of.Concern    Index.Range
    ## 1  Green                           Good           0-50
    ## 2 Yellow                       Moderate         51-100
    ## 3 Orange Unhealthy for Sensitive Groups        101-150
    ## 4    Red                      Unhealthy        151-200
    ## 5 Purple                 Very Unhealthy        201-300
    ## 6 Maroon                      Hazardous 301 and higher
    ##                                                                                        Description
    ## 1                          Air quality is satisfactory, and air pollution poses little or no risk.
    ## 2      Air quality is acceptable; some people unusually sensitive to air pollution may be at risk.
    ## 3       Sensitive groups may experience health effects; general public less likely to be affected.
    ## 4 Some members of the general public may experience health effects; sensitive groups more serious.
    ## 5                                  Health alert: risk of health effects is increased for everyone.
    ## 6                     Health warning of emergency conditions: everyone more likely to be affected.

``` r
# Use the existing air_data from our environment
# Convert to appropriate format and calculate percentages
df <- data.frame(
  State = air_data$State,
  asthma_pct = air_data$Asthma,
  poor_air_pct = air_data$Poor_Air * 100  # Convert proportion to percentage
)

# Sort by asthma percentage (descending)
df <- df[order(-df$asthma_pct), ]

# Set up plotting parameters
par(mar = c(5, 10, 4, 6))  # Adjust margins for state names
y_pos <- 1:nrow(df)  # Y positions for bars

# Create empty plot
plot(0, 0, type = "n", 
     xlim = c(0, 50), 
     ylim = c(0.5, nrow(df) + 0.5),
     xlab = "Percent", 
     ylab = "",
     main = "Adults with Current Asthma vs. Poor-Air Days by State\n(Ranked by Asthma %)",
     yaxt = "n")  # No default y-axis

# Add asthma bars (blue)
barplot_height <- df$asthma_pct
rect(xleft = 0, ybottom = y_pos - 0.3, 
     xright = barplot_height, ytop = y_pos + 0.3,
     col = "lightblue", border = NA)

# Add air quality bars (red) - offset slightly
barplot_height_air <- df$poor_air_pct
rect(xleft = 0, ybottom = y_pos - 0.3 + 0.6, 
     xright = barplot_height_air, ytop = y_pos + 0.3 + 0.6,
     col = "orange", border = NA)

# Add state names
axis(2, at = y_pos, labels = df$State, las = 1, cex.axis = 0.6)

# Add legend
legend("bottomright", 
       legend = c("Adults with Current Asthma (%)", "Poor Air Quality Days (%)"),
       fill = c("blue", "orange"),
       cex = 0.8,
       bty = "n")
```

<figure>
<img
src="FinalRmarkdownProject_files/figure-gfm/asthma-ranked-bars-1.png"
alt="Adults with Current Asthma vs. Poor-Air Days by State (ranked by Asthma %)" />
<figcaption aria-hidden="true">Adults with Current Asthma vs. Poor-Air
Days by State (ranked by Asthma %)</figcaption>
</figure>

``` r
# Set up side-by-side plots
par(mfrow = c(1, 2), mar = c(5, 4, 4, 2))

# Histogram with density curve for Asthma
hist(df$asthma_pct, 
     breaks = 15,
     main = "Distribution of Asthma Prevalence",
     xlab = "Adults with Current Asthma (%)",
     ylab = "Density",
     col = "lightblue",
     border = "darkblue",
     freq = FALSE,
     xlim = c(0, max(df$asthma_pct) + 2))

# Add density curve
lines(density(df$asthma_pct), col = "darkblue", lwd = 2)


# Histogram with density curve for Poor Air Quality
hist(df$poor_air_pct, 
     breaks = 15,
     main = "Distribution of Poor Air Quality Days",
     xlab = "Poor Air Quality Days (%)",
     ylab = "Density",
     col = "lightcoral",
     border = "darkred",
     freq = FALSE,
     xlim = c(0, max(df$poor_air_pct) + 5))

# Add density curve
lines(density(df$poor_air_pct), col = "darkred", lwd = 2)
```

![](FinalRmarkdownProject_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

``` r
# Reset plotting parameters
par(mfrow = c(1, 1))
```

# STUDY QUESTION and HYPOTHESIS

## Questions

Does air quality effect the prevalence of asthma per state?

## Hypothesis

The states with the highest amounts of poor air quality days will also
have higher prevalence of asthma.

## Prediction

California will have the highest prevalence of asthma because of its’
high amounts of poor air quality days.

# METHODS/RESULTS

To begin the analysis if there was a correlation between asthma rates
and poor air quality, 2022 asthma and air quality data was taken from
The United States Environment Protection Agency. This data was used with
asthma prevalence data from the CDC that showed proportions of people in
the population who have asthma from each state in the U.S. The
proportion of poor air quality days was taken by first finding the total
days recorded and subtracting the good air quality days from it, then we
divided the poor days by the total amount of days. Then using this data
we made a linear regression scatter plot to get a p-value, r value, and
R^2 value. We also made a pearson correlation test from the data to find
confidence intervals, t-test, and p-values. Lastly, a Shaprio-Wilk test
was performed to test if the data had a normal distribution.

## 1st Analysis- Linear Regression.

The first analysis made was a linear regression scatter plot which
compares asthma rates in percentages by the percentage of poor air
quality days. A linear regression line is then fitted to the scatterplot
in order to find if there is a significant correlation between the two
values.

``` r
## 1st Analysis: Linear Regression - Air Quality vs Asthma Rates

# Convert Poor_Air from proportion to percentage
air_data$Poor_Air_Percent <- air_data$Poor_Air * 100

# Scatter plot with regression line
plot(air_data$Poor_Air_Percent, air_data$Asthma,
     xlab = "Percentage of Poor Air Quality Days",
     ylab = "Asthma Rate (%)",
     main = "Air Quality vs Asthma Rates by State",
     pch = 16, col = "blue", cex = 1.2)

# Add regression line
model <- lm(Asthma ~ Poor_Air_Percent, data = air_data)
abline(model, col = "red", lwd = 2)

# Calculate statistics
correlation <- cor(air_data$Poor_Air_Percent, air_data$Asthma)
r_squared <- summary(model)$r.squared
p_value <- summary(model)$coefficients[2,4]

# Create comprehensive results table
regression_stats <- data.frame(
  Statistic = c("Correlation Coefficient", "R-squared", "Adjusted R-squared", 
                "Overall Model p-value", "Regression p-value", "Observations"),
  Value = c(round(correlation, 3),
            round(r_squared, 3),
            round(summary(model)$adj.r.squared, 3),
            round(pf(summary(model)$fstatistic[1], summary(model)$fstatistic[2], 
                     summary(model)$fstatistic[3], lower.tail = FALSE), 4),
            round(p_value, 4),
            nrow(air_data))
)


# Create coefficients table
coefficients_table <- data.frame(
  Term = c("Intercept", "Poor Air Quality (%)"),
  Estimate = c(round(coef(model)[1], 3), round(coef(model)[2], 3)),
  Std_Error = c(round(summary(model)$coefficients[1,2], 3), 
                round(summary(model)$coefficients[2,2], 3)),
  t_value = c(round(summary(model)$coefficients[1,3], 3), 
              round(summary(model)$coefficients[2,3], 3)),
  p_value = c(round(summary(model)$coefficients[1,4], 4), 
              round(summary(model)$coefficients[2,4], 4))
)


# Add clean statistics to plot
legend("topright", 
       legend = c(paste("r =", round(correlation, 3)),
                  paste("R² =", round(r_squared, 3)),
                  paste("p =", ifelse(p_value < 0.001, "< 0.001", round(p_value, 3)))),
       bty = "n", cex = 0.9)
```

![](FinalRmarkdownProject_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

## 2nd Analysis - Perason correlation

The second analysis performed was a pearson correlation test to measure
the strength of the relationship between the linear variables.

``` r
## Pearson Correlation Analysis

# Calculate Pearson correlation
cor_test <- cor.test(air_data$Poor_Air, air_data$Asthma, method = "pearson")

# Create correlation results table
correlation_table <- data.frame(
  Statistic = c("Pearson Correlation Coefficient", 
                "95% Confidence Interval Lower",
                "95% Confidence Interval Upper",
                "t-statistic",
                "Degrees of Freedom",
                "p-value",
                "Sample Size"),
  Value = c(round(cor_test$estimate, 3),
            round(cor_test$conf.int[1], 3),
            round(cor_test$conf.int[2], 3),
            round(cor_test$statistic, 3),
            cor_test$parameter,
            ifelse(cor_test$p.value < 0.001, "< 0.001", round(cor_test$p.value, 4)),
            nrow(air_data))
)

# Display correlation results
knitr::kable(correlation_table, caption = "Pearson Correlation Analysis")
```

| Statistic                       |   Value |
|:--------------------------------|--------:|
| Pearson Correlation Coefficient | -0.2790 |
| 95% Confidence Interval Lower   | -0.5170 |
| 95% Confidence Interval Upper   | -0.0010 |
| t-statistic                     | -2.0130 |
| Degrees of Freedom              | 48.0000 |
| p-value                         |  0.0497 |
| Sample Size                     | 50.0000 |

Pearson Correlation Analysis

## 3rd Analysis- Shapiro-Wilk Normality Tests

The Shapiro-Wilk Normality Test was used to determine if the data had a
normal distribution between the variables of poor air quality days and
asthma rate.

``` r
# Shapiro-Wilk tests for normality
shapiro_poor_air <- shapiro.test(air_data$Poor_Air)
shapiro_asthma <- shapiro.test(air_data$Asthma)

# Create results table
normality_tests <- data.frame(
  Variable = c("Poor Air Quality Days", "Asthma Rate"),
  Statistic_W = c(round(shapiro_poor_air$statistic, 4), 
                  round(shapiro_asthma$statistic, 4)),
  P_Value = c(ifelse(shapiro_poor_air$p.value < 0.001, "< 0.001", 
                     round(shapiro_poor_air$p.value, 4)),
              ifelse(shapiro_asthma$p.value < 0.001, "< 0.001", 
                     round(shapiro_asthma$p.value, 4))),
  Normal_Distribution = c(ifelse(shapiro_poor_air$p.value > 0.05, "Yes", "No"),
                          ifelse(shapiro_asthma$p.value > 0.05, "Yes", "No"))
)

# Display results
knitr::kable(normality_tests, caption = "Shapiro-Wilk Normality Tests")
```

| Variable              | Statistic_W | P_Value | Normal_Distribution |
|:----------------------|------------:|--------:|:--------------------|
| Poor Air Quality Days |      0.9798 |  0.5452 | Yes                 |
| Asthma Rate           |      0.9768 |  0.4249 | Yes                 |

Shapiro-Wilk Normality Tests

# DISCUSSION

## Interpretation of 1st analysis- Linear Regression

The linear regression test shows a r value of -0.279, which shows that
there is a weak negative linear relationship between the two variables
of asthma rates (%) and percentage of poor air quality days. This means
that as the percentage of poor air quality days gets worse, the asthma
rates decrease. This is an unexpected result as it was expected that as
the days got worse the asthma rates would also increase and show a
strong positive linear relationship. The R^2 value was a value of .078,
which means that only about 7.8% of the variation in asthma rates can be
explained by the poor air quality days. Because this value is so low, it
can be inferred that there must be another factor that is contributing
to asthma rates besides poor air quality. The P-value is 0.05 which is
statistically significant and shows a correlation between the two
variables, this shows that there is an effect on asthma rates from poor
air quality, but it is not as strong as expected.

## Interpretation of 2nd analysis- Pearson Correlation.

The pearson correlation coefficient obtained from this test was -0.2790,
indicating a weak negative correlation between Asthma rates and
percentage of poor air quality days. This shows that as the percentage
of poor air quality days increases, asthma rates tend to decrease which
is an unexpected result obtained from this test. It is expected that
lower air quality would cause higher rates of asthma, not lower. The
Confidence interval is -0.5170 to -0.0010 shows that somewhere between
these two values is where the true value lies, also the t value was
-2.013, showing that there is a negative correlation between the
variables. The p-value was .0497, is almost statistically significant,
however it is a weak correlation.

## Interpretation of 3rd analysis- Shapiro-Wilk Test

The Shapiro-Wilk test showed a statistic_W value of .9798 for poor air
quality days and .9768 for asthma rates. Because the values were close
to one, it shows that the data closely follows a normal distribution.
The test also displayed a p-value of .5452 for poor air quality days and
.4249 for asthma rates. As the p-values were more than .05 this also
tells us that the variables are normally distributed.

# CONCLUSION

In conclusion, the data obtained from these analysis reveals that our
hypothesis, “The states with the highest amounts of poor air quality
days will also have higher prevalence of asthma,” is not supported by
the data. It was believed that there would have been a positive
correlation between the two variables, but the results of the tests
revealed that it had a negative correlation. Due to this, there must be
other variables that are significantly affecting the rates of asthma
other than poor air quality. The other variables could be inherited
biological factors, chemicals, ventilation, climate, and altitude for
instance. Because our analysis was limited to just looking at poor air
quality it is possible that by looking at other factors a significant
correlation could be found. Another limitation is that the data we
looked at included moderate and unhealthy air quality days instead of it
being generally “unhealthy” air quality days, which could have given
more support to our original hypothesis that the states with the highest
amounts of poor air quality days will also have higher prevalence of
asthma.

# REFERENCES

1.  ChatGPT. OpenAI, version Jan 2025. Used as a reference for functions
    such as plot() and to correct syntax errors. Accessed 2025-11-22.

2.  Google. (2025). Gemini (version Oct 2025). Tool used for quick
    fixes, editing grammar and flow of text, and checking all rubric
    requirements were met.

3.  “Asthma.” Mayo Clinic, Mayo Foundation for Medical Education and
    Research, 8 Mar. 2025,
    www.mayoclinic.org/diseases-conditions/asthma/symptoms-causes/syc-20369653.

4.  Tiotiu AI, Novakova P, Nedeva D, Chong-Neto HJ, Novakova S,
    Steiropoulos P, Kowal K. Impact of Air Pollution on Asthma Outcomes.
    Int J Environ Res Public Health. 2020 Aug 27;17(17):6212. doi:
    10.3390/ijerph17176212. PMID: 32867076; PMCID: PMC7503605.

5.  “Jama Network \| Home of Jama and the Specialty Journals of the
    American Medical Association.” Jama Network, jamanetwork.com/.
    Accessed 12 Nov. 2025.

6.  “Asthma Facts.” Asthma & Allergy Foundation of America, 23
    Apr. 2025, aafa.org/asthma/asthma-facts/.
