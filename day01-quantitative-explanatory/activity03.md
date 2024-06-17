Activity 3 - MLR
================

# Day 1

## Load the necessary packages

I encourage you to continue using the two packages from Posit (formerly
[RStudio](https://posit.co/)): `{tidyverse}` and `{tidymodels}`.
Remember that [Emil Hvitfeldt](https://www.emilhvitfeldt.com/) (of
Posit) has put together a [complementary online
text](https://emilhvitfeldt.github.io/ISLR-tidymodels-labs/index.html)
for the labs in the *ISLR* text that utilize `{tidyverse}` and
`{tidymodels}` instead of base R.

- In the **Packages** pane of RStudio, check if `{tidyverse}` and
  `{tidymodels}` are installed. Be sure to check both your **User
  Library** and **System Library**.

- If either of these are not currently listed (they should be because
  you verified this in Activity 1), type the following in your
  **Console** pane, replacing `package_name` with the appropriate name,
  and press Enter/Return afterwards.

  ``` r
  install.packages("package_name")
  ```

- Once you have verified that both `{tidyverse}` and `{tidymodels}` are
  installed (in either your user or system library), load these packages
  in the R chunk below titled `load-packages`.

- Run the `load-packages` code chunk or **knit**
  <img src="../README-img/knit-icon.png" alt="knit" width = "20"/> icon
  your Rmd document to verify that no errors occur.

``` r
library(tidyverse)
library(GGally)
library(tidymodels)
```

Since we will be looking at many relationships graphically, it will be
nice to not have to code each of these individually. `{GGally}` is an
extension to `{ggplot2}` that reduces some of the complexities when
combining multiple plots. For example,
[`GGally::ggpairs`](http://ggobi.github.io/ggally/articles/ggpairs.html)
is very handy for pairwise comparisons of multiple variables.

- In the **Packages** pane of RStudio, check if `{GGally}` is already
  installed. Be sure to check both your **User Library** and **System
  Library**.

- If this is not currently listed, type the following in your
  **Console** pane and press Enter/Return afterwards.

  ``` r
  install.packages("GGally")
  ```

- Once you have verified that `{GGally}` is installed, load it in the R
  chunk titled `load-packages`.

- Run the `setup` code chunk or **knit**
  <img src="../README-img/knit-icon.png" alt="knit" width = "20"/> icon
  your Rmd document to verify that no errors occur.

## Load the data and

I found a way to upload data from OpenIntro without needing to download
it first! Recall that data we are working with is from the OpenIntro
site (its “about” page:
<https://www.openintro.org/data/index.php?data=hfi>). We can access the
raw data from their tab-delimited text file link:
<https://www.openintro.org/data/tab-delimited/hfi.txt>.

Create a new R code chunk below that is titled `load-data` and reads in
the above linked TSV (tab-separated values) file by doing the following:

- Rather than downloading this file, uploading to RStudio, then reading
  it in, explore how to load this file directly from the provided URL
  with `readr::read_tsv` (`{readr}` is part of `{tidyverse}`).
- Assign this data set into a data frame named `hfi` (short for “Human
  Freedom Index”).
- Filter the data `hfi` data frame for year 2016 and assigns the result
  to an R data object named `hfi_2016`. You will use `hfi_2016` for the
  remainder of this activity.

``` r
hfi <- read_csv('hfi.csv')
```

    ## Rows: 1458 Columns: 123
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr   (3): ISO_code, countries, region
    ## dbl (120): year, pf_rol_procedural, pf_rol_civil, pf_rol_criminal, pf_rol, p...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

We will continue using personal freedom scores, `pf_score`, as the
response variable and build on our model that had
`pf_expression_control` as the explanatory variable.

Create a new R code chunk below, with an appropriate title, that does
the following:

- Review the about page of the data set and select at least one
  additional numeric variables (hint: look for `<dbl>` or `<int>`
  designations) to describe its distribution. Remember to write your
  description.
- You may also wish to do this for `pf_score` and
  `pf_expression_control` again to help you remember what you noticed
  last week.

``` r
hfi_2016 <- hfi %>% filter(year == 2016)

glimpse(hfi_2016)
```

    ## Rows: 162
    ## Columns: 123
    ## $ year                               <dbl> 2016, 2016, 2016, 2016, 2016, 2016,…
    ## $ ISO_code                           <chr> "ALB", "DZA", "AGO", "ARG", "ARM", …
    ## $ countries                          <chr> "Albania", "Algeria", "Angola", "Ar…
    ## $ region                             <chr> "Eastern Europe", "Middle East & No…
    ## $ pf_rol_procedural                  <dbl> 6.661503, NA, NA, 7.098483, NA, 8.4…
    ## $ pf_rol_civil                       <dbl> 4.547244, NA, NA, 5.791960, NA, 7.5…
    ## $ pf_rol_criminal                    <dbl> 4.666508, NA, NA, 4.343930, NA, 7.3…
    ## $ pf_rol                             <dbl> 5.291752, 3.819566, 3.451814, 5.744…
    ## $ pf_ss_homicide                     <dbl> 8.920429, 9.456254, 8.060260, 7.622…
    ## $ pf_ss_disappearances_disap         <dbl> 10, 10, 5, 10, 10, 10, 10, 10, 10, …
    ## $ pf_ss_disappearances_violent       <dbl> 10.000000, 9.294030, 10.000000, 10.…
    ## $ pf_ss_disappearances_organized     <dbl> 10.0, 5.0, 7.5, 7.5, 7.5, 10.0, 10.…
    ## $ pf_ss_disappearances_fatalities    <dbl> 10.000000, 9.926119, 10.000000, 10.…
    ## $ pf_ss_disappearances_injuries      <dbl> 10.000000, 9.990149, 10.000000, 9.9…
    ## $ pf_ss_disappearances               <dbl> 10.000000, 8.842060, 8.500000, 9.49…
    ## $ pf_ss_women_fgm                    <dbl> 10.0, 10.0, 10.0, 10.0, 10.0, 10.0,…
    ## $ pf_ss_women_missing                <dbl> 7.5, 7.5, 10.0, 10.0, 5.0, 10.0, 10…
    ## $ pf_ss_women_inheritance_widows     <dbl> 5, 0, 5, 10, 10, 10, 10, 5, NA, 0, …
    ## $ pf_ss_women_inheritance_daughters  <dbl> 5, 0, 5, 10, 10, 10, 10, 10, NA, 0,…
    ## $ pf_ss_women_inheritance            <dbl> 5.0, 0.0, 5.0, 10.0, 10.0, 10.0, 10…
    ## $ pf_ss_women                        <dbl> 7.500000, 5.833333, 8.333333, 10.00…
    ## $ pf_ss                              <dbl> 8.806810, 8.043882, 8.297865, 9.040…
    ## $ pf_movement_domestic               <dbl> 5, 5, 0, 10, 5, 10, 10, 5, 10, 10, …
    ## $ pf_movement_foreign                <dbl> 10, 5, 5, 10, 5, 10, 10, 5, 10, 5, …
    ## $ pf_movement_women                  <dbl> 5, 5, 10, 10, 10, 10, 10, 5, NA, 5,…
    ## $ pf_movement                        <dbl> 6.666667, 5.000000, 5.000000, 10.00…
    ## $ pf_religion_estop_establish        <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA,…
    ## $ pf_religion_estop_operate          <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA,…
    ## $ pf_religion_estop                  <dbl> 10.0, 5.0, 10.0, 7.5, 5.0, 10.0, 10…
    ## $ pf_religion_harassment             <dbl> 9.566667, 6.873333, 8.904444, 9.037…
    ## $ pf_religion_restrictions           <dbl> 8.011111, 2.961111, 7.455556, 6.850…
    ## $ pf_religion                        <dbl> 9.192593, 4.944815, 8.786667, 7.795…
    ## $ pf_association_association         <dbl> 10.0, 5.0, 2.5, 7.5, 7.5, 10.0, 10.…
    ## $ pf_association_assembly            <dbl> 10.0, 5.0, 2.5, 10.0, 7.5, 10.0, 10…
    ## $ pf_association_political_establish <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA,…
    ## $ pf_association_political_operate   <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA,…
    ## $ pf_association_political           <dbl> 10.0, 5.0, 2.5, 5.0, 5.0, 10.0, 10.…
    ## $ pf_association_prof_establish      <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA,…
    ## $ pf_association_prof_operate        <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA,…
    ## $ pf_association_prof                <dbl> 10.0, 5.0, 5.0, 7.5, 5.0, 10.0, 10.…
    ## $ pf_association_sport_establish     <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA,…
    ## $ pf_association_sport_operate       <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA,…
    ## $ pf_association_sport               <dbl> 10.0, 5.0, 7.5, 7.5, 7.5, 10.0, 10.…
    ## $ pf_association                     <dbl> 10.0, 5.0, 4.0, 7.5, 6.5, 10.0, 10.…
    ## $ pf_expression_killed               <dbl> 10.000000, 10.000000, 10.000000, 10…
    ## $ pf_expression_jailed               <dbl> 10.000000, 10.000000, 10.000000, 10…
    ## $ pf_expression_influence            <dbl> 5.0000000, 2.6666667, 2.6666667, 5.…
    ## $ pf_expression_control              <dbl> 5.25, 4.00, 2.50, 5.50, 4.25, 7.75,…
    ## $ pf_expression_cable                <dbl> 10.0, 10.0, 7.5, 10.0, 7.5, 10.0, 1…
    ## $ pf_expression_newspapers           <dbl> 10.0, 7.5, 5.0, 10.0, 7.5, 10.0, 10…
    ## $ pf_expression_internet             <dbl> 10.0, 7.5, 7.5, 10.0, 7.5, 10.0, 10…
    ## $ pf_expression                      <dbl> 8.607143, 7.380952, 6.452381, 8.738…
    ## $ pf_identity_legal                  <dbl> 0, NA, 10, 10, 7, 7, 10, 0, NA, NA,…
    ## $ pf_identity_parental_marriage      <dbl> 10, 0, 10, 10, 10, 10, 10, 10, 10, …
    ## $ pf_identity_parental_divorce       <dbl> 10, 5, 10, 10, 10, 10, 10, 10, 10, …
    ## $ pf_identity_parental               <dbl> 10.0, 2.5, 10.0, 10.0, 10.0, 10.0, …
    ## $ pf_identity_sex_male               <dbl> 10, 0, 0, 10, 10, 10, 10, 10, 10, 1…
    ## $ pf_identity_sex_female             <dbl> 10, 0, 0, 10, 10, 10, 10, 10, 10, 1…
    ## $ pf_identity_sex                    <dbl> 10, 0, 0, 10, 10, 10, 10, 10, 10, 1…
    ## $ pf_identity_divorce                <dbl> 5, 0, 10, 10, 5, 10, 10, 5, NA, 0, …
    ## $ pf_identity                        <dbl> 6.2500000, 0.8333333, 7.5000000, 10…
    ## $ pf_score                           <dbl> 7.596281, 5.281772, 6.111324, 8.099…
    ## $ pf_rank                            <dbl> 57, 147, 117, 42, 84, 11, 8, 131, 6…
    ## $ ef_government_consumption          <dbl> 8.232353, 2.150000, 7.600000, 5.335…
    ## $ ef_government_transfers            <dbl> 7.509902, 7.817129, 8.886739, 6.048…
    ## $ ef_government_enterprises          <dbl> 8, 0, 0, 6, 8, 10, 10, 0, 7, 10, 7,…
    ## $ ef_government_tax_income           <dbl> 9, 7, 10, 7, 5, 5, 4, 9, 10, 10, 8,…
    ## $ ef_government_tax_payroll          <dbl> 7, 2, 9, 1, 5, 5, 3, 4, 10, 10, 8, …
    ## $ ef_government_tax                  <dbl> 8.0, 4.5, 9.5, 4.0, 5.0, 5.0, 3.5, …
    ## $ ef_government                      <dbl> 7.935564, 3.616782, 6.496685, 5.346…
    ## $ ef_legal_judicial                  <dbl> 2.6682218, 4.1867042, 1.8431292, 3.…
    ## $ ef_legal_courts                    <dbl> 3.145462, 4.327113, 1.974566, 2.930…
    ## $ ef_legal_protection                <dbl> 4.512228, 4.689952, 2.512364, 4.255…
    ## $ ef_legal_military                  <dbl> 8.333333, 4.166667, 3.333333, 7.500…
    ## $ ef_legal_integrity                 <dbl> 4.166667, 5.000000, 4.166667, 3.333…
    ## $ ef_legal_enforcement               <dbl> 4.3874441, 4.5075380, 2.3022004, 3.…
    ## $ ef_legal_restrictions              <dbl> 6.485287, 6.626692, 5.455882, 6.857…
    ## $ ef_legal_police                    <dbl> 6.933500, 6.136845, 3.016104, 3.385…
    ## $ ef_legal_crime                     <dbl> 6.215401, 6.737383, 4.291197, 4.133…
    ## $ ef_legal_gender                    <dbl> 0.9487179, 0.8205128, 0.8461538, 0.…
    ## $ ef_legal                           <dbl> 5.071814, 4.690743, 2.963635, 3.904…
    ## $ ef_money_growth                    <dbl> 8.986454, 6.955962, 9.385679, 5.233…
    ## $ ef_money_sd                        <dbl> 9.484575, 8.339152, 4.986742, 5.224…
    ## $ ef_money_inflation                 <dbl> 9.743600, 8.720460, 3.054000, 2.000…
    ## $ ef_money_currency                  <dbl> 10, 5, 5, 10, 10, 10, 10, 5, 0, 10,…
    ## $ ef_money                           <dbl> 9.553657, 7.253894, 5.606605, 5.614…
    ## $ ef_trade_tariffs_revenue           <dbl> 9.626667, 8.480000, 8.993333, 6.060…
    ## $ ef_trade_tariffs_mean              <dbl> 9.24, 6.22, 7.72, 7.26, 8.76, 9.50,…
    ## $ ef_trade_tariffs_sd                <dbl> 8.0240, 5.9176, 4.2544, 5.9448, 8.0…
    ## $ ef_trade_tariffs                   <dbl> 8.963556, 6.872533, 6.989244, 6.421…
    ## $ ef_trade_regulatory_nontariff      <dbl> 5.574481, 4.962589, 3.132738, 4.466…
    ## $ ef_trade_regulatory_compliance     <dbl> 9.4053278, 0.0000000, 0.9171598, 5.…
    ## $ ef_trade_regulatory                <dbl> 7.489905, 2.481294, 2.024949, 4.811…
    ## $ ef_trade_black                     <dbl> 10.00000, 5.56391, 10.00000, 0.0000…
    ## $ ef_trade_movement_foreign          <dbl> 6.306106, 3.664829, 2.946919, 5.358…
    ## $ ef_trade_movement_capital          <dbl> 4.6153846, 0.0000000, 3.0769231, 0.…
    ## $ ef_trade_movement_visit            <dbl> 8.2969231, 1.1062564, 0.1106256, 7.…
    ## $ ef_trade_movement                  <dbl> 6.406138, 1.590362, 2.044823, 4.697…
    ## $ ef_trade                           <dbl> 8.214900, 4.127025, 5.264754, 3.982…
    ## $ ef_regulation_credit_ownership     <dbl> 5, 0, 8, 5, 10, 10, 8, 5, 10, 10, 5…
    ## $ ef_regulation_credit_private       <dbl> 7.295687, 5.301526, 9.194715, 4.259…
    ## $ ef_regulation_credit_interest      <dbl> 9, 10, 4, 7, 10, 10, 10, 9, 10, 10,…
    ## $ ef_regulation_credit               <dbl> 7.098562, 5.100509, 7.064905, 5.419…
    ## $ ef_regulation_labor_minwage        <dbl> 5.566667, 5.566667, 8.900000, 2.766…
    ## $ ef_regulation_labor_firing         <dbl> 5.396399, 3.896912, 2.656198, 2.191…
    ## $ ef_regulation_labor_bargain        <dbl> 6.234861, 5.958321, 5.172987, 3.432…
    ## $ ef_regulation_labor_hours          <dbl> 8, 6, 4, 10, 10, 10, 6, 6, 8, 8, 10…
    ## $ ef_regulation_labor_dismissal      <dbl> 6.299741, 7.755176, 6.632764, 2.517…
    ## $ ef_regulation_labor_conscription   <dbl> 10, 1, 0, 10, 0, 10, 3, 1, 10, 10, …
    ## $ ef_regulation_labor                <dbl> 6.916278, 5.029513, 4.560325, 5.151…
    ## $ ef_regulation_business_adm         <dbl> 6.072172, 3.722341, 2.758428, 2.404…
    ## $ ef_regulation_business_bureaucracy <dbl> 6.000000, 1.777778, 1.333333, 6.666…
    ## $ ef_regulation_business_start       <dbl> 9.713864, 9.243070, 8.664627, 9.122…
    ## $ ef_regulation_business_bribes      <dbl> 4.050196, 3.765515, 1.945540, 3.260…
    ## $ ef_regulation_business_licensing   <dbl> 7.324582, 8.523503, 8.096776, 5.253…
    ## $ ef_regulation_business_compliance  <dbl> 7.074366, 7.029528, 6.782923, 6.508…
    ## $ ef_regulation_business             <dbl> 6.705863, 5.676956, 4.930271, 5.535…
    ## $ ef_regulation                      <dbl> 6.906901, 5.268992, 5.518500, 5.369…
    ## $ ef_score                           <dbl> 7.54, 4.99, 5.17, 4.84, 7.57, 7.98,…
    ## $ ef_rank                            <dbl> 34, 159, 155, 160, 29, 10, 27, 106,…
    ## $ hf_score                           <dbl> 7.568140, 5.135886, 5.640662, 6.469…
    ## $ hf_rank                            <dbl> 48, 155, 142, 107, 57, 4, 16, 130, …
    ## $ hf_quartile                        <dbl> 2, 4, 4, 3, 2, 1, 1, 4, 2, 2, 4, 2,…

## Pairwise relationships

In Activity 2 you explored simple linear regression models.
Specifically, you fit and assessed this relationship:

$$
y = \beta_0 + \beta_1 \times x + \varepsilon
$$

![check-in](../README-img/noun-magnifying-glass.png) **Check in**

Review how you described this model in Activity 2. - What were your
parameter estimates (i.e., the $\beta$s)? How did you interpret these
and what did they imply for this scenario? - How good of a fit was this
model? What did you use to assess this?

For this activity, we will begin using the two other quantitative
variables to describe the patterns in the response variable. Take a
moment to think about what this previous sentence means:

- What does this mean from a statistical point of view?
- What does this mean from a “real world” point of view (i.e., for your
  data’s situation)?

Now, we will obtain graphical and numerical summaries to describe the
pairwise relationships.

- In the code chunk below titled `pairs-plot`, replace “verbatim” with
  “r” just before the code chunk title.
- Replace `explanatory` in the `select` line with the variable you
  identified above
- Run your code chunk or knit your document.

``` r
hfi_2016 %>% 

  select(pf_score, pf_expression_control, pf_rank) %>% 
  ggpairs()
```

![](activity03_files/figure-gfm/pairs-plot-1.png)<!-- -->

``` r
hfi_2016 %>% 
  select(pf_score, pf_expression_control, pf_rank) %>% 
  ggpairs()
```

![](activity03_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

Note that a warning message (really a list of warning messages) might
display in your **Console** and likely under your R code chunk when you
knit this report. In R, warning messages are not necessarily a bad thing
and you should read these to make sure you understand what it is
informing you of. To suppress warning messages from displaying after
this specific R code chunk when you knit your report, add the follow
inside the curly brackets (`{r }`) at the top of your R code chunk
(notice the preceding comma): `, warning=FALSE`.

Somewhat related… If you do not want all the messages `{tidyverse}` and
`{tidymodels}` produce when you load them, you can add `, message=FALSE`
to your `load-packages` R code chunk.

After running the `pairs-plot` code, answer the following questions:

1.  For each pair of variables, how would you describe the relationship
    graphically?

Do any of the relationships look linear? Yes and there are no outliers.
Are there any interesting/odd features (outliers, non-linear patterns,
etc.)?

2.  For each pair of variables, how would you describe the relationship
    numerically?

3.  Are your two explanatory variables collinear (correlated)? Yes with
    a correlation of 0.6

There’s a positive relationship between pf score and pf control.
Additionally, there is a negative relationship between pf rank and pf
score. As one’s score increases, their rank decreases. Furthermore, the
expression control has a negative relation with rank. As expression
control increases, the rank decreases.

Do any of the relationships look linear? Yes and there are no outliers.
Are there any interesting/odd features (outliers, non-linear patterns,
etc.)? There are no outliers.

2.  For each pair of variables, how would you describe the relationship
    numerically?

There’s a positive relationship between pf score and pf control.
Additionally, there is a negative relationship between pf rank and pf
score. As one’s score increases, their rank decreases. Furthermore, the
expression control has a negative relation with rank. As expression
control increases, the rank decreases.

3.  Are your two explanatory variables collinear (correlated)?

Yes, they are negatively correlated.

Essentially, this means that adding more than one of these variables to
the model would not add much value to the model. We will talk more on
this issue in Activity 4 (other considerations in regression models).

## The multiple linear regression model

You will now fit the following model:

$$
y = \beta_0 + \beta_1 \times x_1 + \beta_2 \times x_2 + \varepsilon
$$

- In the code chunk below titled `mlr-model`, replace “verbatim” with
  “r” just before the code chunk title.
- Replace `explanatory`, similarly to what you did in your `pairs-plot`
  R code chunk.
- Run your code chunk or knit your document.

``` r
#fit the mlr model
lm_spec <- linear_reg() %>%
  set_mode("regression") %>%
  set_engine("lm")

lm_spec
```

    ## Linear Regression Model Specification (regression)
    ## 
    ## Computational engine: lm

``` r
mlr_mod <- lm_spec %>% 
fit(pf_score ~ pf_expression_control + ef_legal, data = hfi_2016)

# model output
tidy(mlr_mod)
```

    ## # A tibble: 3 × 5
    ##   term                  estimate std.error statistic  p.value
    ##   <chr>                    <dbl>     <dbl>     <dbl>    <dbl>
    ## 1 (Intercept)              3.36     0.202      16.7  9.56e-37
    ## 2 pf_expression_control    0.428    0.0306     14.0  1.76e-29
    ## 3 ef_legal                 0.286    0.0465      6.14 6.20e- 9

After doing this, answer the following questions:

4.  Using your output, write the complete estimated equation for this
    model. Remember in Activity 2 that this looked like:

\##$$
\hat{y} = b_0 + b_1 \times x_1
$$ = 3.361 + 0.428 x_1\$\$

where $b_0$ and $b_1$ were your model parameter estimates. Note that
your model here will be different (and have more terms).

5.  For each of the estimated parameters (the *y*-intercept and the
    slopes associated with each explanatory variable - three total),
    interpret these values in the context of this problem. That is, what
    do they mean for a “non-data” person?

this formula can now be used to predict the personal freedom score based
on the value of the political pressures and controls on media content
index.

### Intercept 3.361, the intercept represents the estimated personal freedom score when the political pressures and controls on media content index () is 0. If there were no political pressures and controls on media content (i.e.,  = 0), the personal freedom score would be approximately 0.428. This provides a baseline level of personal freedom in the absence of political pressures on media content.

### Slope 0.428, the slope represents the estimated change in the personal freedom score for each one-unit increase in the political pressures and controls on media content index (). Specifically, for every one-unit increase in the  index, the personal freedom score is expected to increase by approximately 0.428 units.

### This positive relationship suggests that higher political pressures and controls on media content are associated with higher personal freedom scores. This might seem counterintuitive at first, but it could imply that in environments where there are more political pressures and controls on media, there could be compensatory mechanisms or efforts in place to ensure personal freedoms are maintained or improved.

### The intercept provides a starting point for personal freedom scores in the theoretical scenario where political pressures and controls on media content are completely absent.

### The slope indicates the direction and strength of the relationship between political pressures on media and personal freedom scores. The positive slope suggests a direct, although not necessarily causal, relationship where increasing political pressures and controls on media content correlate with increasing personal freedom scores.

### Summary \> For countries with a `pf_expression_control` of 0 (those with the largest amount of political pressure on media content), we expect their mean personal freedom score to be 0.428. \> For every 1 unit increase in `pf_expression_control` (political pressure on media content index), we expect a country’s mean personal freedom score to increase 0.428 units.

## Challenge: 3-D plots

In *ISL*, the authors provided a 3-D scatterplot with a plane that
represents the estimated model. Do some internet sleuthing to minimally
produce a 3-D scatterplot (you do not need to include the plane).
Ideally, this would be something that plays nicely with (looks similar
to) `{ggplot2}`.

- Create a new R code chunk, with a descriptive name, and add your code
  to create this plot.

``` r
library(ggplot2)
library(plotly)
```

    ## 
    ## Attaching package: 'plotly'

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     last_plot

    ## The following object is masked from 'package:stats':
    ## 
    ##     filter

    ## The following object is masked from 'package:graphics':
    ## 
    ##     layout

``` r
two_d <- ggplot(hfi_2016, aes(x = pf_expression_control, y = pf_score, color = pf_rank)) +
  geom_point(size = 2) +
  labs(title = "3D Scatter Plot",
       x = "Weight",
       y = "Miles per Gallon",
       color = "Horsepower")
# Convert ggplot to plotly object for 3D plotting
ggplotly(two_d) %>%
  layout(
    scene = list(
      xaxis = list(title = 'Weight'),
      yaxis = list(title = 'Miles per Gallon'),
      zaxis = list(title = 'Horsepower')
    )
  )
```

    ## PhantomJS not found. You can install it with webshot::install_phantomjs(). If it is installed, please make sure the phantomjs executable can be found via the PATH variable.

<div class="plotly html-widget html-fill-item-overflow-hidden html-fill-item" id="htmlwidget-cec1cf6da010ae09497b" style="width:672px;height:480px;"></div>
<script type="application/json" data-for="htmlwidget-cec1cf6da010ae09497b">{"x":{"data":[{"x":[5.25,4,2.5,5.5,4.25,7.75,8,0.25,7.25,0.75,3.25,7.5,2.5,9.25,6.75,7.25,5,4.25,4.25,5.25,4.5,3.75,5.75,7,0.75,3.25,4.25,8.25,7.75,3,2.5,6.75,1.5,3.5,3.75,2,8.5,5.25,5.75,7.5,8,8.5,4.75,3.25,1.5,5.5,9,0.5,6,9,6.75,3.75,1.25,4.75,7.5,6.75,5.5,4.25,2.75,4,6,5.75,1.75,5,5.75,8.75,4.5,5.5,1,2,8.5,6.5,7.5,7.75,6,3.5,1.5,4,6.5,4.25,3.25,1.5,8,4.75,4.5,4.5,1.25,8.5,8.75,3.75,4.25,6.25,3.75,5.25,7.75,5,7.5,2.25,4.75,7,5.25,3.75,4.75,2.75,6.75,4.75,8.75,8,4,5.25,4.5,9.25,3.25,2.5,6.25,6.75,4,5.25,5,6.5,8.25,3.25,6,1.5,1.5,2,6.25,4.75,6,5,4.5,7.75,8,5,6.5,3.75,1,6.75,2.5,8.75,9,0.5,8,1.75,4.5,1.75,7.25,5.25,7.25,5.5,1.75,3.75,3.75,2.5,7.75,7,7.5,2.25,1.75,1,4,3.75],"y":[7.5962805759999998,5.2817721090000003,6.1113243639999997,8.0996956820000001,6.9128036020000003,9.1844384530000003,9.2469484009999992,5.6765534779999998,7.454537846,6.1360699939999996,5.3026003580000003,7.7068944650000004,6.0590281629999998,8.9871785870000007,7.4308638360000003,7.4969762949999996,6.6009728150000004,7.206770015,7.8614471449999996,6.8763344530000001,6.6659774059999997,4.6636997180000002,8.1555390770000002,7.4553396699999999,4.4141344269999996,7.2384482859999997,5.3307743109999999,9.1517266419999999,7.9865830659999997,5.465632394,5.50954107,8.2160353409999995,5.3508203390000002,7.0215132730000001,4.9472567859999996,6.7778676940000002,8.1654289690000006,7.0625420480000001,8.456175752,8.5143939159999995,9.0297614139999993,9.3256399689999991,6.9425749620000001,7.5505553589999996,3.8945537730000002,6.917901981,9.0137012179999996,5.0640899409999998,7.7922043949999997,9.2943679170000006,8.7668029270000005,5.3155752219999997,5.3026847019999996,7.576535507,9.2351913779999997,7.8721778149999997,7.9467681499999996,6.5482459820000001,5.3712016069999997,6.8597217239999999,7.0417958729999999,7.1844425879999996,6.3864825969999997,8.5836795909999992,8.2589457730000007,9.083506216,6.1958713349999996,6.3813124600000002,4.5324492449999996,3.1160276200000001,8.9391294039999991,7.5449161419999999,8.6904463169999993,7.2684160929999999,8.7337705499999991,6.2386665170000004,6.375675545,6.44553669,8.7663498579999999,5.6305744740000003,6.2470129539999997,5.8635861709999997,8.8510680839999996,6.4299941790000004,6.6884439599999999,6.403401744,3.8805656229999999,8.8213389259999992,9.2574018220000003,7.4136580240000001,6.8351235089999998,7.4452136089999996,5.9029799540000001,6.0609294709999997,8.9779974619999994,4.9877497760000002,7.7174656319999997,6.7960203970000004,7.0590295249999997,7.9983125529999999,7.6301041329999997,5.9867565120000004,6.6592066450000003,5.4633608689999997,7.3949155019999999,6.91161139,9.3988423599999997,9.2848191569999994,6.4324947259999998,5.7145200970000003,5.8236172059999998,9.3424811329999997,5.5090552129999999,5.3245924479999998,7.7152400739999996,7.2549488159999997,6.9745981869999998,7.7207866950000001,6.4975266940000003,8.3531204549999991,9.0437122429999999,5.5255529770000003,8.6532324989999996,5.7143816650000003,6.4677390509999997,4.4387320690000003,6.7741877690000001,7.8479432520000003,7.3725243349999996,7.0446591520000004,7.4794236349999998,8.5364998910000001,8.8227914670000001,7.695143614,8.7582137640000006,6.0407218330000001,4.2460471709999998,7.7885630460000002,6.0203286570000003,9.3347502979999994,9.1855180989999994,2.5116537540000001,9.0413940369999999,5.6523245790000001,6.1264186929999997,6.3995626010000004,6.186449627,6.7299380099999997,6.9204463880000002,6.577472244,6.0924653089999996,6.1288890939999998,6.5862704079999999,5.0733501759999999,8.9958358720000007,8.7473102160000007,8.2961418059999996,5.5214489130000004,5.9680076,2.1665553399999999,6.0076988560000002,5.1707255749999996],"text":["pf_expression_control: 5.25<br />pf_score: 7.596281<br />pf_rank:  57","pf_expression_control: 4.00<br />pf_score: 5.281772<br />pf_rank: 147","pf_expression_control: 2.50<br />pf_score: 6.111324<br />pf_rank: 117","pf_expression_control: 5.50<br />pf_score: 8.099696<br />pf_rank:  42","pf_expression_control: 4.25<br />pf_score: 6.912804<br />pf_rank:  84","pf_expression_control: 7.75<br />pf_score: 9.184438<br />pf_rank:  11","pf_expression_control: 8.00<br />pf_score: 9.246948<br />pf_rank:   8","pf_expression_control: 0.25<br />pf_score: 5.676553<br />pf_rank: 131","pf_expression_control: 7.25<br />pf_score: 7.454538<br />pf_rank:  64","pf_expression_control: 0.75<br />pf_score: 6.136070<br />pf_rank: 114","pf_expression_control: 3.25<br />pf_score: 5.302600<br />pf_rank: 146","pf_expression_control: 7.50<br />pf_score: 7.706894<br />pf_rank:  54","pf_expression_control: 2.50<br />pf_score: 6.059028<br />pf_rank: 120","pf_expression_control: 9.25<br />pf_score: 8.987179<br />pf_rank:  19","pf_expression_control: 6.75<br />pf_score: 7.430864<br />pf_rank:  66","pf_expression_control: 7.25<br />pf_score: 7.496976<br />pf_rank:  61","pf_expression_control: 5.00<br />pf_score: 6.600973<br />pf_rank:  96","pf_expression_control: 4.25<br />pf_score: 7.206770<br />pf_rank:  73","pf_expression_control: 4.25<br />pf_score: 7.861447<br />pf_rank:  47","pf_expression_control: 5.25<br />pf_score: 6.876334<br />pf_rank:  86","pf_expression_control: 4.50<br />pf_score: 6.665977<br />pf_rank:  94","pf_expression_control: 3.75<br />pf_score: 4.663700<br />pf_rank: 153","pf_expression_control: 5.75<br />pf_score: 8.155539<br />pf_rank:  41","pf_expression_control: 7.00<br />pf_score: 7.455340<br />pf_rank:  63","pf_expression_control: 0.75<br />pf_score: 4.414134<br />pf_rank: 156","pf_expression_control: 3.25<br />pf_score: 7.238448<br />pf_rank:  72","pf_expression_control: 4.25<br />pf_score: 5.330774<br />pf_rank: 142","pf_expression_control: 8.25<br />pf_score: 9.151727<br />pf_rank:  12","pf_expression_control: 7.75<br />pf_score: 7.986583<br />pf_rank:  44","pf_expression_control: 3.00<br />pf_score: 5.465632<br />pf_rank: 138","pf_expression_control: 2.50<br />pf_score: 5.509541<br />pf_rank: 136","pf_expression_control: 6.75<br />pf_score: 8.216035<br />pf_rank:  39","pf_expression_control: 1.50<br />pf_score: 5.350820<br />pf_rank: 141","pf_expression_control: 3.50<br />pf_score: 7.021513<br />pf_rank:  79","pf_expression_control: 3.75<br />pf_score: 4.947257<br />pf_rank: 152","pf_expression_control: 2.00<br />pf_score: 6.777868<br />pf_rank:  90","pf_expression_control: 8.50<br />pf_score: 8.165429<br />pf_rank:  40","pf_expression_control: 5.25<br />pf_score: 7.062542<br />pf_rank:  75","pf_expression_control: 5.75<br />pf_score: 8.456176<br />pf_rank:  35","pf_expression_control: 7.50<br />pf_score: 8.514394<br />pf_rank:  34","pf_expression_control: 8.00<br />pf_score: 9.029761<br />pf_rank:  16","pf_expression_control: 8.50<br />pf_score: 9.325640<br />pf_rank:   4","pf_expression_control: 4.75<br />pf_score: 6.942575<br />pf_rank:  81","pf_expression_control: 3.25<br />pf_score: 7.550555<br />pf_rank:  59","pf_expression_control: 1.50<br />pf_score: 3.894554<br />pf_rank: 158","pf_expression_control: 5.50<br />pf_score: 6.917902<br />pf_rank:  83","pf_expression_control: 9.00<br />pf_score: 9.013701<br />pf_rank:  17","pf_expression_control: 0.50<br />pf_score: 5.064090<br />pf_rank: 150","pf_expression_control: 6.00<br />pf_score: 7.792204<br />pf_rank:  49","pf_expression_control: 9.00<br />pf_score: 9.294368<br />pf_rank:   5","pf_expression_control: 6.75<br />pf_score: 8.766803<br />pf_rank:  25","pf_expression_control: 3.75<br />pf_score: 5.315575<br />pf_rank: 144","pf_expression_control: 1.25<br />pf_score: 5.302685<br />pf_rank: 145","pf_expression_control: 4.75<br />pf_score: 7.576536<br />pf_rank:  58","pf_expression_control: 7.50<br />pf_score: 9.235191<br />pf_rank:   9","pf_expression_control: 6.75<br />pf_score: 7.872178<br />pf_rank:  46","pf_expression_control: 5.50<br />pf_score: 7.946768<br />pf_rank:  45","pf_expression_control: 4.25<br />pf_score: 6.548246<br />pf_rank:  99","pf_expression_control: 2.75<br />pf_score: 5.371202<br />pf_rank: 140","pf_expression_control: 4.00<br />pf_score: 6.859722<br />pf_rank:  87","pf_expression_control: 6.00<br />pf_score: 7.041796<br />pf_rank:  78","pf_expression_control: 5.75<br />pf_score: 7.184443<br />pf_rank:  74","pf_expression_control: 1.75<br />pf_score: 6.386483<br />pf_rank: 107","pf_expression_control: 5.00<br />pf_score: 8.583680<br />pf_rank:  32","pf_expression_control: 5.75<br />pf_score: 8.258946<br />pf_rank:  38","pf_expression_control: 8.75<br />pf_score: 9.083506<br />pf_rank:  13","pf_expression_control: 4.50<br />pf_score: 6.195871<br />pf_rank: 112","pf_expression_control: 5.50<br />pf_score: 6.381312<br />pf_rank: 108","pf_expression_control: 1.00<br />pf_score: 4.532449<br />pf_rank: 154","pf_expression_control: 2.00<br />pf_score: 3.116028<br />pf_rank: 160","pf_expression_control: 8.50<br />pf_score: 8.939129<br />pf_rank:  21","pf_expression_control: 6.50<br />pf_score: 7.544916<br />pf_rank:  60","pf_expression_control: 7.50<br />pf_score: 8.690446<br />pf_rank:  30","pf_expression_control: 7.75<br />pf_score: 7.268416<br />pf_rank:  70","pf_expression_control: 6.00<br />pf_score: 8.733771<br />pf_rank:  29","pf_expression_control: 3.50<br />pf_score: 6.238667<br />pf_rank: 111","pf_expression_control: 1.50<br />pf_score: 6.375676<br />pf_rank: 109","pf_expression_control: 4.00<br />pf_score: 6.445537<br />pf_rank: 102","pf_expression_control: 6.50<br />pf_score: 8.766350<br />pf_rank:  26","pf_expression_control: 4.25<br />pf_score: 5.630574<br />pf_rank: 133","pf_expression_control: 3.25<br />pf_score: 6.247013<br />pf_rank: 110","pf_expression_control: 1.50<br />pf_score: 5.863586<br />pf_rank: 127","pf_expression_control: 8.00<br />pf_score: 8.851068<br />pf_rank:  22","pf_expression_control: 4.75<br />pf_score: 6.429994<br />pf_rank: 104","pf_expression_control: 4.50<br />pf_score: 6.688444<br />pf_rank:  93","pf_expression_control: 4.50<br />pf_score: 6.403402<br />pf_rank: 105","pf_expression_control: 1.25<br />pf_score: 3.880566<br />pf_rank: 159","pf_expression_control: 8.50<br />pf_score: 8.821339<br />pf_rank:  24","pf_expression_control: 8.75<br />pf_score: 9.257402<br />pf_rank:   7","pf_expression_control: 3.75<br />pf_score: 7.413658<br />pf_rank:  67","pf_expression_control: 4.25<br />pf_score: 6.835124<br />pf_rank:  88","pf_expression_control: 6.25<br />pf_score: 7.445214<br />pf_rank:  65","pf_expression_control: 3.75<br />pf_score: 5.902980<br />pf_rank: 126","pf_expression_control: 5.25<br />pf_score: 6.060929<br />pf_rank: 119","pf_expression_control: 7.75<br />pf_score: 8.977997<br />pf_rank:  20","pf_expression_control: 5.00<br />pf_score: 4.987750<br />pf_rank: 151","pf_expression_control: 7.50<br />pf_score: 7.717466<br />pf_rank:  52","pf_expression_control: 2.25<br />pf_score: 6.796020<br />pf_rank:  89","pf_expression_control: 4.75<br />pf_score: 7.059030<br />pf_rank:  76","pf_expression_control: 7.00<br />pf_score: 7.998313<br />pf_rank:  43","pf_expression_control: 5.25<br />pf_score: 7.630104<br />pf_rank:  56","pf_expression_control: 3.75<br />pf_score: 5.986757<br />pf_rank: 124","pf_expression_control: 4.75<br />pf_score: 6.659207<br />pf_rank:  95","pf_expression_control: 2.75<br />pf_score: 5.463361<br />pf_rank: 139","pf_expression_control: 6.75<br />pf_score: 7.394916<br />pf_rank:  68","pf_expression_control: 4.75<br />pf_score: 6.911611<br />pf_rank:  85","pf_expression_control: 8.75<br />pf_score: 9.398842<br />pf_rank:   1","pf_expression_control: 8.00<br />pf_score: 9.284819<br />pf_rank:   6","pf_expression_control: 4.00<br />pf_score: 6.432495<br />pf_rank: 103","pf_expression_control: 5.25<br />pf_score: 5.714520<br />pf_rank: 129","pf_expression_control: 4.50<br />pf_score: 5.823617<br />pf_rank: 128","pf_expression_control: 9.25<br />pf_score: 9.342481<br />pf_rank:   2","pf_expression_control: 3.25<br />pf_score: 5.509055<br />pf_rank: 137","pf_expression_control: 2.50<br />pf_score: 5.324592<br />pf_rank: 143","pf_expression_control: 6.25<br />pf_score: 7.715240<br />pf_rank:  53","pf_expression_control: 6.75<br />pf_score: 7.254949<br />pf_rank:  71","pf_expression_control: 4.00<br />pf_score: 6.974598<br />pf_rank:  80","pf_expression_control: 5.25<br />pf_score: 7.720787<br />pf_rank:  51","pf_expression_control: 5.00<br />pf_score: 6.497527<br />pf_rank: 100","pf_expression_control: 6.50<br />pf_score: 8.353120<br />pf_rank:  36","pf_expression_control: 8.25<br />pf_score: 9.043712<br />pf_rank:  14","pf_expression_control: 3.25<br />pf_score: 5.525553<br />pf_rank: 134","pf_expression_control: 6.00<br />pf_score: 8.653232<br />pf_rank:  31","pf_expression_control: 1.50<br />pf_score: 5.714382<br />pf_rank: 130","pf_expression_control: 1.50<br />pf_score: 6.467739<br />pf_rank: 101","pf_expression_control: 2.00<br />pf_score: 4.438732<br />pf_rank: 155","pf_expression_control: 6.25<br />pf_score: 6.774188<br />pf_rank:  91","pf_expression_control: 4.75<br />pf_score: 7.847943<br />pf_rank:  48","pf_expression_control: 6.00<br />pf_score: 7.372524<br />pf_rank:  69","pf_expression_control: 5.00<br />pf_score: 7.044659<br />pf_rank:  77","pf_expression_control: 4.50<br />pf_score: 7.479424<br />pf_rank:  62","pf_expression_control: 7.75<br />pf_score: 8.536500<br />pf_rank:  33","pf_expression_control: 8.00<br />pf_score: 8.822791<br />pf_rank:  23","pf_expression_control: 5.00<br />pf_score: 7.695144<br />pf_rank:  55","pf_expression_control: 6.50<br />pf_score: 8.758214<br />pf_rank:  27","pf_expression_control: 3.75<br />pf_score: 6.040722<br />pf_rank: 121","pf_expression_control: 1.00<br />pf_score: 4.246047<br />pf_rank: 157","pf_expression_control: 6.75<br />pf_score: 7.788563<br />pf_rank:  50","pf_expression_control: 2.50<br />pf_score: 6.020329<br />pf_rank: 122","pf_expression_control: 8.75<br />pf_score: 9.334750<br />pf_rank:   3","pf_expression_control: 9.00<br />pf_score: 9.185518<br />pf_rank:  10","pf_expression_control: 0.50<br />pf_score: 2.511654<br />pf_rank: 161","pf_expression_control: 8.00<br />pf_score: 9.041394<br />pf_rank:  15","pf_expression_control: 1.75<br />pf_score: 5.652325<br />pf_rank: 132","pf_expression_control: 4.50<br />pf_score: 6.126419<br />pf_rank: 116","pf_expression_control: 1.75<br />pf_score: 6.399563<br />pf_rank: 106","pf_expression_control: 7.25<br />pf_score: 6.186450<br />pf_rank: 113","pf_expression_control: 5.25<br />pf_score: 6.729938<br />pf_rank:  92","pf_expression_control: 7.25<br />pf_score: 6.920446<br />pf_rank:  82","pf_expression_control: 5.50<br />pf_score: 6.577472<br />pf_rank:  98","pf_expression_control: 1.75<br />pf_score: 6.092465<br />pf_rank: 118","pf_expression_control: 3.75<br />pf_score: 6.128889<br />pf_rank: 115","pf_expression_control: 3.75<br />pf_score: 6.586270<br />pf_rank:  97","pf_expression_control: 2.50<br />pf_score: 5.073350<br />pf_rank: 149","pf_expression_control: 7.75<br />pf_score: 8.995836<br />pf_rank:  18","pf_expression_control: 7.00<br />pf_score: 8.747310<br />pf_rank:  28","pf_expression_control: 7.50<br />pf_score: 8.296142<br />pf_rank:  37","pf_expression_control: 2.25<br />pf_score: 5.521449<br />pf_rank: 135","pf_expression_control: 1.75<br />pf_score: 5.968008<br />pf_rank: 125","pf_expression_control: 1.00<br />pf_score: 2.166555<br />pf_rank: 162","pf_expression_control: 4.00<br />pf_score: 6.007699<br />pf_rank: 123","pf_expression_control: 3.75<br />pf_score: 5.170726<br />pf_rank: 148"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":["rgba(41,86,125,1)","rgba(79,163,229,1)","rgba(66,137,193,1)","rgba(35,74,109,1)","rgba(52,108,155,1)","rgba(23,50,77,1)","rgba(22,48,74,1)","rgba(72,149,209,1)","rgba(44,92,133,1)","rgba(65,134,189,1)","rgba(79,162,227,1)","rgba(39,84,122,1)","rgba(67,139,196,1)","rgba(26,56,85,1)","rgba(44,93,135,1)","rgba(42,89,129,1)","rgba(57,118,168,1)","rgba(47,99,142,1)","rgba(37,78,114,1)","rgba(53,110,157,1)","rgba(56,117,166,1)","rgba(82,169,236,1)","rgba(34,73,108,1)","rgba(43,91,131,1)","rgba(83,172,240,1)","rgba(47,98,141,1)","rgba(77,159,223,1)","rgba(23,51,78,1)","rgba(35,76,111,1)","rgba(75,155,218,1)","rgba(74,153,215,1)","rgba(34,72,106,1)","rgba(77,158,221,1)","rgba(50,104,149,1)","rgba(81,168,235,1)","rgba(54,113,162,1)","rgba(34,72,107,1)","rgba(48,101,145,1)","rgba(32,69,101,1)","rgba(32,68,100,1)","rgba(25,54,82,1)","rgba(20,45,70,1)","rgba(51,106,151,1)","rgba(42,88,127,1)","rgba(84,173,242,1)","rgba(51,107,154,1)","rgba(25,55,83,1)","rgba(81,166,232,1)","rgba(37,80,116,1)","rgba(20,46,71,1)","rgba(28,61,91,1)","rgba(78,161,225,1)","rgba(78,162,226,1)","rgba(41,87,126,1)","rgba(22,49,75,1)","rgba(36,77,113,1)","rgba(36,76,112,1)","rgba(58,121,172,1)","rgba(76,157,220,1)","rgba(53,111,158,1)","rgba(49,103,148,1)","rgba(48,100,144,1)","rgba(62,128,181,1)","rgba(31,66,98,1)","rgba(33,71,105,1)","rgba(23,52,79,1)","rgba(64,132,187,1)","rgba(62,129,182,1)","rgba(82,170,237,1)","rgba(85,175,245,1)","rgba(27,58,87,1)","rgba(42,88,128,1)","rgba(30,65,96,1)","rgba(46,97,139,1)","rgba(30,64,95,1)","rgba(63,131,186,1)","rgba(62,130,183,1)","rgba(59,124,175,1)","rgba(28,62,92,1)","rgba(73,151,212,1)","rgba(63,130,185,1)","rgba(70,145,205,1)","rgba(27,59,88,1)","rgba(60,125,178,1)","rgba(56,116,165,1)","rgba(61,126,179,1)","rgba(85,174,243,1)","rgba(28,60,90,1)","rgba(21,47,73,1)","rgba(45,94,136,1)","rgba(54,112,159,1)","rgba(44,92,134,1)","rgba(70,145,203,1)","rgba(67,138,195,1)","rgba(26,57,86,1)","rgba(81,167,234,1)","rgba(39,82,119,1)","rgba(54,112,160,1)","rgba(48,102,146,1)","rgba(35,75,110,1)","rgba(40,85,124,1)","rgba(69,143,201,1)","rgba(56,118,167,1)","rgba(76,156,219,1)","rgba(45,95,137,1)","rgba(52,109,156,1)","rgba(19,43,67,1)","rgba(21,47,72,1)","rgba(60,124,176,1)","rgba(71,147,207,1)","rgba(71,146,206,1)","rgba(19,44,68,1)","rgba(75,154,217,1)","rgba(77,160,224,1)","rgba(39,83,121,1)","rgba(46,97,140,1)","rgba(50,105,150,1)","rgba(38,81,118,1)","rgba(59,122,173,1)","rgba(32,69,102,1)","rgba(24,53,80,1)","rgba(73,152,213,1)","rgba(30,65,97,1)","rgba(72,148,208,1)","rgba(59,123,174,1)","rgba(83,171,238,1)","rgba(55,114,163,1)","rgba(37,79,115,1)","rgba(46,96,138,1)","rgba(49,102,147,1)","rgba(43,90,130,1)","rgba(31,67,99,1)","rgba(27,59,89,1)","rgba(40,84,123,1)","rgba(29,62,93,1)","rgba(68,140,198,1)","rgba(84,172,241,1)","rgba(38,80,117,1)","rgba(68,141,199,1)","rgba(20,44,69,1)","rgba(22,50,76,1)","rgba(86,176,246,1)","rgba(24,53,81,1)","rgba(73,150,211,1)","rgba(66,136,192,1)","rgba(61,127,180,1)","rgba(64,133,188,1)","rgba(55,115,164,1)","rgba(51,107,153,1)","rgba(58,120,171,1)","rgba(66,137,194,1)","rgba(65,135,190,1)","rgba(57,119,170,1)","rgba(80,165,231,1)","rgba(25,56,84,1)","rgba(29,63,94,1)","rgba(33,70,103,1)","rgba(74,153,214,1)","rgba(69,144,202,1)","rgba(86,177,247,1)","rgba(69,142,200,1)","rgba(80,164,230,1)"],"opacity":1,"size":7.559055118110237,"symbol":"circle","line":{"width":1.8897637795275593,"color":["rgba(41,86,125,1)","rgba(79,163,229,1)","rgba(66,137,193,1)","rgba(35,74,109,1)","rgba(52,108,155,1)","rgba(23,50,77,1)","rgba(22,48,74,1)","rgba(72,149,209,1)","rgba(44,92,133,1)","rgba(65,134,189,1)","rgba(79,162,227,1)","rgba(39,84,122,1)","rgba(67,139,196,1)","rgba(26,56,85,1)","rgba(44,93,135,1)","rgba(42,89,129,1)","rgba(57,118,168,1)","rgba(47,99,142,1)","rgba(37,78,114,1)","rgba(53,110,157,1)","rgba(56,117,166,1)","rgba(82,169,236,1)","rgba(34,73,108,1)","rgba(43,91,131,1)","rgba(83,172,240,1)","rgba(47,98,141,1)","rgba(77,159,223,1)","rgba(23,51,78,1)","rgba(35,76,111,1)","rgba(75,155,218,1)","rgba(74,153,215,1)","rgba(34,72,106,1)","rgba(77,158,221,1)","rgba(50,104,149,1)","rgba(81,168,235,1)","rgba(54,113,162,1)","rgba(34,72,107,1)","rgba(48,101,145,1)","rgba(32,69,101,1)","rgba(32,68,100,1)","rgba(25,54,82,1)","rgba(20,45,70,1)","rgba(51,106,151,1)","rgba(42,88,127,1)","rgba(84,173,242,1)","rgba(51,107,154,1)","rgba(25,55,83,1)","rgba(81,166,232,1)","rgba(37,80,116,1)","rgba(20,46,71,1)","rgba(28,61,91,1)","rgba(78,161,225,1)","rgba(78,162,226,1)","rgba(41,87,126,1)","rgba(22,49,75,1)","rgba(36,77,113,1)","rgba(36,76,112,1)","rgba(58,121,172,1)","rgba(76,157,220,1)","rgba(53,111,158,1)","rgba(49,103,148,1)","rgba(48,100,144,1)","rgba(62,128,181,1)","rgba(31,66,98,1)","rgba(33,71,105,1)","rgba(23,52,79,1)","rgba(64,132,187,1)","rgba(62,129,182,1)","rgba(82,170,237,1)","rgba(85,175,245,1)","rgba(27,58,87,1)","rgba(42,88,128,1)","rgba(30,65,96,1)","rgba(46,97,139,1)","rgba(30,64,95,1)","rgba(63,131,186,1)","rgba(62,130,183,1)","rgba(59,124,175,1)","rgba(28,62,92,1)","rgba(73,151,212,1)","rgba(63,130,185,1)","rgba(70,145,205,1)","rgba(27,59,88,1)","rgba(60,125,178,1)","rgba(56,116,165,1)","rgba(61,126,179,1)","rgba(85,174,243,1)","rgba(28,60,90,1)","rgba(21,47,73,1)","rgba(45,94,136,1)","rgba(54,112,159,1)","rgba(44,92,134,1)","rgba(70,145,203,1)","rgba(67,138,195,1)","rgba(26,57,86,1)","rgba(81,167,234,1)","rgba(39,82,119,1)","rgba(54,112,160,1)","rgba(48,102,146,1)","rgba(35,75,110,1)","rgba(40,85,124,1)","rgba(69,143,201,1)","rgba(56,118,167,1)","rgba(76,156,219,1)","rgba(45,95,137,1)","rgba(52,109,156,1)","rgba(19,43,67,1)","rgba(21,47,72,1)","rgba(60,124,176,1)","rgba(71,147,207,1)","rgba(71,146,206,1)","rgba(19,44,68,1)","rgba(75,154,217,1)","rgba(77,160,224,1)","rgba(39,83,121,1)","rgba(46,97,140,1)","rgba(50,105,150,1)","rgba(38,81,118,1)","rgba(59,122,173,1)","rgba(32,69,102,1)","rgba(24,53,80,1)","rgba(73,152,213,1)","rgba(30,65,97,1)","rgba(72,148,208,1)","rgba(59,123,174,1)","rgba(83,171,238,1)","rgba(55,114,163,1)","rgba(37,79,115,1)","rgba(46,96,138,1)","rgba(49,102,147,1)","rgba(43,90,130,1)","rgba(31,67,99,1)","rgba(27,59,89,1)","rgba(40,84,123,1)","rgba(29,62,93,1)","rgba(68,140,198,1)","rgba(84,172,241,1)","rgba(38,80,117,1)","rgba(68,141,199,1)","rgba(20,44,69,1)","rgba(22,50,76,1)","rgba(86,176,246,1)","rgba(24,53,81,1)","rgba(73,150,211,1)","rgba(66,136,192,1)","rgba(61,127,180,1)","rgba(64,133,188,1)","rgba(55,115,164,1)","rgba(51,107,153,1)","rgba(58,120,171,1)","rgba(66,137,194,1)","rgba(65,135,190,1)","rgba(57,119,170,1)","rgba(80,165,231,1)","rgba(25,56,84,1)","rgba(29,63,94,1)","rgba(33,70,103,1)","rgba(74,153,214,1)","rgba(69,144,202,1)","rgba(86,177,247,1)","rgba(69,142,200,1)","rgba(80,164,230,1)"]}},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[0],"y":[2],"name":"99_4583520b6f4c72ec455f359b3b5540df","type":"scatter","mode":"markers","opacity":0,"hoverinfo":"skip","showlegend":false,"marker":{"color":[0,1],"colorscale":[[0,"#132B43"],[0.0033444816053511696,"#132B44"],[0.0066889632107023393,"#132C44"],[0.010033444816053512,"#142C45"],[0.013377926421404682,"#142D45"],[0.016722408026755852,"#142D46"],[0.020066889632107024,"#142D46"],[0.023411371237458192,"#142E47"],[0.026755852842809364,"#152E47"],[0.030100334448160532,"#152F48"],[0.033444816053511704,"#152F48"],[0.036789297658862873,"#152F49"],[0.040133779264214048,"#153049"],[0.043478260869565216,"#16304A"],[0.046822742474916385,"#16304A"],[0.05016722408026756,"#16314B"],[0.053511705685618728,"#16314B"],[0.056856187290969896,"#16324C"],[0.060200668896321065,"#17324D"],[0.06354515050167224,"#17324D"],[0.066889632107023408,"#17334E"],[0.070234113712374577,"#17334E"],[0.073578595317725745,"#17344F"],[0.076923076923076913,"#18344F"],[0.080267558528428096,"#183450"],[0.083612040133779264,"#183550"],[0.086956521739130432,"#183551"],[0.090301003344481601,"#183651"],[0.093645484949832769,"#193652"],[0.096989966555183937,"#193652"],[0.10033444816053512,"#193753"],[0.10367892976588627,"#193754"],[0.10702341137123746,"#193854"],[0.11036789297658864,"#1A3855"],[0.11371237458193979,"#1A3955"],[0.11705685618729098,"#1A3956"],[0.12040133779264213,"#1A3956"],[0.12374581939799331,"#1A3A57"],[0.12709030100334448,"#1B3A57"],[0.13043478260869565,"#1B3B58"],[0.13377926421404682,"#1B3B59"],[0.13712374581939799,"#1B3B59"],[0.14046822742474915,"#1C3C5A"],[0.14381270903010032,"#1C3C5A"],[0.14715719063545149,"#1C3D5B"],[0.15050167224080266,"#1C3D5B"],[0.15384615384615383,"#1C3D5C"],[0.15719063545150502,"#1D3E5C"],[0.16053511705685619,"#1D3E5D"],[0.16387959866220736,"#1D3F5D"],[0.16722408026755853,"#1D3F5E"],[0.1705685618729097,"#1D3F5F"],[0.17391304347826086,"#1E405F"],[0.17725752508361203,"#1E4060"],[0.1806020066889632,"#1E4160"],[0.18394648829431437,"#1E4161"],[0.18729096989966554,"#1E4261"],[0.19063545150501671,"#1F4262"],[0.19397993311036787,"#1F4263"],[0.19732441471571904,"#1F4363"],[0.20066889632107024,"#1F4364"],[0.20401337792642141,"#1F4464"],[0.20735785953177255,"#204465"],[0.21070234113712372,"#204465"],[0.21404682274247491,"#204566"],[0.21739130434782608,"#204566"],[0.22073578595317728,"#214667"],[0.22408026755852842,"#214668"],[0.22742474916387959,"#214768"],[0.23076923076923075,"#214769"],[0.23411371237458195,"#214769"],[0.23745819397993309,"#22486A"],[0.24080267558528426,"#22486A"],[0.24414715719063546,"#22496B"],[0.24749163879598662,"#22496C"],[0.25083612040133774,"#224A6C"],[0.25418060200668896,"#234A6D"],[0.25752508361204013,"#234A6D"],[0.2608695652173913,"#234B6E"],[0.26421404682274247,"#234B6E"],[0.26755852842809363,"#244C6F"],[0.2709030100334448,"#244C70"],[0.27424749163879597,"#244C70"],[0.27759197324414714,"#244D71"],[0.28093645484949831,"#244D71"],[0.28428093645484948,"#254E72"],[0.28762541806020064,"#254E72"],[0.29096989966555187,"#254F73"],[0.29431438127090298,"#254F74"],[0.29765886287625415,"#254F74"],[0.30100334448160532,"#265075"],[0.30434782608695654,"#265075"],[0.30769230769230765,"#265176"],[0.31103678929765882,"#265176"],[0.31438127090301005,"#275277"],[0.31772575250836121,"#275278"],[0.32107023411371238,"#275278"],[0.3244147157190635,"#275379"],[0.32775919732441472,"#275379"],[0.33110367892976589,"#28547A"],[0.33444816053511706,"#28547B"],[0.33779264214046822,"#28557B"],[0.34113712374581939,"#28557C"],[0.34448160535117056,"#28567C"],[0.34782608695652173,"#29567D"],[0.3511705685618729,"#29567D"],[0.35451505016722407,"#29577E"],[0.35785953177257523,"#29577F"],[0.3612040133779264,"#2A587F"],[0.36454849498327757,"#2A5880"],[0.36789297658862874,"#2A5980"],[0.37123745819397991,"#2A5981"],[0.37458193979933108,"#2A5982"],[0.3779264214046823,"#2B5A82"],[0.38127090301003341,"#2B5A83"],[0.38461538461538458,"#2B5B83"],[0.38795986622073575,"#2B5B84"],[0.39130434782608697,"#2C5C85"],[0.39464882943143809,"#2C5C85"],[0.39799331103678931,"#2C5D86"],[0.40133779264214048,"#2C5D86"],[0.40468227424749159,"#2C5D87"],[0.40802675585284282,"#2D5E87"],[0.41137123745819393,"#2D5E88"],[0.4147157190635451,"#2D5F89"],[0.41806020066889632,"#2D5F89"],[0.42140468227424743,"#2E608A"],[0.42474916387959866,"#2E608A"],[0.42809364548494983,"#2E618B"],[0.43143812709030094,"#2E618C"],[0.43478260869565216,"#2E618C"],[0.43812709030100333,"#2F628D"],[0.44147157190635455,"#2F628D"],[0.44481605351170567,"#2F638E"],[0.44816053511705684,"#2F638F"],[0.45150501672240806,"#30648F"],[0.45484949832775917,"#306490"],[0.45819397993311028,"#306590"],[0.46153846153846151,"#306591"],[0.46488294314381268,"#306592"],[0.4682274247491639,"#316692"],[0.47157190635451501,"#316693"],[0.47491638795986618,"#316793"],[0.47826086956521741,"#316794"],[0.48160535117056852,"#326895"],[0.48494983277591974,"#326895"],[0.48829431438127091,"#326996"],[0.49163879598662202,"#326996"],[0.49498327759197325,"#326997"],[0.49832775919732436,"#336A98"],[0.50167224080267547,"#336A98"],[0.50501672240802675,"#336B99"],[0.50836120401337792,"#336B99"],[0.51170568561872909,"#346C9A"],[0.51505016722408026,"#346C9B"],[0.51839464882943143,"#346D9B"],[0.52173913043478259,"#346D9C"],[0.52508361204013376,"#346E9D"],[0.52842809364548493,"#356E9D"],[0.5317725752508361,"#356E9E"],[0.53511705685618727,"#356F9E"],[0.53846153846153844,"#356F9F"],[0.5418060200668896,"#3670A0"],[0.54515050167224077,"#3670A0"],[0.54849498327759194,"#3671A1"],[0.55183946488294311,"#3671A1"],[0.55518394648829428,"#3772A2"],[0.55852842809364545,"#3772A3"],[0.56187290969899661,"#3773A3"],[0.56521739130434778,"#3773A4"],[0.56856187290969895,"#3773A4"],[0.57190635451505012,"#3874A5"],[0.57525083612040129,"#3874A6"],[0.57859531772575246,"#3875A6"],[0.58193979933110374,"#3875A7"],[0.58528428093645479,"#3976A8"],[0.58862876254180596,"#3976A8"],[0.59197324414715713,"#3977A9"],[0.5953177257525083,"#3977A9"],[0.59866220735785958,"#3978AA"],[0.60200668896321063,"#3A78AB"],[0.6053511705685618,"#3A79AB"],[0.60869565217391308,"#3A79AC"],[0.61204013377926414,"#3A79AC"],[0.61538461538461531,"#3B7AAD"],[0.61872909698996659,"#3B7AAE"],[0.62207357859531764,"#3B7BAE"],[0.62541806020066892,"#3B7BAF"],[0.62876254180602009,"#3C7CB0"],[0.63210702341137115,"#3C7CB0"],[0.63545150501672243,"#3C7DB1"],[0.63879598662207349,"#3C7DB1"],[0.64214046822742477,"#3C7EB2"],[0.64548494983277593,"#3D7EB3"],[0.64882943143812699,"#3D7FB3"],[0.65217391304347827,"#3D7FB4"],[0.65551839464882944,"#3D7FB5"],[0.6588628762541805,"#3E80B5"],[0.66220735785953178,"#3E80B6"],[0.66555183946488294,"#3E81B6"],[0.66889632107023411,"#3E81B7"],[0.67224080267558528,"#3F82B8"],[0.67558528428093645,"#3F82B8"],[0.67892976588628762,"#3F83B9"],[0.68227424749163879,"#3F83BA"],[0.68561872909698984,"#4084BA"],[0.68896321070234112,"#4084BB"],[0.69230769230769229,"#4085BB"],[0.69565217391304346,"#4085BC"],[0.69899665551839463,"#4086BD"],[0.7023411371237458,"#4186BD"],[0.70568561872909696,"#4186BE"],[0.70903010033444813,"#4187BF"],[0.7123745819397993,"#4187BF"],[0.71571906354515047,"#4288C0"],[0.71906354515050164,"#4288C1"],[0.72240802675585281,"#4289C1"],[0.72575250836120397,"#4289C2"],[0.72909698996655514,"#438AC2"],[0.73244147157190631,"#438AC3"],[0.73578595317725748,"#438BC4"],[0.73913043478260865,"#438BC4"],[0.74247491638795982,"#438CC5"],[0.74581939799331098,"#448CC6"],[0.74916387959866215,"#448DC6"],[0.75250836120401332,"#448DC7"],[0.7558528428093646,"#448EC8"],[0.75919732441471566,"#458EC8"],[0.76254180602006683,"#458FC9"],[0.76588628762541811,"#458FC9"],[0.76923076923076916,"#458FCA"],[0.77257525083612033,"#4690CB"],[0.7759197324414715,"#4690CB"],[0.77926421404682267,"#4691CC"],[0.78260869565217395,"#4691CD"],[0.785953177257525,"#4792CD"],[0.78929765886287617,"#4792CE"],[0.79264214046822745,"#4793CF"],[0.79598662207357862,"#4793CF"],[0.79933110367892968,"#4894D0"],[0.80267558528428096,"#4894D0"],[0.80602006688963213,"#4895D1"],[0.80936454849498318,"#4895D2"],[0.81270903010033446,"#4896D2"],[0.81605351170568563,"#4996D3"],[0.81939799331103669,"#4997D4"],[0.82274247491638786,"#4997D4"],[0.82608695652173914,"#4998D5"],[0.82943143812709019,"#4A98D6"],[0.83277591973244136,"#4A99D6"],[0.83612040133779264,"#4A99D7"],[0.83946488294314381,"#4A9AD8"],[0.84280936454849487,"#4B9AD8"],[0.84615384615384615,"#4B9BD9"],[0.84949832775919731,"#4B9BDA"],[0.85284280936454837,"#4B9BDA"],[0.85618729096989965,"#4C9CDB"],[0.85953177257525082,"#4C9CDB"],[0.86287625418060188,"#4C9DDC"],[0.86622073578595316,"#4C9DDD"],[0.86956521739130432,"#4D9EDD"],[0.87290969899665538,"#4D9EDE"],[0.87625418060200666,"#4D9FDF"],[0.87959866220735783,"#4D9FDF"],[0.88294314381270911,"#4DA0E0"],[0.88628762541806017,"#4EA0E1"],[0.88963210702341133,"#4EA1E1"],[0.89297658862876261,"#4EA1E2"],[0.89632107023411367,"#4EA2E3"],[0.89966555183946484,"#4FA2E3"],[0.90301003344481612,"#4FA3E4"],[0.90635451505016706,"#4FA3E5"],[0.90969899665551834,"#4FA4E5"],[0.91304347826086951,"#50A4E6"],[0.91638795986622057,"#50A5E7"],[0.91973244147157185,"#50A5E7"],[0.92307692307692302,"#50A6E8"],[0.9264214046822743,"#51A6E8"],[0.92976588628762535,"#51A7E9"],[0.93311036789297652,"#51A7EA"],[0.9364548494983278,"#51A8EA"],[0.93979933110367886,"#52A8EB"],[0.94314381270903003,"#52A9EC"],[0.94648829431438131,"#52A9EC"],[0.94983277591973236,"#52AAED"],[0.95317725752508353,"#53AAEE"],[0.95652173913043481,"#53ABEE"],[0.95986622073578587,"#53ABEF"],[0.96321070234113704,"#53ACF0"],[0.96655518394648832,"#54ACF0"],[0.96989966555183948,"#54ADF1"],[0.97324414715719054,"#54ADF2"],[0.97658862876254182,"#54AEF2"],[0.97993311036789299,"#55AEF3"],[0.98327759197324405,"#55AFF4"],[0.98662207357859533,"#55AFF4"],[0.98996655518394649,"#55B0F5"],[0.99331103678929755,"#56B0F6"],[0.99665551839464872,"#56B1F6"],[1,"#56B1F7"]],"colorbar":{"bgcolor":"rgba(255,255,255,1)","bordercolor":"transparent","borderwidth":1.8897637795275593,"thickness":23.039999999999996,"title":"Horsepower","titlefont":{"color":"rgba(0,0,0,1)","family":"","size":14.611872146118724},"tickmode":"array","ticktext":["40","80","120","160"],"tickvals":[0.24223602484472051,0.49068322981366458,0.73913043478260865,0.98757763975155277],"tickfont":{"color":"rgba(0,0,0,1)","family":"","size":11.68949771689498},"ticklen":2,"len":0.5}},"xaxis":"x","yaxis":"y","frame":null}],"layout":{"margin":{"t":43.762557077625573,"r":7.3059360730593621,"b":40.182648401826498,"l":31.415525114155255},"plot_bgcolor":"rgba(235,235,235,1)","paper_bgcolor":"rgba(255,255,255,1)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.611872146118724},"title":{"text":"3D Scatter Plot","font":{"color":"rgba(0,0,0,1)","family":"","size":17.534246575342465},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-0.20000000000000001,9.6999999999999993],"tickmode":"array","ticktext":["0.0","2.5","5.0","7.5"],"tickvals":[0,2.5,5,7.5],"categoryorder":"array","categoryarray":["0.0","2.5","5.0","7.5"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.6529680365296811,"tickwidth":0.66417600664176002,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.68949771689498},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176002,"zeroline":false,"anchor":"y","title":{"text":"Weight","font":{"color":"rgba(0,0,0,1)","family":"","size":14.611872146118724}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[1.8049409889999999,9.7604567109999998],"tickmode":"array","ticktext":["2","4","6","8"],"tickvals":[2,4,6,8],"categoryorder":"array","categoryarray":["2","4","6","8"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.6529680365296811,"tickwidth":0.66417600664176002,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.68949771689498},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176002,"zeroline":false,"anchor":"x","title":{"text":"Miles per Gallon","font":{"color":"rgba(0,0,0,1)","family":"","size":14.611872146118724}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":"rgba(255,255,255,1)","bordercolor":"transparent","borderwidth":1.8897637795275593,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.68949771689498},"title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.611872146118724}}},"hovermode":"closest","barmode":"relative","scene":{"xaxis":{"title":"Weight"},"yaxis":{"title":"Miles per Gallon"},"zaxis":{"title":"Horsepower"}}},"config":{"doubleClick":"reset","modeBarButtonsToAdd":["hoverclosest","hovercompare"],"showSendToCloud":false},"source":"A","attrs":{"a11c5e659ef6":{"x":{},"y":{},"colour":{},"type":"scatter"}},"cur_data":"a11c5e659ef6","visdat":{"a11c5e659ef6":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.20000000000000001,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>

``` r
# Create a 3D scatter plot
fig <- plot_ly(hfi_2016, x = ~pf_expression_control, y = ~pf_score, z = ~pf_rank, type = 'scatter3d', mode = 'markers')

fig
```

<div class="plotly html-widget html-fill-item-overflow-hidden html-fill-item" id="htmlwidget-ee13aee86d750556fced" style="width:672px;height:480px;"></div>
<script type="application/json" data-for="htmlwidget-ee13aee86d750556fced">{"x":{"visdat":{"a11c61a758e4":["function () ","plotlyVisDat"]},"cur_data":"a11c61a758e4","attrs":{"a11c61a758e4":{"x":{},"y":{},"z":{},"mode":"markers","alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"scatter3d"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"scene":{"xaxis":{"title":"pf_expression_control"},"yaxis":{"title":"pf_score"},"zaxis":{"title":"pf_rank"}},"hovermode":"closest","showlegend":false},"source":"A","config":{"modeBarButtonsToAdd":["hoverclosest","hovercompare"],"showSendToCloud":false},"data":[{"x":[5.25,4,2.5,5.5,4.25,7.75,8,0.25,7.25,0.75,3.25,7.5,2.5,9.25,6.75,7.25,5,4.25,4.25,5.25,4.5,3.75,5.75,7,0.75,3.25,4.25,8.25,7.75,3,2.5,6.75,1.5,3.5,3.75,2,8.5,5.25,5.75,7.5,8,8.5,4.75,3.25,1.5,5.5,9,0.5,6,9,6.75,3.75,1.25,4.75,7.5,6.75,5.5,4.25,2.75,4,6,5.75,1.75,5,5.75,8.75,4.5,5.5,1,2,8.5,6.5,7.5,7.75,6,3.5,1.5,4,6.5,4.25,3.25,1.5,8,4.75,4.5,4.5,1.25,8.5,8.75,3.75,4.25,6.25,3.75,5.25,7.75,5,7.5,2.25,4.75,7,5.25,3.75,4.75,2.75,6.75,4.75,8.75,8,4,5.25,4.5,9.25,3.25,2.5,6.25,6.75,4,5.25,5,6.5,8.25,3.25,6,1.5,1.5,2,6.25,4.75,6,5,4.5,7.75,8,5,6.5,3.75,1,6.75,2.5,8.75,9,0.5,8,1.75,4.5,1.75,7.25,5.25,7.25,5.5,1.75,3.75,3.75,2.5,7.75,7,7.5,2.25,1.75,1,4,3.75],"y":[7.5962805759999998,5.2817721090000003,6.1113243639999997,8.0996956820000001,6.9128036020000003,9.1844384530000003,9.2469484009999992,5.6765534779999998,7.454537846,6.1360699939999996,5.3026003580000003,7.7068944650000004,6.0590281629999998,8.9871785870000007,7.4308638360000003,7.4969762949999996,6.6009728150000004,7.206770015,7.8614471449999996,6.8763344530000001,6.6659774059999997,4.6636997180000002,8.1555390770000002,7.4553396699999999,4.4141344269999996,7.2384482859999997,5.3307743109999999,9.1517266419999999,7.9865830659999997,5.465632394,5.50954107,8.2160353409999995,5.3508203390000002,7.0215132730000001,4.9472567859999996,6.7778676940000002,8.1654289690000006,7.0625420480000001,8.456175752,8.5143939159999995,9.0297614139999993,9.3256399689999991,6.9425749620000001,7.5505553589999996,3.8945537730000002,6.917901981,9.0137012179999996,5.0640899409999998,7.7922043949999997,9.2943679170000006,8.7668029270000005,5.3155752219999997,5.3026847019999996,7.576535507,9.2351913779999997,7.8721778149999997,7.9467681499999996,6.5482459820000001,5.3712016069999997,6.8597217239999999,7.0417958729999999,7.1844425879999996,6.3864825969999997,8.5836795909999992,8.2589457730000007,9.083506216,6.1958713349999996,6.3813124600000002,4.5324492449999996,3.1160276200000001,8.9391294039999991,7.5449161419999999,8.6904463169999993,7.2684160929999999,8.7337705499999991,6.2386665170000004,6.375675545,6.44553669,8.7663498579999999,5.6305744740000003,6.2470129539999997,5.8635861709999997,8.8510680839999996,6.4299941790000004,6.6884439599999999,6.403401744,3.8805656229999999,8.8213389259999992,9.2574018220000003,7.4136580240000001,6.8351235089999998,7.4452136089999996,5.9029799540000001,6.0609294709999997,8.9779974619999994,4.9877497760000002,7.7174656319999997,6.7960203970000004,7.0590295249999997,7.9983125529999999,7.6301041329999997,5.9867565120000004,6.6592066450000003,5.4633608689999997,7.3949155019999999,6.91161139,9.3988423599999997,9.2848191569999994,6.4324947259999998,5.7145200970000003,5.8236172059999998,9.3424811329999997,5.5090552129999999,5.3245924479999998,7.7152400739999996,7.2549488159999997,6.9745981869999998,7.7207866950000001,6.4975266940000003,8.3531204549999991,9.0437122429999999,5.5255529770000003,8.6532324989999996,5.7143816650000003,6.4677390509999997,4.4387320690000003,6.7741877690000001,7.8479432520000003,7.3725243349999996,7.0446591520000004,7.4794236349999998,8.5364998910000001,8.8227914670000001,7.695143614,8.7582137640000006,6.0407218330000001,4.2460471709999998,7.7885630460000002,6.0203286570000003,9.3347502979999994,9.1855180989999994,2.5116537540000001,9.0413940369999999,5.6523245790000001,6.1264186929999997,6.3995626010000004,6.186449627,6.7299380099999997,6.9204463880000002,6.577472244,6.0924653089999996,6.1288890939999998,6.5862704079999999,5.0733501759999999,8.9958358720000007,8.7473102160000007,8.2961418059999996,5.5214489130000004,5.9680076,2.1665553399999999,6.0076988560000002,5.1707255749999996],"z":[57,147,117,42,84,11,8,131,64,114,146,54,120,19,66,61,96,73,47,86,94,153,41,63,156,72,142,12,44,138,136,39,141,79,152,90,40,75,35,34,16,4,81,59,158,83,17,150,49,5,25,144,145,58,9,46,45,99,140,87,78,74,107,32,38,13,112,108,154,160,21,60,30,70,29,111,109,102,26,133,110,127,22,104,93,105,159,24,7,67,88,65,126,119,20,151,52,89,76,43,56,124,95,139,68,85,1,6,103,129,128,2,137,143,53,71,80,51,100,36,14,134,31,130,101,155,91,48,69,77,62,33,23,55,27,121,157,50,122,3,10,161,15,132,116,106,113,92,82,98,118,115,97,149,18,28,37,135,125,162,123,148],"mode":"markers","type":"scatter3d","marker":{"color":"rgba(31,119,180,1)","line":{"color":"rgba(31,119,180,1)"}},"error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"line":{"color":"rgba(31,119,180,1)"},"frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.20000000000000001,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>

``` r
library(plotly)

two_d <- ggplot(hfi_2016, aes(x = pf_expression_control, y = pf_score, color = pf_rank)) +
  geom_point(size = 2) +
  labs(title = "3D Scatter Plot",
       x = "Weight",
       y = "Miles per Gallon",
       color = "Horsepower")
# Convert ggplot to plotly object for 3D plotting
ggplotly(two_d) %>%
  layout(
    scene = list(
      xaxis = list(title = 'Weight'),
      yaxis = list(title = 'Miles per Gallon'),
      zaxis = list(title = 'Horsepower')
    )
  )
```

<div class="plotly html-widget html-fill-item-overflow-hidden html-fill-item" id="htmlwidget-fdde9b6b305313dc83d4" style="width:672px;height:480px;"></div>
<script type="application/json" data-for="htmlwidget-fdde9b6b305313dc83d4">{"x":{"data":[{"x":[5.25,4,2.5,5.5,4.25,7.75,8,0.25,7.25,0.75,3.25,7.5,2.5,9.25,6.75,7.25,5,4.25,4.25,5.25,4.5,3.75,5.75,7,0.75,3.25,4.25,8.25,7.75,3,2.5,6.75,1.5,3.5,3.75,2,8.5,5.25,5.75,7.5,8,8.5,4.75,3.25,1.5,5.5,9,0.5,6,9,6.75,3.75,1.25,4.75,7.5,6.75,5.5,4.25,2.75,4,6,5.75,1.75,5,5.75,8.75,4.5,5.5,1,2,8.5,6.5,7.5,7.75,6,3.5,1.5,4,6.5,4.25,3.25,1.5,8,4.75,4.5,4.5,1.25,8.5,8.75,3.75,4.25,6.25,3.75,5.25,7.75,5,7.5,2.25,4.75,7,5.25,3.75,4.75,2.75,6.75,4.75,8.75,8,4,5.25,4.5,9.25,3.25,2.5,6.25,6.75,4,5.25,5,6.5,8.25,3.25,6,1.5,1.5,2,6.25,4.75,6,5,4.5,7.75,8,5,6.5,3.75,1,6.75,2.5,8.75,9,0.5,8,1.75,4.5,1.75,7.25,5.25,7.25,5.5,1.75,3.75,3.75,2.5,7.75,7,7.5,2.25,1.75,1,4,3.75],"y":[7.5962805759999998,5.2817721090000003,6.1113243639999997,8.0996956820000001,6.9128036020000003,9.1844384530000003,9.2469484009999992,5.6765534779999998,7.454537846,6.1360699939999996,5.3026003580000003,7.7068944650000004,6.0590281629999998,8.9871785870000007,7.4308638360000003,7.4969762949999996,6.6009728150000004,7.206770015,7.8614471449999996,6.8763344530000001,6.6659774059999997,4.6636997180000002,8.1555390770000002,7.4553396699999999,4.4141344269999996,7.2384482859999997,5.3307743109999999,9.1517266419999999,7.9865830659999997,5.465632394,5.50954107,8.2160353409999995,5.3508203390000002,7.0215132730000001,4.9472567859999996,6.7778676940000002,8.1654289690000006,7.0625420480000001,8.456175752,8.5143939159999995,9.0297614139999993,9.3256399689999991,6.9425749620000001,7.5505553589999996,3.8945537730000002,6.917901981,9.0137012179999996,5.0640899409999998,7.7922043949999997,9.2943679170000006,8.7668029270000005,5.3155752219999997,5.3026847019999996,7.576535507,9.2351913779999997,7.8721778149999997,7.9467681499999996,6.5482459820000001,5.3712016069999997,6.8597217239999999,7.0417958729999999,7.1844425879999996,6.3864825969999997,8.5836795909999992,8.2589457730000007,9.083506216,6.1958713349999996,6.3813124600000002,4.5324492449999996,3.1160276200000001,8.9391294039999991,7.5449161419999999,8.6904463169999993,7.2684160929999999,8.7337705499999991,6.2386665170000004,6.375675545,6.44553669,8.7663498579999999,5.6305744740000003,6.2470129539999997,5.8635861709999997,8.8510680839999996,6.4299941790000004,6.6884439599999999,6.403401744,3.8805656229999999,8.8213389259999992,9.2574018220000003,7.4136580240000001,6.8351235089999998,7.4452136089999996,5.9029799540000001,6.0609294709999997,8.9779974619999994,4.9877497760000002,7.7174656319999997,6.7960203970000004,7.0590295249999997,7.9983125529999999,7.6301041329999997,5.9867565120000004,6.6592066450000003,5.4633608689999997,7.3949155019999999,6.91161139,9.3988423599999997,9.2848191569999994,6.4324947259999998,5.7145200970000003,5.8236172059999998,9.3424811329999997,5.5090552129999999,5.3245924479999998,7.7152400739999996,7.2549488159999997,6.9745981869999998,7.7207866950000001,6.4975266940000003,8.3531204549999991,9.0437122429999999,5.5255529770000003,8.6532324989999996,5.7143816650000003,6.4677390509999997,4.4387320690000003,6.7741877690000001,7.8479432520000003,7.3725243349999996,7.0446591520000004,7.4794236349999998,8.5364998910000001,8.8227914670000001,7.695143614,8.7582137640000006,6.0407218330000001,4.2460471709999998,7.7885630460000002,6.0203286570000003,9.3347502979999994,9.1855180989999994,2.5116537540000001,9.0413940369999999,5.6523245790000001,6.1264186929999997,6.3995626010000004,6.186449627,6.7299380099999997,6.9204463880000002,6.577472244,6.0924653089999996,6.1288890939999998,6.5862704079999999,5.0733501759999999,8.9958358720000007,8.7473102160000007,8.2961418059999996,5.5214489130000004,5.9680076,2.1665553399999999,6.0076988560000002,5.1707255749999996],"text":["pf_expression_control: 5.25<br />pf_score: 7.596281<br />pf_rank:  57","pf_expression_control: 4.00<br />pf_score: 5.281772<br />pf_rank: 147","pf_expression_control: 2.50<br />pf_score: 6.111324<br />pf_rank: 117","pf_expression_control: 5.50<br />pf_score: 8.099696<br />pf_rank:  42","pf_expression_control: 4.25<br />pf_score: 6.912804<br />pf_rank:  84","pf_expression_control: 7.75<br />pf_score: 9.184438<br />pf_rank:  11","pf_expression_control: 8.00<br />pf_score: 9.246948<br />pf_rank:   8","pf_expression_control: 0.25<br />pf_score: 5.676553<br />pf_rank: 131","pf_expression_control: 7.25<br />pf_score: 7.454538<br />pf_rank:  64","pf_expression_control: 0.75<br />pf_score: 6.136070<br />pf_rank: 114","pf_expression_control: 3.25<br />pf_score: 5.302600<br />pf_rank: 146","pf_expression_control: 7.50<br />pf_score: 7.706894<br />pf_rank:  54","pf_expression_control: 2.50<br />pf_score: 6.059028<br />pf_rank: 120","pf_expression_control: 9.25<br />pf_score: 8.987179<br />pf_rank:  19","pf_expression_control: 6.75<br />pf_score: 7.430864<br />pf_rank:  66","pf_expression_control: 7.25<br />pf_score: 7.496976<br />pf_rank:  61","pf_expression_control: 5.00<br />pf_score: 6.600973<br />pf_rank:  96","pf_expression_control: 4.25<br />pf_score: 7.206770<br />pf_rank:  73","pf_expression_control: 4.25<br />pf_score: 7.861447<br />pf_rank:  47","pf_expression_control: 5.25<br />pf_score: 6.876334<br />pf_rank:  86","pf_expression_control: 4.50<br />pf_score: 6.665977<br />pf_rank:  94","pf_expression_control: 3.75<br />pf_score: 4.663700<br />pf_rank: 153","pf_expression_control: 5.75<br />pf_score: 8.155539<br />pf_rank:  41","pf_expression_control: 7.00<br />pf_score: 7.455340<br />pf_rank:  63","pf_expression_control: 0.75<br />pf_score: 4.414134<br />pf_rank: 156","pf_expression_control: 3.25<br />pf_score: 7.238448<br />pf_rank:  72","pf_expression_control: 4.25<br />pf_score: 5.330774<br />pf_rank: 142","pf_expression_control: 8.25<br />pf_score: 9.151727<br />pf_rank:  12","pf_expression_control: 7.75<br />pf_score: 7.986583<br />pf_rank:  44","pf_expression_control: 3.00<br />pf_score: 5.465632<br />pf_rank: 138","pf_expression_control: 2.50<br />pf_score: 5.509541<br />pf_rank: 136","pf_expression_control: 6.75<br />pf_score: 8.216035<br />pf_rank:  39","pf_expression_control: 1.50<br />pf_score: 5.350820<br />pf_rank: 141","pf_expression_control: 3.50<br />pf_score: 7.021513<br />pf_rank:  79","pf_expression_control: 3.75<br />pf_score: 4.947257<br />pf_rank: 152","pf_expression_control: 2.00<br />pf_score: 6.777868<br />pf_rank:  90","pf_expression_control: 8.50<br />pf_score: 8.165429<br />pf_rank:  40","pf_expression_control: 5.25<br />pf_score: 7.062542<br />pf_rank:  75","pf_expression_control: 5.75<br />pf_score: 8.456176<br />pf_rank:  35","pf_expression_control: 7.50<br />pf_score: 8.514394<br />pf_rank:  34","pf_expression_control: 8.00<br />pf_score: 9.029761<br />pf_rank:  16","pf_expression_control: 8.50<br />pf_score: 9.325640<br />pf_rank:   4","pf_expression_control: 4.75<br />pf_score: 6.942575<br />pf_rank:  81","pf_expression_control: 3.25<br />pf_score: 7.550555<br />pf_rank:  59","pf_expression_control: 1.50<br />pf_score: 3.894554<br />pf_rank: 158","pf_expression_control: 5.50<br />pf_score: 6.917902<br />pf_rank:  83","pf_expression_control: 9.00<br />pf_score: 9.013701<br />pf_rank:  17","pf_expression_control: 0.50<br />pf_score: 5.064090<br />pf_rank: 150","pf_expression_control: 6.00<br />pf_score: 7.792204<br />pf_rank:  49","pf_expression_control: 9.00<br />pf_score: 9.294368<br />pf_rank:   5","pf_expression_control: 6.75<br />pf_score: 8.766803<br />pf_rank:  25","pf_expression_control: 3.75<br />pf_score: 5.315575<br />pf_rank: 144","pf_expression_control: 1.25<br />pf_score: 5.302685<br />pf_rank: 145","pf_expression_control: 4.75<br />pf_score: 7.576536<br />pf_rank:  58","pf_expression_control: 7.50<br />pf_score: 9.235191<br />pf_rank:   9","pf_expression_control: 6.75<br />pf_score: 7.872178<br />pf_rank:  46","pf_expression_control: 5.50<br />pf_score: 7.946768<br />pf_rank:  45","pf_expression_control: 4.25<br />pf_score: 6.548246<br />pf_rank:  99","pf_expression_control: 2.75<br />pf_score: 5.371202<br />pf_rank: 140","pf_expression_control: 4.00<br />pf_score: 6.859722<br />pf_rank:  87","pf_expression_control: 6.00<br />pf_score: 7.041796<br />pf_rank:  78","pf_expression_control: 5.75<br />pf_score: 7.184443<br />pf_rank:  74","pf_expression_control: 1.75<br />pf_score: 6.386483<br />pf_rank: 107","pf_expression_control: 5.00<br />pf_score: 8.583680<br />pf_rank:  32","pf_expression_control: 5.75<br />pf_score: 8.258946<br />pf_rank:  38","pf_expression_control: 8.75<br />pf_score: 9.083506<br />pf_rank:  13","pf_expression_control: 4.50<br />pf_score: 6.195871<br />pf_rank: 112","pf_expression_control: 5.50<br />pf_score: 6.381312<br />pf_rank: 108","pf_expression_control: 1.00<br />pf_score: 4.532449<br />pf_rank: 154","pf_expression_control: 2.00<br />pf_score: 3.116028<br />pf_rank: 160","pf_expression_control: 8.50<br />pf_score: 8.939129<br />pf_rank:  21","pf_expression_control: 6.50<br />pf_score: 7.544916<br />pf_rank:  60","pf_expression_control: 7.50<br />pf_score: 8.690446<br />pf_rank:  30","pf_expression_control: 7.75<br />pf_score: 7.268416<br />pf_rank:  70","pf_expression_control: 6.00<br />pf_score: 8.733771<br />pf_rank:  29","pf_expression_control: 3.50<br />pf_score: 6.238667<br />pf_rank: 111","pf_expression_control: 1.50<br />pf_score: 6.375676<br />pf_rank: 109","pf_expression_control: 4.00<br />pf_score: 6.445537<br />pf_rank: 102","pf_expression_control: 6.50<br />pf_score: 8.766350<br />pf_rank:  26","pf_expression_control: 4.25<br />pf_score: 5.630574<br />pf_rank: 133","pf_expression_control: 3.25<br />pf_score: 6.247013<br />pf_rank: 110","pf_expression_control: 1.50<br />pf_score: 5.863586<br />pf_rank: 127","pf_expression_control: 8.00<br />pf_score: 8.851068<br />pf_rank:  22","pf_expression_control: 4.75<br />pf_score: 6.429994<br />pf_rank: 104","pf_expression_control: 4.50<br />pf_score: 6.688444<br />pf_rank:  93","pf_expression_control: 4.50<br />pf_score: 6.403402<br />pf_rank: 105","pf_expression_control: 1.25<br />pf_score: 3.880566<br />pf_rank: 159","pf_expression_control: 8.50<br />pf_score: 8.821339<br />pf_rank:  24","pf_expression_control: 8.75<br />pf_score: 9.257402<br />pf_rank:   7","pf_expression_control: 3.75<br />pf_score: 7.413658<br />pf_rank:  67","pf_expression_control: 4.25<br />pf_score: 6.835124<br />pf_rank:  88","pf_expression_control: 6.25<br />pf_score: 7.445214<br />pf_rank:  65","pf_expression_control: 3.75<br />pf_score: 5.902980<br />pf_rank: 126","pf_expression_control: 5.25<br />pf_score: 6.060929<br />pf_rank: 119","pf_expression_control: 7.75<br />pf_score: 8.977997<br />pf_rank:  20","pf_expression_control: 5.00<br />pf_score: 4.987750<br />pf_rank: 151","pf_expression_control: 7.50<br />pf_score: 7.717466<br />pf_rank:  52","pf_expression_control: 2.25<br />pf_score: 6.796020<br />pf_rank:  89","pf_expression_control: 4.75<br />pf_score: 7.059030<br />pf_rank:  76","pf_expression_control: 7.00<br />pf_score: 7.998313<br />pf_rank:  43","pf_expression_control: 5.25<br />pf_score: 7.630104<br />pf_rank:  56","pf_expression_control: 3.75<br />pf_score: 5.986757<br />pf_rank: 124","pf_expression_control: 4.75<br />pf_score: 6.659207<br />pf_rank:  95","pf_expression_control: 2.75<br />pf_score: 5.463361<br />pf_rank: 139","pf_expression_control: 6.75<br />pf_score: 7.394916<br />pf_rank:  68","pf_expression_control: 4.75<br />pf_score: 6.911611<br />pf_rank:  85","pf_expression_control: 8.75<br />pf_score: 9.398842<br />pf_rank:   1","pf_expression_control: 8.00<br />pf_score: 9.284819<br />pf_rank:   6","pf_expression_control: 4.00<br />pf_score: 6.432495<br />pf_rank: 103","pf_expression_control: 5.25<br />pf_score: 5.714520<br />pf_rank: 129","pf_expression_control: 4.50<br />pf_score: 5.823617<br />pf_rank: 128","pf_expression_control: 9.25<br />pf_score: 9.342481<br />pf_rank:   2","pf_expression_control: 3.25<br />pf_score: 5.509055<br />pf_rank: 137","pf_expression_control: 2.50<br />pf_score: 5.324592<br />pf_rank: 143","pf_expression_control: 6.25<br />pf_score: 7.715240<br />pf_rank:  53","pf_expression_control: 6.75<br />pf_score: 7.254949<br />pf_rank:  71","pf_expression_control: 4.00<br />pf_score: 6.974598<br />pf_rank:  80","pf_expression_control: 5.25<br />pf_score: 7.720787<br />pf_rank:  51","pf_expression_control: 5.00<br />pf_score: 6.497527<br />pf_rank: 100","pf_expression_control: 6.50<br />pf_score: 8.353120<br />pf_rank:  36","pf_expression_control: 8.25<br />pf_score: 9.043712<br />pf_rank:  14","pf_expression_control: 3.25<br />pf_score: 5.525553<br />pf_rank: 134","pf_expression_control: 6.00<br />pf_score: 8.653232<br />pf_rank:  31","pf_expression_control: 1.50<br />pf_score: 5.714382<br />pf_rank: 130","pf_expression_control: 1.50<br />pf_score: 6.467739<br />pf_rank: 101","pf_expression_control: 2.00<br />pf_score: 4.438732<br />pf_rank: 155","pf_expression_control: 6.25<br />pf_score: 6.774188<br />pf_rank:  91","pf_expression_control: 4.75<br />pf_score: 7.847943<br />pf_rank:  48","pf_expression_control: 6.00<br />pf_score: 7.372524<br />pf_rank:  69","pf_expression_control: 5.00<br />pf_score: 7.044659<br />pf_rank:  77","pf_expression_control: 4.50<br />pf_score: 7.479424<br />pf_rank:  62","pf_expression_control: 7.75<br />pf_score: 8.536500<br />pf_rank:  33","pf_expression_control: 8.00<br />pf_score: 8.822791<br />pf_rank:  23","pf_expression_control: 5.00<br />pf_score: 7.695144<br />pf_rank:  55","pf_expression_control: 6.50<br />pf_score: 8.758214<br />pf_rank:  27","pf_expression_control: 3.75<br />pf_score: 6.040722<br />pf_rank: 121","pf_expression_control: 1.00<br />pf_score: 4.246047<br />pf_rank: 157","pf_expression_control: 6.75<br />pf_score: 7.788563<br />pf_rank:  50","pf_expression_control: 2.50<br />pf_score: 6.020329<br />pf_rank: 122","pf_expression_control: 8.75<br />pf_score: 9.334750<br />pf_rank:   3","pf_expression_control: 9.00<br />pf_score: 9.185518<br />pf_rank:  10","pf_expression_control: 0.50<br />pf_score: 2.511654<br />pf_rank: 161","pf_expression_control: 8.00<br />pf_score: 9.041394<br />pf_rank:  15","pf_expression_control: 1.75<br />pf_score: 5.652325<br />pf_rank: 132","pf_expression_control: 4.50<br />pf_score: 6.126419<br />pf_rank: 116","pf_expression_control: 1.75<br />pf_score: 6.399563<br />pf_rank: 106","pf_expression_control: 7.25<br />pf_score: 6.186450<br />pf_rank: 113","pf_expression_control: 5.25<br />pf_score: 6.729938<br />pf_rank:  92","pf_expression_control: 7.25<br />pf_score: 6.920446<br />pf_rank:  82","pf_expression_control: 5.50<br />pf_score: 6.577472<br />pf_rank:  98","pf_expression_control: 1.75<br />pf_score: 6.092465<br />pf_rank: 118","pf_expression_control: 3.75<br />pf_score: 6.128889<br />pf_rank: 115","pf_expression_control: 3.75<br />pf_score: 6.586270<br />pf_rank:  97","pf_expression_control: 2.50<br />pf_score: 5.073350<br />pf_rank: 149","pf_expression_control: 7.75<br />pf_score: 8.995836<br />pf_rank:  18","pf_expression_control: 7.00<br />pf_score: 8.747310<br />pf_rank:  28","pf_expression_control: 7.50<br />pf_score: 8.296142<br />pf_rank:  37","pf_expression_control: 2.25<br />pf_score: 5.521449<br />pf_rank: 135","pf_expression_control: 1.75<br />pf_score: 5.968008<br />pf_rank: 125","pf_expression_control: 1.00<br />pf_score: 2.166555<br />pf_rank: 162","pf_expression_control: 4.00<br />pf_score: 6.007699<br />pf_rank: 123","pf_expression_control: 3.75<br />pf_score: 5.170726<br />pf_rank: 148"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":["rgba(41,86,125,1)","rgba(79,163,229,1)","rgba(66,137,193,1)","rgba(35,74,109,1)","rgba(52,108,155,1)","rgba(23,50,77,1)","rgba(22,48,74,1)","rgba(72,149,209,1)","rgba(44,92,133,1)","rgba(65,134,189,1)","rgba(79,162,227,1)","rgba(39,84,122,1)","rgba(67,139,196,1)","rgba(26,56,85,1)","rgba(44,93,135,1)","rgba(42,89,129,1)","rgba(57,118,168,1)","rgba(47,99,142,1)","rgba(37,78,114,1)","rgba(53,110,157,1)","rgba(56,117,166,1)","rgba(82,169,236,1)","rgba(34,73,108,1)","rgba(43,91,131,1)","rgba(83,172,240,1)","rgba(47,98,141,1)","rgba(77,159,223,1)","rgba(23,51,78,1)","rgba(35,76,111,1)","rgba(75,155,218,1)","rgba(74,153,215,1)","rgba(34,72,106,1)","rgba(77,158,221,1)","rgba(50,104,149,1)","rgba(81,168,235,1)","rgba(54,113,162,1)","rgba(34,72,107,1)","rgba(48,101,145,1)","rgba(32,69,101,1)","rgba(32,68,100,1)","rgba(25,54,82,1)","rgba(20,45,70,1)","rgba(51,106,151,1)","rgba(42,88,127,1)","rgba(84,173,242,1)","rgba(51,107,154,1)","rgba(25,55,83,1)","rgba(81,166,232,1)","rgba(37,80,116,1)","rgba(20,46,71,1)","rgba(28,61,91,1)","rgba(78,161,225,1)","rgba(78,162,226,1)","rgba(41,87,126,1)","rgba(22,49,75,1)","rgba(36,77,113,1)","rgba(36,76,112,1)","rgba(58,121,172,1)","rgba(76,157,220,1)","rgba(53,111,158,1)","rgba(49,103,148,1)","rgba(48,100,144,1)","rgba(62,128,181,1)","rgba(31,66,98,1)","rgba(33,71,105,1)","rgba(23,52,79,1)","rgba(64,132,187,1)","rgba(62,129,182,1)","rgba(82,170,237,1)","rgba(85,175,245,1)","rgba(27,58,87,1)","rgba(42,88,128,1)","rgba(30,65,96,1)","rgba(46,97,139,1)","rgba(30,64,95,1)","rgba(63,131,186,1)","rgba(62,130,183,1)","rgba(59,124,175,1)","rgba(28,62,92,1)","rgba(73,151,212,1)","rgba(63,130,185,1)","rgba(70,145,205,1)","rgba(27,59,88,1)","rgba(60,125,178,1)","rgba(56,116,165,1)","rgba(61,126,179,1)","rgba(85,174,243,1)","rgba(28,60,90,1)","rgba(21,47,73,1)","rgba(45,94,136,1)","rgba(54,112,159,1)","rgba(44,92,134,1)","rgba(70,145,203,1)","rgba(67,138,195,1)","rgba(26,57,86,1)","rgba(81,167,234,1)","rgba(39,82,119,1)","rgba(54,112,160,1)","rgba(48,102,146,1)","rgba(35,75,110,1)","rgba(40,85,124,1)","rgba(69,143,201,1)","rgba(56,118,167,1)","rgba(76,156,219,1)","rgba(45,95,137,1)","rgba(52,109,156,1)","rgba(19,43,67,1)","rgba(21,47,72,1)","rgba(60,124,176,1)","rgba(71,147,207,1)","rgba(71,146,206,1)","rgba(19,44,68,1)","rgba(75,154,217,1)","rgba(77,160,224,1)","rgba(39,83,121,1)","rgba(46,97,140,1)","rgba(50,105,150,1)","rgba(38,81,118,1)","rgba(59,122,173,1)","rgba(32,69,102,1)","rgba(24,53,80,1)","rgba(73,152,213,1)","rgba(30,65,97,1)","rgba(72,148,208,1)","rgba(59,123,174,1)","rgba(83,171,238,1)","rgba(55,114,163,1)","rgba(37,79,115,1)","rgba(46,96,138,1)","rgba(49,102,147,1)","rgba(43,90,130,1)","rgba(31,67,99,1)","rgba(27,59,89,1)","rgba(40,84,123,1)","rgba(29,62,93,1)","rgba(68,140,198,1)","rgba(84,172,241,1)","rgba(38,80,117,1)","rgba(68,141,199,1)","rgba(20,44,69,1)","rgba(22,50,76,1)","rgba(86,176,246,1)","rgba(24,53,81,1)","rgba(73,150,211,1)","rgba(66,136,192,1)","rgba(61,127,180,1)","rgba(64,133,188,1)","rgba(55,115,164,1)","rgba(51,107,153,1)","rgba(58,120,171,1)","rgba(66,137,194,1)","rgba(65,135,190,1)","rgba(57,119,170,1)","rgba(80,165,231,1)","rgba(25,56,84,1)","rgba(29,63,94,1)","rgba(33,70,103,1)","rgba(74,153,214,1)","rgba(69,144,202,1)","rgba(86,177,247,1)","rgba(69,142,200,1)","rgba(80,164,230,1)"],"opacity":1,"size":7.559055118110237,"symbol":"circle","line":{"width":1.8897637795275593,"color":["rgba(41,86,125,1)","rgba(79,163,229,1)","rgba(66,137,193,1)","rgba(35,74,109,1)","rgba(52,108,155,1)","rgba(23,50,77,1)","rgba(22,48,74,1)","rgba(72,149,209,1)","rgba(44,92,133,1)","rgba(65,134,189,1)","rgba(79,162,227,1)","rgba(39,84,122,1)","rgba(67,139,196,1)","rgba(26,56,85,1)","rgba(44,93,135,1)","rgba(42,89,129,1)","rgba(57,118,168,1)","rgba(47,99,142,1)","rgba(37,78,114,1)","rgba(53,110,157,1)","rgba(56,117,166,1)","rgba(82,169,236,1)","rgba(34,73,108,1)","rgba(43,91,131,1)","rgba(83,172,240,1)","rgba(47,98,141,1)","rgba(77,159,223,1)","rgba(23,51,78,1)","rgba(35,76,111,1)","rgba(75,155,218,1)","rgba(74,153,215,1)","rgba(34,72,106,1)","rgba(77,158,221,1)","rgba(50,104,149,1)","rgba(81,168,235,1)","rgba(54,113,162,1)","rgba(34,72,107,1)","rgba(48,101,145,1)","rgba(32,69,101,1)","rgba(32,68,100,1)","rgba(25,54,82,1)","rgba(20,45,70,1)","rgba(51,106,151,1)","rgba(42,88,127,1)","rgba(84,173,242,1)","rgba(51,107,154,1)","rgba(25,55,83,1)","rgba(81,166,232,1)","rgba(37,80,116,1)","rgba(20,46,71,1)","rgba(28,61,91,1)","rgba(78,161,225,1)","rgba(78,162,226,1)","rgba(41,87,126,1)","rgba(22,49,75,1)","rgba(36,77,113,1)","rgba(36,76,112,1)","rgba(58,121,172,1)","rgba(76,157,220,1)","rgba(53,111,158,1)","rgba(49,103,148,1)","rgba(48,100,144,1)","rgba(62,128,181,1)","rgba(31,66,98,1)","rgba(33,71,105,1)","rgba(23,52,79,1)","rgba(64,132,187,1)","rgba(62,129,182,1)","rgba(82,170,237,1)","rgba(85,175,245,1)","rgba(27,58,87,1)","rgba(42,88,128,1)","rgba(30,65,96,1)","rgba(46,97,139,1)","rgba(30,64,95,1)","rgba(63,131,186,1)","rgba(62,130,183,1)","rgba(59,124,175,1)","rgba(28,62,92,1)","rgba(73,151,212,1)","rgba(63,130,185,1)","rgba(70,145,205,1)","rgba(27,59,88,1)","rgba(60,125,178,1)","rgba(56,116,165,1)","rgba(61,126,179,1)","rgba(85,174,243,1)","rgba(28,60,90,1)","rgba(21,47,73,1)","rgba(45,94,136,1)","rgba(54,112,159,1)","rgba(44,92,134,1)","rgba(70,145,203,1)","rgba(67,138,195,1)","rgba(26,57,86,1)","rgba(81,167,234,1)","rgba(39,82,119,1)","rgba(54,112,160,1)","rgba(48,102,146,1)","rgba(35,75,110,1)","rgba(40,85,124,1)","rgba(69,143,201,1)","rgba(56,118,167,1)","rgba(76,156,219,1)","rgba(45,95,137,1)","rgba(52,109,156,1)","rgba(19,43,67,1)","rgba(21,47,72,1)","rgba(60,124,176,1)","rgba(71,147,207,1)","rgba(71,146,206,1)","rgba(19,44,68,1)","rgba(75,154,217,1)","rgba(77,160,224,1)","rgba(39,83,121,1)","rgba(46,97,140,1)","rgba(50,105,150,1)","rgba(38,81,118,1)","rgba(59,122,173,1)","rgba(32,69,102,1)","rgba(24,53,80,1)","rgba(73,152,213,1)","rgba(30,65,97,1)","rgba(72,148,208,1)","rgba(59,123,174,1)","rgba(83,171,238,1)","rgba(55,114,163,1)","rgba(37,79,115,1)","rgba(46,96,138,1)","rgba(49,102,147,1)","rgba(43,90,130,1)","rgba(31,67,99,1)","rgba(27,59,89,1)","rgba(40,84,123,1)","rgba(29,62,93,1)","rgba(68,140,198,1)","rgba(84,172,241,1)","rgba(38,80,117,1)","rgba(68,141,199,1)","rgba(20,44,69,1)","rgba(22,50,76,1)","rgba(86,176,246,1)","rgba(24,53,81,1)","rgba(73,150,211,1)","rgba(66,136,192,1)","rgba(61,127,180,1)","rgba(64,133,188,1)","rgba(55,115,164,1)","rgba(51,107,153,1)","rgba(58,120,171,1)","rgba(66,137,194,1)","rgba(65,135,190,1)","rgba(57,119,170,1)","rgba(80,165,231,1)","rgba(25,56,84,1)","rgba(29,63,94,1)","rgba(33,70,103,1)","rgba(74,153,214,1)","rgba(69,144,202,1)","rgba(86,177,247,1)","rgba(69,142,200,1)","rgba(80,164,230,1)"]}},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[0],"y":[2],"name":"99_4583520b6f4c72ec455f359b3b5540df","type":"scatter","mode":"markers","opacity":0,"hoverinfo":"skip","showlegend":false,"marker":{"color":[0,1],"colorscale":[[0,"#132B43"],[0.0033444816053511696,"#132B44"],[0.0066889632107023393,"#132C44"],[0.010033444816053512,"#142C45"],[0.013377926421404682,"#142D45"],[0.016722408026755852,"#142D46"],[0.020066889632107024,"#142D46"],[0.023411371237458192,"#142E47"],[0.026755852842809364,"#152E47"],[0.030100334448160532,"#152F48"],[0.033444816053511704,"#152F48"],[0.036789297658862873,"#152F49"],[0.040133779264214048,"#153049"],[0.043478260869565216,"#16304A"],[0.046822742474916385,"#16304A"],[0.05016722408026756,"#16314B"],[0.053511705685618728,"#16314B"],[0.056856187290969896,"#16324C"],[0.060200668896321065,"#17324D"],[0.06354515050167224,"#17324D"],[0.066889632107023408,"#17334E"],[0.070234113712374577,"#17334E"],[0.073578595317725745,"#17344F"],[0.076923076923076913,"#18344F"],[0.080267558528428096,"#183450"],[0.083612040133779264,"#183550"],[0.086956521739130432,"#183551"],[0.090301003344481601,"#183651"],[0.093645484949832769,"#193652"],[0.096989966555183937,"#193652"],[0.10033444816053512,"#193753"],[0.10367892976588627,"#193754"],[0.10702341137123746,"#193854"],[0.11036789297658864,"#1A3855"],[0.11371237458193979,"#1A3955"],[0.11705685618729098,"#1A3956"],[0.12040133779264213,"#1A3956"],[0.12374581939799331,"#1A3A57"],[0.12709030100334448,"#1B3A57"],[0.13043478260869565,"#1B3B58"],[0.13377926421404682,"#1B3B59"],[0.13712374581939799,"#1B3B59"],[0.14046822742474915,"#1C3C5A"],[0.14381270903010032,"#1C3C5A"],[0.14715719063545149,"#1C3D5B"],[0.15050167224080266,"#1C3D5B"],[0.15384615384615383,"#1C3D5C"],[0.15719063545150502,"#1D3E5C"],[0.16053511705685619,"#1D3E5D"],[0.16387959866220736,"#1D3F5D"],[0.16722408026755853,"#1D3F5E"],[0.1705685618729097,"#1D3F5F"],[0.17391304347826086,"#1E405F"],[0.17725752508361203,"#1E4060"],[0.1806020066889632,"#1E4160"],[0.18394648829431437,"#1E4161"],[0.18729096989966554,"#1E4261"],[0.19063545150501671,"#1F4262"],[0.19397993311036787,"#1F4263"],[0.19732441471571904,"#1F4363"],[0.20066889632107024,"#1F4364"],[0.20401337792642141,"#1F4464"],[0.20735785953177255,"#204465"],[0.21070234113712372,"#204465"],[0.21404682274247491,"#204566"],[0.21739130434782608,"#204566"],[0.22073578595317728,"#214667"],[0.22408026755852842,"#214668"],[0.22742474916387959,"#214768"],[0.23076923076923075,"#214769"],[0.23411371237458195,"#214769"],[0.23745819397993309,"#22486A"],[0.24080267558528426,"#22486A"],[0.24414715719063546,"#22496B"],[0.24749163879598662,"#22496C"],[0.25083612040133774,"#224A6C"],[0.25418060200668896,"#234A6D"],[0.25752508361204013,"#234A6D"],[0.2608695652173913,"#234B6E"],[0.26421404682274247,"#234B6E"],[0.26755852842809363,"#244C6F"],[0.2709030100334448,"#244C70"],[0.27424749163879597,"#244C70"],[0.27759197324414714,"#244D71"],[0.28093645484949831,"#244D71"],[0.28428093645484948,"#254E72"],[0.28762541806020064,"#254E72"],[0.29096989966555187,"#254F73"],[0.29431438127090298,"#254F74"],[0.29765886287625415,"#254F74"],[0.30100334448160532,"#265075"],[0.30434782608695654,"#265075"],[0.30769230769230765,"#265176"],[0.31103678929765882,"#265176"],[0.31438127090301005,"#275277"],[0.31772575250836121,"#275278"],[0.32107023411371238,"#275278"],[0.3244147157190635,"#275379"],[0.32775919732441472,"#275379"],[0.33110367892976589,"#28547A"],[0.33444816053511706,"#28547B"],[0.33779264214046822,"#28557B"],[0.34113712374581939,"#28557C"],[0.34448160535117056,"#28567C"],[0.34782608695652173,"#29567D"],[0.3511705685618729,"#29567D"],[0.35451505016722407,"#29577E"],[0.35785953177257523,"#29577F"],[0.3612040133779264,"#2A587F"],[0.36454849498327757,"#2A5880"],[0.36789297658862874,"#2A5980"],[0.37123745819397991,"#2A5981"],[0.37458193979933108,"#2A5982"],[0.3779264214046823,"#2B5A82"],[0.38127090301003341,"#2B5A83"],[0.38461538461538458,"#2B5B83"],[0.38795986622073575,"#2B5B84"],[0.39130434782608697,"#2C5C85"],[0.39464882943143809,"#2C5C85"],[0.39799331103678931,"#2C5D86"],[0.40133779264214048,"#2C5D86"],[0.40468227424749159,"#2C5D87"],[0.40802675585284282,"#2D5E87"],[0.41137123745819393,"#2D5E88"],[0.4147157190635451,"#2D5F89"],[0.41806020066889632,"#2D5F89"],[0.42140468227424743,"#2E608A"],[0.42474916387959866,"#2E608A"],[0.42809364548494983,"#2E618B"],[0.43143812709030094,"#2E618C"],[0.43478260869565216,"#2E618C"],[0.43812709030100333,"#2F628D"],[0.44147157190635455,"#2F628D"],[0.44481605351170567,"#2F638E"],[0.44816053511705684,"#2F638F"],[0.45150501672240806,"#30648F"],[0.45484949832775917,"#306490"],[0.45819397993311028,"#306590"],[0.46153846153846151,"#306591"],[0.46488294314381268,"#306592"],[0.4682274247491639,"#316692"],[0.47157190635451501,"#316693"],[0.47491638795986618,"#316793"],[0.47826086956521741,"#316794"],[0.48160535117056852,"#326895"],[0.48494983277591974,"#326895"],[0.48829431438127091,"#326996"],[0.49163879598662202,"#326996"],[0.49498327759197325,"#326997"],[0.49832775919732436,"#336A98"],[0.50167224080267547,"#336A98"],[0.50501672240802675,"#336B99"],[0.50836120401337792,"#336B99"],[0.51170568561872909,"#346C9A"],[0.51505016722408026,"#346C9B"],[0.51839464882943143,"#346D9B"],[0.52173913043478259,"#346D9C"],[0.52508361204013376,"#346E9D"],[0.52842809364548493,"#356E9D"],[0.5317725752508361,"#356E9E"],[0.53511705685618727,"#356F9E"],[0.53846153846153844,"#356F9F"],[0.5418060200668896,"#3670A0"],[0.54515050167224077,"#3670A0"],[0.54849498327759194,"#3671A1"],[0.55183946488294311,"#3671A1"],[0.55518394648829428,"#3772A2"],[0.55852842809364545,"#3772A3"],[0.56187290969899661,"#3773A3"],[0.56521739130434778,"#3773A4"],[0.56856187290969895,"#3773A4"],[0.57190635451505012,"#3874A5"],[0.57525083612040129,"#3874A6"],[0.57859531772575246,"#3875A6"],[0.58193979933110374,"#3875A7"],[0.58528428093645479,"#3976A8"],[0.58862876254180596,"#3976A8"],[0.59197324414715713,"#3977A9"],[0.5953177257525083,"#3977A9"],[0.59866220735785958,"#3978AA"],[0.60200668896321063,"#3A78AB"],[0.6053511705685618,"#3A79AB"],[0.60869565217391308,"#3A79AC"],[0.61204013377926414,"#3A79AC"],[0.61538461538461531,"#3B7AAD"],[0.61872909698996659,"#3B7AAE"],[0.62207357859531764,"#3B7BAE"],[0.62541806020066892,"#3B7BAF"],[0.62876254180602009,"#3C7CB0"],[0.63210702341137115,"#3C7CB0"],[0.63545150501672243,"#3C7DB1"],[0.63879598662207349,"#3C7DB1"],[0.64214046822742477,"#3C7EB2"],[0.64548494983277593,"#3D7EB3"],[0.64882943143812699,"#3D7FB3"],[0.65217391304347827,"#3D7FB4"],[0.65551839464882944,"#3D7FB5"],[0.6588628762541805,"#3E80B5"],[0.66220735785953178,"#3E80B6"],[0.66555183946488294,"#3E81B6"],[0.66889632107023411,"#3E81B7"],[0.67224080267558528,"#3F82B8"],[0.67558528428093645,"#3F82B8"],[0.67892976588628762,"#3F83B9"],[0.68227424749163879,"#3F83BA"],[0.68561872909698984,"#4084BA"],[0.68896321070234112,"#4084BB"],[0.69230769230769229,"#4085BB"],[0.69565217391304346,"#4085BC"],[0.69899665551839463,"#4086BD"],[0.7023411371237458,"#4186BD"],[0.70568561872909696,"#4186BE"],[0.70903010033444813,"#4187BF"],[0.7123745819397993,"#4187BF"],[0.71571906354515047,"#4288C0"],[0.71906354515050164,"#4288C1"],[0.72240802675585281,"#4289C1"],[0.72575250836120397,"#4289C2"],[0.72909698996655514,"#438AC2"],[0.73244147157190631,"#438AC3"],[0.73578595317725748,"#438BC4"],[0.73913043478260865,"#438BC4"],[0.74247491638795982,"#438CC5"],[0.74581939799331098,"#448CC6"],[0.74916387959866215,"#448DC6"],[0.75250836120401332,"#448DC7"],[0.7558528428093646,"#448EC8"],[0.75919732441471566,"#458EC8"],[0.76254180602006683,"#458FC9"],[0.76588628762541811,"#458FC9"],[0.76923076923076916,"#458FCA"],[0.77257525083612033,"#4690CB"],[0.7759197324414715,"#4690CB"],[0.77926421404682267,"#4691CC"],[0.78260869565217395,"#4691CD"],[0.785953177257525,"#4792CD"],[0.78929765886287617,"#4792CE"],[0.79264214046822745,"#4793CF"],[0.79598662207357862,"#4793CF"],[0.79933110367892968,"#4894D0"],[0.80267558528428096,"#4894D0"],[0.80602006688963213,"#4895D1"],[0.80936454849498318,"#4895D2"],[0.81270903010033446,"#4896D2"],[0.81605351170568563,"#4996D3"],[0.81939799331103669,"#4997D4"],[0.82274247491638786,"#4997D4"],[0.82608695652173914,"#4998D5"],[0.82943143812709019,"#4A98D6"],[0.83277591973244136,"#4A99D6"],[0.83612040133779264,"#4A99D7"],[0.83946488294314381,"#4A9AD8"],[0.84280936454849487,"#4B9AD8"],[0.84615384615384615,"#4B9BD9"],[0.84949832775919731,"#4B9BDA"],[0.85284280936454837,"#4B9BDA"],[0.85618729096989965,"#4C9CDB"],[0.85953177257525082,"#4C9CDB"],[0.86287625418060188,"#4C9DDC"],[0.86622073578595316,"#4C9DDD"],[0.86956521739130432,"#4D9EDD"],[0.87290969899665538,"#4D9EDE"],[0.87625418060200666,"#4D9FDF"],[0.87959866220735783,"#4D9FDF"],[0.88294314381270911,"#4DA0E0"],[0.88628762541806017,"#4EA0E1"],[0.88963210702341133,"#4EA1E1"],[0.89297658862876261,"#4EA1E2"],[0.89632107023411367,"#4EA2E3"],[0.89966555183946484,"#4FA2E3"],[0.90301003344481612,"#4FA3E4"],[0.90635451505016706,"#4FA3E5"],[0.90969899665551834,"#4FA4E5"],[0.91304347826086951,"#50A4E6"],[0.91638795986622057,"#50A5E7"],[0.91973244147157185,"#50A5E7"],[0.92307692307692302,"#50A6E8"],[0.9264214046822743,"#51A6E8"],[0.92976588628762535,"#51A7E9"],[0.93311036789297652,"#51A7EA"],[0.9364548494983278,"#51A8EA"],[0.93979933110367886,"#52A8EB"],[0.94314381270903003,"#52A9EC"],[0.94648829431438131,"#52A9EC"],[0.94983277591973236,"#52AAED"],[0.95317725752508353,"#53AAEE"],[0.95652173913043481,"#53ABEE"],[0.95986622073578587,"#53ABEF"],[0.96321070234113704,"#53ACF0"],[0.96655518394648832,"#54ACF0"],[0.96989966555183948,"#54ADF1"],[0.97324414715719054,"#54ADF2"],[0.97658862876254182,"#54AEF2"],[0.97993311036789299,"#55AEF3"],[0.98327759197324405,"#55AFF4"],[0.98662207357859533,"#55AFF4"],[0.98996655518394649,"#55B0F5"],[0.99331103678929755,"#56B0F6"],[0.99665551839464872,"#56B1F6"],[1,"#56B1F7"]],"colorbar":{"bgcolor":"rgba(255,255,255,1)","bordercolor":"transparent","borderwidth":1.8897637795275593,"thickness":23.039999999999996,"title":"Horsepower","titlefont":{"color":"rgba(0,0,0,1)","family":"","size":14.611872146118724},"tickmode":"array","ticktext":["40","80","120","160"],"tickvals":[0.24223602484472051,0.49068322981366458,0.73913043478260865,0.98757763975155277],"tickfont":{"color":"rgba(0,0,0,1)","family":"","size":11.68949771689498},"ticklen":2,"len":0.5}},"xaxis":"x","yaxis":"y","frame":null}],"layout":{"margin":{"t":43.762557077625573,"r":7.3059360730593621,"b":40.182648401826498,"l":31.415525114155255},"plot_bgcolor":"rgba(235,235,235,1)","paper_bgcolor":"rgba(255,255,255,1)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.611872146118724},"title":{"text":"3D Scatter Plot","font":{"color":"rgba(0,0,0,1)","family":"","size":17.534246575342465},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-0.20000000000000001,9.6999999999999993],"tickmode":"array","ticktext":["0.0","2.5","5.0","7.5"],"tickvals":[0,2.5,5,7.5],"categoryorder":"array","categoryarray":["0.0","2.5","5.0","7.5"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.6529680365296811,"tickwidth":0.66417600664176002,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.68949771689498},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176002,"zeroline":false,"anchor":"y","title":{"text":"Weight","font":{"color":"rgba(0,0,0,1)","family":"","size":14.611872146118724}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[1.8049409889999999,9.7604567109999998],"tickmode":"array","ticktext":["2","4","6","8"],"tickvals":[2,4,6,8],"categoryorder":"array","categoryarray":["2","4","6","8"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.6529680365296811,"tickwidth":0.66417600664176002,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.68949771689498},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176002,"zeroline":false,"anchor":"x","title":{"text":"Miles per Gallon","font":{"color":"rgba(0,0,0,1)","family":"","size":14.611872146118724}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":"rgba(255,255,255,1)","bordercolor":"transparent","borderwidth":1.8897637795275593,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.68949771689498},"title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.611872146118724}}},"hovermode":"closest","barmode":"relative","scene":{"xaxis":{"title":"Weight"},"yaxis":{"title":"Miles per Gallon"},"zaxis":{"title":"Horsepower"}}},"config":{"doubleClick":"reset","modeBarButtonsToAdd":["hoverclosest","hovercompare"],"showSendToCloud":false},"source":"A","attrs":{"a11cb9d399b":{"x":{},"y":{},"colour":{},"type":"scatter"}},"cur_data":"a11cb9d399b","visdat":{"a11cb9d399b":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.20000000000000001,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>

``` r
# Create a 3D scatter plot
fig <- plot_ly(hfi_2016, x = ~pf_expression_control, y = ~pf_score, z = ~pf_rank, type = 'scatter3d', mode = 'markers')

fig
```

<div class="plotly html-widget html-fill-item-overflow-hidden html-fill-item" id="htmlwidget-2e6c21c36e5e1610921f" style="width:672px;height:480px;"></div>
<script type="application/json" data-for="htmlwidget-2e6c21c36e5e1610921f">{"x":{"visdat":{"a11c7f30f511":["function () ","plotlyVisDat"]},"cur_data":"a11c7f30f511","attrs":{"a11c7f30f511":{"x":{},"y":{},"z":{},"mode":"markers","alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"scatter3d"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"scene":{"xaxis":{"title":"pf_expression_control"},"yaxis":{"title":"pf_score"},"zaxis":{"title":"pf_rank"}},"hovermode":"closest","showlegend":false},"source":"A","config":{"modeBarButtonsToAdd":["hoverclosest","hovercompare"],"showSendToCloud":false},"data":[{"x":[5.25,4,2.5,5.5,4.25,7.75,8,0.25,7.25,0.75,3.25,7.5,2.5,9.25,6.75,7.25,5,4.25,4.25,5.25,4.5,3.75,5.75,7,0.75,3.25,4.25,8.25,7.75,3,2.5,6.75,1.5,3.5,3.75,2,8.5,5.25,5.75,7.5,8,8.5,4.75,3.25,1.5,5.5,9,0.5,6,9,6.75,3.75,1.25,4.75,7.5,6.75,5.5,4.25,2.75,4,6,5.75,1.75,5,5.75,8.75,4.5,5.5,1,2,8.5,6.5,7.5,7.75,6,3.5,1.5,4,6.5,4.25,3.25,1.5,8,4.75,4.5,4.5,1.25,8.5,8.75,3.75,4.25,6.25,3.75,5.25,7.75,5,7.5,2.25,4.75,7,5.25,3.75,4.75,2.75,6.75,4.75,8.75,8,4,5.25,4.5,9.25,3.25,2.5,6.25,6.75,4,5.25,5,6.5,8.25,3.25,6,1.5,1.5,2,6.25,4.75,6,5,4.5,7.75,8,5,6.5,3.75,1,6.75,2.5,8.75,9,0.5,8,1.75,4.5,1.75,7.25,5.25,7.25,5.5,1.75,3.75,3.75,2.5,7.75,7,7.5,2.25,1.75,1,4,3.75],"y":[7.5962805759999998,5.2817721090000003,6.1113243639999997,8.0996956820000001,6.9128036020000003,9.1844384530000003,9.2469484009999992,5.6765534779999998,7.454537846,6.1360699939999996,5.3026003580000003,7.7068944650000004,6.0590281629999998,8.9871785870000007,7.4308638360000003,7.4969762949999996,6.6009728150000004,7.206770015,7.8614471449999996,6.8763344530000001,6.6659774059999997,4.6636997180000002,8.1555390770000002,7.4553396699999999,4.4141344269999996,7.2384482859999997,5.3307743109999999,9.1517266419999999,7.9865830659999997,5.465632394,5.50954107,8.2160353409999995,5.3508203390000002,7.0215132730000001,4.9472567859999996,6.7778676940000002,8.1654289690000006,7.0625420480000001,8.456175752,8.5143939159999995,9.0297614139999993,9.3256399689999991,6.9425749620000001,7.5505553589999996,3.8945537730000002,6.917901981,9.0137012179999996,5.0640899409999998,7.7922043949999997,9.2943679170000006,8.7668029270000005,5.3155752219999997,5.3026847019999996,7.576535507,9.2351913779999997,7.8721778149999997,7.9467681499999996,6.5482459820000001,5.3712016069999997,6.8597217239999999,7.0417958729999999,7.1844425879999996,6.3864825969999997,8.5836795909999992,8.2589457730000007,9.083506216,6.1958713349999996,6.3813124600000002,4.5324492449999996,3.1160276200000001,8.9391294039999991,7.5449161419999999,8.6904463169999993,7.2684160929999999,8.7337705499999991,6.2386665170000004,6.375675545,6.44553669,8.7663498579999999,5.6305744740000003,6.2470129539999997,5.8635861709999997,8.8510680839999996,6.4299941790000004,6.6884439599999999,6.403401744,3.8805656229999999,8.8213389259999992,9.2574018220000003,7.4136580240000001,6.8351235089999998,7.4452136089999996,5.9029799540000001,6.0609294709999997,8.9779974619999994,4.9877497760000002,7.7174656319999997,6.7960203970000004,7.0590295249999997,7.9983125529999999,7.6301041329999997,5.9867565120000004,6.6592066450000003,5.4633608689999997,7.3949155019999999,6.91161139,9.3988423599999997,9.2848191569999994,6.4324947259999998,5.7145200970000003,5.8236172059999998,9.3424811329999997,5.5090552129999999,5.3245924479999998,7.7152400739999996,7.2549488159999997,6.9745981869999998,7.7207866950000001,6.4975266940000003,8.3531204549999991,9.0437122429999999,5.5255529770000003,8.6532324989999996,5.7143816650000003,6.4677390509999997,4.4387320690000003,6.7741877690000001,7.8479432520000003,7.3725243349999996,7.0446591520000004,7.4794236349999998,8.5364998910000001,8.8227914670000001,7.695143614,8.7582137640000006,6.0407218330000001,4.2460471709999998,7.7885630460000002,6.0203286570000003,9.3347502979999994,9.1855180989999994,2.5116537540000001,9.0413940369999999,5.6523245790000001,6.1264186929999997,6.3995626010000004,6.186449627,6.7299380099999997,6.9204463880000002,6.577472244,6.0924653089999996,6.1288890939999998,6.5862704079999999,5.0733501759999999,8.9958358720000007,8.7473102160000007,8.2961418059999996,5.5214489130000004,5.9680076,2.1665553399999999,6.0076988560000002,5.1707255749999996],"z":[57,147,117,42,84,11,8,131,64,114,146,54,120,19,66,61,96,73,47,86,94,153,41,63,156,72,142,12,44,138,136,39,141,79,152,90,40,75,35,34,16,4,81,59,158,83,17,150,49,5,25,144,145,58,9,46,45,99,140,87,78,74,107,32,38,13,112,108,154,160,21,60,30,70,29,111,109,102,26,133,110,127,22,104,93,105,159,24,7,67,88,65,126,119,20,151,52,89,76,43,56,124,95,139,68,85,1,6,103,129,128,2,137,143,53,71,80,51,100,36,14,134,31,130,101,155,91,48,69,77,62,33,23,55,27,121,157,50,122,3,10,161,15,132,116,106,113,92,82,98,118,115,97,149,18,28,37,135,125,162,123,148],"mode":"markers","type":"scatter3d","marker":{"color":"rgba(31,119,180,1)","line":{"color":"rgba(31,119,180,1)"}},"error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"line":{"color":"rgba(31,119,180,1)"},"frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.20000000000000001,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>

After doing this, respond to the following prompt:

6.  Compare your 3-D scatterplot and the `GGally::ggpairs` output. The
    3D gives a clearer visualization. Comment on the strengths and
    weaknesses of these two visualizations. GGpairs is easier to
    interpret while 3D though gives a 3dimensional angle of the plot may
    be difficult to interpret. The advantage is you can rotate the plot.
    Do both display on GitHub when you push your work there? 3D does not
    load in Github.

# Day 2

During Day 1, you fit a model with one quantitative response variable
and two quantitative explanatory variables. Now we look at a model with
one quantitative explanatory variable and one qualitative explanatory
variable. We will use the full 2016 dataset for this entire activity.
For the Mini-Competition next week, you will be instructed to use the
train/test split process.

## Fitting the overall model

This is similar to what we have already been doing - fitting our desired
model. For today’s activity, we will fit something like:

\#$$
y = \beta_0 + \beta_1 \times \text{qualitative\\_variable} + \beta_2 \times \text{quantitative\\_variable} + \varepsilon
#$$

where $y$, $\text{qualitative\\_variable}$, and
$\text{quantitative\\_variable}$ are from `hfi_2016`. Note that the two
explanatory variables can be entered in whatever order.

To help with interpretability, we will focus on qualitative predictor
variables with only two levels. Unfortunately, none of the current `chr`
variables have only two levels. Fortunately, we can create our own.

- In the code chunk below titled `binary-pred`, replace “verbatim” with
  “r” just before the code chunk title.
- Run your code chunk or knit your document.

``` r
hfi_2016 <- hfi_2016 %>%
  mutate(west_atlantic = if_else(
    region %in% c("North America", "Latin America & the Caribbean"),
    "No",
    "Yes"
  ))
```

7.  What is happening in the above code? What new variable did we
    create? How do you know it is new? What values does it take when?

Based on the above, the binary indicator was assigned YES if the region
is NOT Latin America and the Caribbean.

- In the code chunk below titled `qual-mlr`, replace “verbatim” with “r”
  just before the code chunk title.
- Run your code chunk or knit your document.

``` r
# review any visual patterns
hfi_2016 %>% 
  select(pf_score, west_atlantic, pf_expression_control) %>% 
  ggpairs()
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](activity03_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

``` r
#fit the mlr model
lm_spec <- linear_reg() %>%
  set_mode("regression") %>%
  set_engine("lm")

qual_mod <- lm_spec %>% 
  fit(pf_score ~ west_atlantic + pf_expression_control, data = hfi_2016)

# model output
tidy(qual_mod)
```

    ## # A tibble: 3 × 5
    ##   term                  estimate std.error statistic  p.value
    ##   <chr>                    <dbl>     <dbl>     <dbl>    <dbl>
    ## 1 (Intercept)              4.38     0.213     20.5   1.57e-46
    ## 2 west_atlanticYes        -0.102    0.167     -0.612 5.41e- 1
    ## 3 pf_expression_control    0.540    0.0273    19.8   1.01e-44

When looking at your `ggpairs` output, remember to ask yourself, “does
it make sense to include all of these variables?” Specifically, if you
notice that the response variables are highly correlated (collinear),
including both does not necessarily add much value as they are
essentially saying the same thing. Note: There are more advanced methods
to include the variability within a rater for our model - this is beyond
STA 631. If this sounds of interest to you, explore *generalized
estimating equations* (GEE) or *generalized linear mixed models* (GLMM).
However, there are often times when we choose to include variables in
our model because it is important to us - for various reasons.
Regardless, I encourage you to keep your readings of *DF* in mind - who
will benefit by including this information; who will be hurt by
including this information?

Also, when looking at your model (`tidy`) output, the `term` label for
your qualitative explanatory variable look odd. Answer the following
questions:

8.  What is the label that R assigned to this explanatory variable
    `term`? west_atlantic.

9.  What information is represented here? YES if the region is NOT Latin
    America and the Caribbean.

10. What information is missing here? what to do if there is another
    region, not sure of how to interpret it.

Your are essentially fitting two models (or $k$ models, where $k$ is the
number of levels in your qualitative variable). From your reading, you
learned that R is creating an indicator variable (see p. 83). If you
have 3 levels in your qualitative variable, you would have 2 (3 - 1)
indicator variables. If you have $k$ levels in your qualitative
variable, you would have $k - 1$ indicator variables.

The decision for R to call the indicator variable by one of your levels
instead of the other has no deeper meaning. R simply codes the level
that comes first alphabetically with a $0$ for your indicator variable.
You can change this reference level of a categorical variable, which is
the level that is coded as a 0, using the `relevel` function. Use
`?relevel` to learn more.

11. Write the estimated equation for your MLR model with a qualitative
    explanatory variable.

y = 4.377 + -.1  + .54  +

12. Now, for each level of your qualitative variable, write the
    simplified equation of the estimated line for that level. Note that
    if your qualitative variable has two levels, you should have two
    simplified equations.

The interpretation of the coefficients (parameter estimates) in multiple
regression is slightly different from that of simple regression. The
estimate for the indicator variable reflects how much more a group is
expected to be if something has that quality, *while holding all other
variables constant*. The estimate for the quantitative variable reflects
how much change in the response variable occurs due to a 1-unit increase
in the quantitative variable, *while holding all other variables
constant*.

13. Interpret the parameter estimate for the reference level of your
    categorical variable in the context of your problem. Page 83 of the
    text can help here (or have me come chat with you).

14. Interpret the parameter estimate for your quantitative variable in
    the context of your problem.

## Challenge: Multiple levels

Below, create a new R code chunk (with a descriptive name) that fits a
new model with the same response (`pf_score`) and quantitative
explanatory variable (`pf_expression_control`), but now use a
qualitative variable with more than two levels (say, `region`) and
obtain the `tidy` model output.

How does R appear to handle categorical variables with more than two
levels? Every region is included into the model.

``` r
hfi_2016_update<- hfi_2016|>
  mutate(west_atlantic = factor(west_atlantic, levels = c("Yes", "No")))

qual_mod <- lm_spec |> 
  fit(pf_score ~ region + pf_expression_control, data = hfi_2016_update)

# model output
tidy(qual_mod)
```

    ## # A tibble: 11 × 5
    ##    term                                estimate std.error statistic  p.value
    ##    <chr>                                  <dbl>     <dbl>     <dbl>    <dbl>
    ##  1 (Intercept)                            5.39     0.272     19.8   7.47e-44
    ##  2 regionEast Asia                        0.496    0.380      1.31  1.93e- 1
    ##  3 regionEastern Europe                   0.326    0.309      1.06  2.93e- 1
    ##  4 regionLatin America & the Caribbean   -0.229    0.300     -0.762 4.47e- 1
    ##  5 regionMiddle East & North Africa      -1.39     0.299     -4.64  7.40e- 6
    ##  6 regionNorth America                    0.610    0.542      1.13  2.62e- 1
    ##  7 regionOceania                          0.233    0.433      0.537 5.92e- 1
    ##  8 regionSouth Asia                      -0.716    0.305     -2.35  2.02e- 2
    ##  9 regionSub-Saharan Africa              -0.746    0.283     -2.64  9.24e- 3
    ## 10 regionWestern Europe                   0.522    0.345      1.52  1.32e- 1
    ## 11 pf_expression_control                  0.387    0.0299    12.9   2.99e-26

# Day 3

We will explore a MLR model with an interaction between quantitative and
qualitative explanatory variables as well as see some other methods to
assess the fit of our model. From the modeling process we came up with
as a class, we will now address the “series of important questions that
we should consider when performing multiple linear regression” (*ISL*
[Section 3.2.2](https://hastie.su.domains/ISLR2/ISLRv2_website.pdf),
p. 75):

1.  Is at least one of the $p$ predictors $X_1$, $X_2$, $\ldots$, $X_p$
    useful in predicting the response $Y$?
2.  Do all the predictors help to explain $Y$, or is only a subset of
    the predictors useful?
3.  How well does the model fit the data?
4.  Given a set of predictor values, what response value should we
    predict and how accurate is our prediction?

Note that the text (*ISLR*) covers interactions between two quantitative
explanatory variables as well. By including an interaction term in our
model, it may seem like we are relaxing the “additive assumption” a
little. However, the additive assumption is about the coefficients (the
$\beta$s) and not the variables.

## Fitting the overall model with $qualitative \times quantitative$ interaction

Recall from Day 2 that you explored the model:

$$
y = \beta_0 + \beta_1 \times \text{qualitative\\_variable} + \beta_2 \times \text{quantitative\\_variable} + \varepsilon
$$

Today we will explore a similar model, except that also includes the
interaction between your qualitative and quantitative explanatory
variables. That is,

$$
y = \beta_0 + \beta_1 \times \text{qualitative\\_variable} + \beta_2 \times \text{quantitative\\_variable} + \beta_3 \times ( \text{qualitative\\_variable} \times \text{quantitative\\_variable}) + \varepsilon
$$

- Run all previous code up to this point - you will need your prior
  dataset of just 2016 observations with the `west_atlantic` variable.
- In the code chunk below titled `int-mlr`, replace “verbatim” with “r”
  just before the code chunk title.
- Run your code chunk or knit your document.

``` r
# review any visual patterns
hfi_2016 %>% 
  select(pf_score, west_atlantic, pf_expression_control) %>% 
  ggpairs()
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](activity03_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

``` r
#fit the mlr model
lm_spec <- linear_reg() %>%
  set_mode("regression") %>%
  set_engine("lm")

int_mod <- lm_spec %>% 
  fit(pf_score ~ west_atlantic * pf_expression_control, data = hfi_2016)

# model output
tidy(int_mod)
```

    ## # A tibble: 4 × 5
    ##   term                                   estimate std.error statistic  p.value
    ##   <chr>                                     <dbl>     <dbl>     <dbl>    <dbl>
    ## 1 (Intercept)                               5.72     0.459      12.5  2.76e-25
    ## 2 west_atlanticYes                         -1.60     0.484      -3.30 1.18e- 3
    ## 3 pf_expression_control                     0.296    0.0789      3.75 2.45e- 4
    ## 4 west_atlanticYes:pf_expression_control    0.275    0.0838      3.28 1.26e- 3

Note that I shortened the model statement using
`qualitative * quantitative`, but this can sometimes be confusing to
read. Another way to write the right-hand side of the equation is:
`qualitative + quantitative + qualitative * quantitative`.

After doing this, answer the following question:

15. When viewing the `tidy` output, notice that the interaction term is
    listed as `qualitativelevel:quantitative`. Referring back to Day 2
    with how R displays qualitative variables, interpret what this
    syntax means.

The syntax qualitativelevel:quantitative in the context of multiple
linear regression typically refers to an interaction term between a
qualitative (categorical) variable and a quantitative (continuous)
variable.

for example, in this regression model, the interaction term coefficient
indicates how the relationship between pf_expression_control and the
dependent variable pf_score differs depending on whether the region is
the Western Atlantic or not.

16. Using page 100 of *ISLR* as a reference, if needed, and your work
    from Day 2, write the simplified equation of the line corresponding
    to each level of your qualitative explanatory variable.

pf_score=5.721−1.598×west_atlanticYes+0.296×pf_expression_control+0.275×(west_atlanticYes×pf_expression_control)
This equation indicates how pf_score is influenced by being in the
Western Atlantic region, freedom of expression control, and their
interaction. All coefficients are statistically significant, suggesting
that these factors meaningfully contribute to predicting pf_score.

17. For two observations with similar values of the quantitative , which
    level tends to have higher values of the response variable? For two
    observations with similar values of pf_expression_control, the level
    (west_atlanticYes) that tends to have higher values of the response
    variable (pf_score) depends on the specific value of
    pf_expression_control:

For lower values of pf_expression_control, countries not in the Western
Atlantic have higher pf_score. For higher values of
pf_expression_control, countries in the Western Atlantic have higher
pf_score.

To determine which level of the categorical (qualitative) variable tends
to have higher values of the response variable for two observations with
similar values of the continuous (quantitative) variable, we need to
look at the coefficients in the multiple linear regression (MLR) model,
particularly focusing on the main effect of the qualitative variable and
the interaction term.

18. Like you did in Day 1, assess the fit of this model (no need to do
    any formal hypothesis testing - we will explore this next). How does
    `int_mod`’s fit compare to `mlr_mod`? What did you use to compare
    these? Why?

Recall our brief discussion on how many disciplines are moving away from
$p$-values in favor of other methods. We will explore $p$-values these
other methods later this semester, but we will practice our classical
methods here. This is known as an “overall $F$ test” and the hypotheses
are:

That (the null) no predictors are useful for the model (i.e., all slopes
are equal to zero) versus the alternative that at least one predictor is
useful for the model (i.e., at least one slope is not zero). One way to
check this is to build our null model (no predictors) and then compare
this to our candidate model (`int_mod`).

- In the code chunk below titled `mod-comp`, replace “verbatim” with “r”
  just before the code chunk title.

\#\`\`\`{r} \# null model null_mod \<- lm_spec %\>% fit(response ~ 1,
data = data)

anova( extract_fit_engine(int_mod), extract_fit_engine(null_mod) )
\`\`\`

19. Using your background knowledge of $F$ tests, what is the $F$ test
    statistic and $p$-value for this test? Based on an $\alpha = 0.05$
    significant level, what should you conclude?

Using your background knowledge of $F$ tests, what is the $F$ test
statistic and $p$-value for this test? Based on an $\alpha = 0.05$
significant level, what should you conclude? F test statistic F\* =
3.284 $p$-value = 1.262236×10−3 (0.001262236) For each slope, you are
testing if that slope is zero (when including the other variables, the
null) or if it is not zero (when including the other variables, the
alternative).

## Partial slope test - do all predictors help explain $y$?

Assuming that your overall model is significant (at least one predictor
is useful), we will continue on. Continue through these next tasks even
if your overall model was not significant.

We could do a similar process to fit a new model while removing one
explanatory variable at at time, and using `anova` to compare these
models. However, the `tidy` output also helps here (the `statistic` and
`p.value` columns).

For each slope, you are testing if that slope is zero (when including
the other variables, the null) or if it is not zero (when including the
other variables, the alternative). Because the interaction term is a
combination of the other two variables, we should assess the first.

20. What is the $t$ test statistic and $p$-value associated with this
    test? Based on an $\alpha = 0.05$ significant level, what should you
    conclude?

Using your background knowledge of $F$ tests, what is the $F$ test
statistic and $p$-value for this test? Based on an $\alpha = 0.05$
significant level, what should you conclude? For the interaction term
{west_atlanticYes:pf_expression_control}: t-statistic: t*=3.283544
p-value: 0.001262236 Hypothesis Testing Null hypothesis (𝐻0): The
coefficient of the interaction term is zero (𝛽3=0) Alternative
hypothesis (𝐻𝐴): The coefficient of the interaction term is not zero
(𝛽3≠0) Calculating the t-Statistic and p-Value t*=3.283544. p-value
p=0.001262236.

Conclusion at 𝛼=0.05 Significance Level To determine if the interaction
term is statistically significant at the 𝛼=0.05, since p\<0.05, we
reject the null hypothesis. Conclusion Based on the 𝑡t-statistic of
3.283544 and a p-value of 0.001262236, we conclude that the interaction
term {west_atlanticYes:pf_expression_control} is statistically
significant at the α=0.05 significance level. This indicates that the
interaction between west_atlanticYes and pf_expression_control
significantly contributes to the prediction of pf_score.

If your interaction term was not significant, you could consider
removing it. Now look at your two non-interaction terms…

21. What are the $t$ test statistic and $p$-value associated with these
    tests? Based on an $\alpha = 0.05$ significant level, what should
    you conclude about these two predictors?

Testing the Predictors For west_atlanticYes: t-statistic: -3.304640
p-value: 0.001176686 For pf_expression_control: t-statistic: 3.753059
p-value: 0.0002449722

Hypothesis Testing Null hypothesis (𝐻0): The coefficient is zero (β=0).
Alternative hypothesis (HA): The coefficient is not zero (𝛽≠0)
Conclusion at α=0.05 Significance Level

For west_atlanticYes t-statistic: t\* = -3.304640 p-value: 0.001176686
Since p\<0.05, we reject the null hypothesis. This means that
west_atlanticYes is a statistically significant predictor of pf_score.

For pf_expression_control t-statistic: t\* = 3.753059 p-value:
0.0002449722 Since p\<0.05, we reject the null hypothesis. This means
that pf_expression_control is a statistically significant predictor of
pf_score.

Conclusion Based on an α=0.05 significance level, we conclude that both
west_atlanticYes and pf_expression_control are statistically significant
predictors of pf_score. This means that the coefficients for these
predictors are significantly different from zero, indicating that they
have a meaningful impact on the response variable pf_score.

You would not need to do (21) if the interaction was significant. You
also should not remove a main variable (non-interaction variable) if the
interaction variable remains in your model.

## Residual assessment - how well does the model fit the data?

You have already done this step in past activities by exploring your
residuals (Activity 2). Using your final model from Task 3, assess how
well your model fits the data.

R^2 (R-squared) 0.7141342, indicates that approximately 71.41% of the
variance in the response variable (pf_score) is explained by the
predictor (pf_expression_control). This suggests a strong relationship
between the predictor and the response variable. Residual Standard Error
(sigma) 0.7992708, this is the standard deviation of the residuals
(prediction errors). A smaller sigma indicates that the model’s
predictions are close to the actual values. F-statistic F\*= 399.7033
p-value: 2.313214×10−45 The F-statistic tests whether the model explains
a significant portion of the variance in the response variable. The very
high F-statistic and extremely low p-value suggest that the model is
highly significant.

Conclusion at α=0.05 Significance Level 1. Significance of Predictors: o
Both the intercept and pf_expression_control are highly significant,
given the very low p-values associated with their coefficients. 2. Model
Fit: o R^2The high R^2 values indicate that the model explains a
substantial portion of the variance in the response variable. o Residual
Standard Error (sigma): The relatively small value of sigma suggests
that the model’s predictions are fairly accurate. o F-statistic: The
very high F-statistic with a p-value much smaller than 0.05 indicates
that the overall model is statistically significant. Based on these
metrics, the model fits the data well, and both the predictor
pf_expression_control and the interaction term (if considered)
significantly contribute to explaining the variance in the response
variable pf_score. Proficiency for Simple and Multiple Linear Regression
Characterize whether you feel that you are proficient, aware, or unaware
of each of the five course objectives (for simple and multiple linear
regression only).

I have familiarized myself with the course objectives for simple and
multiple linear regression, and I understand what is expected of me. As
I continue to learn and develop my skills, I aim to become proficient in
both simple linear regression and multiple linear regression. However, I
still face challenges in interpreting multiple linear regression models.
