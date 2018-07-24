The Basics
================
John Adams

The Basics - Introduction
=========================

This document contains tips on the basics of working with R and Rstudio along with a few of the packages that will make your coding life more manageable.

Load Packages and Libraries
---------------------------

There are specific packages others have built that allow you to do complex tasks with little effort. When you first use R, be sure to install the necessary packages. After you install them, you need to be sure to load the libraries for each of the packages that you will use for your analysis.

For this class, we will use a number of different packages but we will start with the following package.

-   tidyverse: This contains, among other things, the packages ggplot, which allows you to make visualizations and dplyr, which will enable you to wrangle your data frame.

``` r
library(tidyverse)
```

    ## ── Attaching packages ────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 2.2.1     ✔ purrr   0.2.4
    ## ✔ tibble  1.4.2     ✔ dplyr   0.7.6
    ## ✔ tidyr   0.8.0     ✔ stringr 1.3.0
    ## ✔ readr   1.1.1     ✔ forcats 0.3.0

    ## Warning: package 'dplyr' was built under R version 3.4.4

    ## ── Conflicts ───────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

Viewing Your Data
-----------------

This code will allow you to see the variables and the first few observations. The default is to show six observations. To change the number of rows that appear, add a comma, and then the number of rows you want to show up.

``` r
head(diamonds)
```

    ## # A tibble: 6 x 10
    ##   carat cut       color clarity depth table price     x     y     z
    ##   <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ## 1 0.230 Ideal     E     SI2      61.5   55.   326  3.95  3.98  2.43
    ## 2 0.210 Premium   E     SI1      59.8   61.   326  3.89  3.84  2.31
    ## 3 0.230 Good      E     VS1      56.9   65.   327  4.05  4.07  2.31
    ## 4 0.290 Premium   I     VS2      62.4   58.   334  4.20  4.23  2.63
    ## 5 0.310 Good      J     SI2      63.3   58.   335  4.34  4.35  2.75
    ## 6 0.240 Very Good J     VVS2     62.8   57.   336  3.94  3.96  2.48

``` r
head(diamonds, 10)
```

    ## # A tibble: 10 x 10
    ##    carat cut       color clarity depth table price     x     y     z
    ##    <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1 0.230 Ideal     E     SI2      61.5   55.   326  3.95  3.98  2.43
    ##  2 0.210 Premium   E     SI1      59.8   61.   326  3.89  3.84  2.31
    ##  3 0.230 Good      E     VS1      56.9   65.   327  4.05  4.07  2.31
    ##  4 0.290 Premium   I     VS2      62.4   58.   334  4.20  4.23  2.63
    ##  5 0.310 Good      J     SI2      63.3   58.   335  4.34  4.35  2.75
    ##  6 0.240 Very Good J     VVS2     62.8   57.   336  3.94  3.96  2.48
    ##  7 0.240 Very Good I     VVS1     62.3   57.   336  3.95  3.98  2.47
    ##  8 0.260 Very Good H     SI1      61.9   55.   337  4.07  4.11  2.53
    ##  9 0.220 Fair      E     VS2      65.1   61.   337  3.87  3.78  2.49
    ## 10 0.230 Very Good H     VS1      59.4   61.   338  4.00  4.05  2.39

Using the Select Function
-------------------------

This function is particularly helpful when you have a lot of variables in your dataset. If, for example, you had 50 variables, it would be hard to focus on the few variables you want to analyze. The select function allows you to create a new data frame that includes only the variables you want.

The diamonds dataset has ten variables. Let's make a new data frame that only includes cut, clarity, carat, color and the price.

``` r
fourcs_price <- diamonds %>%
                select(carat, cut, color, clarity, price)
head(fourcs_price)
```

    ## # A tibble: 6 x 5
    ##   carat cut       color clarity price
    ##   <dbl> <ord>     <ord> <ord>   <int>
    ## 1 0.230 Ideal     E     SI2       326
    ## 2 0.210 Premium   E     SI1       326
    ## 3 0.230 Good      E     VS1       327
    ## 4 0.290 Premium   I     VS2       334
    ## 5 0.310 Good      J     SI2       335
    ## 6 0.240 Very Good J     VVS2      336

Using the Filter Function
-------------------------

Often times you don't want to focus on a certain subset of your dataset. Using this diamonds dataset, for example, we may want to just examine the ideal cut or the diamonds below a certain price. The Filter function allows us to take out the subset of data we want.

``` r
ideals <- diamonds %>%
         filter(cut == "Ideal")
```

    ## Warning: package 'bindrcpp' was built under R version 3.4.4

``` r
ideals
```

    ## # A tibble: 21,551 x 10
    ##    carat cut   color clarity depth table price     x     y     z
    ##    <dbl> <ord> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1 0.230 Ideal E     SI2      61.5   55.   326  3.95  3.98  2.43
    ##  2 0.230 Ideal J     VS1      62.8   56.   340  3.93  3.90  2.46
    ##  3 0.310 Ideal J     SI2      62.2   54.   344  4.35  4.37  2.71
    ##  4 0.300 Ideal I     SI2      62.0   54.   348  4.31  4.34  2.68
    ##  5 0.330 Ideal I     SI2      61.8   55.   403  4.49  4.51  2.78
    ##  6 0.330 Ideal I     SI2      61.2   56.   403  4.49  4.50  2.75
    ##  7 0.330 Ideal J     SI1      61.1   56.   403  4.49  4.55  2.76
    ##  8 0.230 Ideal G     VS1      61.9   54.   404  3.93  3.95  2.44
    ##  9 0.320 Ideal I     SI1      60.9   55.   404  4.45  4.48  2.72
    ## 10 0.300 Ideal I     SI2      61.0   59.   405  4.30  4.33  2.63
    ## # ... with 21,541 more rows

``` r
caratltwo <- diamonds %>%
          filter(carat <= 2)
caratltwo
```

    ## # A tibble: 52,051 x 10
    ##    carat cut       color clarity depth table price     x     y     z
    ##    <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1 0.230 Ideal     E     SI2      61.5   55.   326  3.95  3.98  2.43
    ##  2 0.210 Premium   E     SI1      59.8   61.   326  3.89  3.84  2.31
    ##  3 0.230 Good      E     VS1      56.9   65.   327  4.05  4.07  2.31
    ##  4 0.290 Premium   I     VS2      62.4   58.   334  4.20  4.23  2.63
    ##  5 0.310 Good      J     SI2      63.3   58.   335  4.34  4.35  2.75
    ##  6 0.240 Very Good J     VVS2     62.8   57.   336  3.94  3.96  2.48
    ##  7 0.240 Very Good I     VVS1     62.3   57.   336  3.95  3.98  2.47
    ##  8 0.260 Very Good H     SI1      61.9   55.   337  4.07  4.11  2.53
    ##  9 0.220 Fair      E     VS2      65.1   61.   337  3.87  3.78  2.49
    ## 10 0.230 Very Good H     VS1      59.4   61.   338  4.00  4.05  2.39
    ## # ... with 52,041 more rows

Summary Statistics
------------------

Calculating summary statistics is always a crucial part of EDA. The summarize command helps you do just that.

### Quantitative Variables

This code will allow you to find the mean, median, standard\_deviation, and IQR of a specific variable.

``` r
ss_price <- diamonds %>% 
          summarize(avg = mean(price), med = median(price), standard_dev = sd(price), 
                    iqr = IQR(price))
ss_price
```

    ## # A tibble: 1 x 4
    ##     avg   med standard_dev   iqr
    ##   <dbl> <dbl>        <dbl> <dbl>
    ## 1 3933. 2401.        3989. 4374.

### Categorical Variables

Here is an example that shows how to make a table showing the number of observations for each category in one categorical variable.

``` r
cut_count <- diamonds %>%
            group_by(cut) %>%
            summarize(count = n()) %>%
            arrange(desc(count))
cut_count
```

    ## # A tibble: 5 x 2
    ##   cut       count
    ##   <ord>     <int>
    ## 1 Ideal     21551
    ## 2 Premium   13791
    ## 3 Very Good 12082
    ## 4 Good       4906
    ## 5 Fair       1610

Now let's make a two-way table.

``` r
totals <- diamonds %>%
group_by(color, cut) %>%
summarize(num = n()) %>%
spread(color, num)
totals
```

    ## # A tibble: 5 x 8
    ##   cut           D     E     F     G     H     I     J
    ##   <ord>     <int> <int> <int> <int> <int> <int> <int>
    ## 1 Fair        163   224   312   314   303   175   119
    ## 2 Good        662   933   909   871   702   522   307
    ## 3 Very Good  1513  2400  2164  2299  1824  1204   678
    ## 4 Premium    1603  2337  2331  2924  2360  1428   808
    ## 5 Ideal      2834  3903  3826  4884  3115  2093   896

### Group by Categorical and Then Summarize A Quantitative Variable

``` r
ss_caratcut <- diamonds %>% 
          group_by(cut) %>%
          summarize(avg = mean(carat), med = median(carat), standard_dev = sd(carat), iqr = IQR(carat))
ss_caratcut
```

    ## # A tibble: 5 x 5
    ##   cut         avg   med standard_dev   iqr
    ##   <ord>     <dbl> <dbl>        <dbl> <dbl>
    ## 1 Fair      1.05  1.00         0.516 0.500
    ## 2 Good      0.849 0.820        0.454 0.510
    ## 3 Very Good 0.806 0.710        0.459 0.610
    ## 4 Premium   0.892 0.860        0.515 0.790
    ## 5 Ideal     0.703 0.540        0.433 0.660
