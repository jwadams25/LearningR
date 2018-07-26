Zillow Exploratory Data Analysis KEY
================
John Adams

Zillow Exploratory Data Analysis Worksheet
==========================================

``` r
library(tidyverse)
```

    ## ── Attaching packages ────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 2.2.1     ✔ purrr   0.2.4
    ## ✔ tibble  1.4.2     ✔ dplyr   0.7.6
    ## ✔ tidyr   0.8.0     ✔ stringr 1.3.0
    ## ✔ readr   1.1.1     ✔ forcats 0.3.0

    ## ── Conflicts ───────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(skimr)
```

Load The Data
-------------

``` r
zillow <- read.csv("file:///Users/j.adams/projects/LearningR/housingdata.csv", stringsAsFactors=FALSE)
```

Exploring the Data
------------------

Use this code to help answer questions in the Exploring the Data section of your worksheet

``` r
head(zillow)
```

    ##   X  Price Lot.Size Waterfront Age Land.Value New.Construct Central.Air
    ## 1 1 132500     0.09         No  42      50000            No          No
    ## 2 2 181115     0.92         No   0      22300            No          No
    ## 3 3 109000     0.19         No 133       7300            No          No
    ## 4 4 155000     0.41         No  13      18700            No          No
    ## 5 5  86060     0.11         No   0      15000           Yes         Yes
    ## 6 6 120000     0.68         No  31      14000            No          No
    ##   Fuel.Type Heat.Type Sewer.Type Living.Area Pct.College Bedrooms
    ## 1  Electric  Electric    Private         906          35        2
    ## 2       Gas Hot Water    Private        1953          51        3
    ## 3       Gas Hot Water     Public        1944          51        4
    ## 4       Gas   Hot Air    Private        1944          51        3
    ## 5       Gas   Hot Air     Public         840          51        2
    ## 6       Gas   Hot Air    Private        1152          22        4
    ##   Fireplaces Bathrooms Rooms Fireplace       Beds
    ## 1          1       1.0     5       Yes 2 or fewer
    ## 2       None       2.5     6        No          3
    ## 3          1       1.0     8       Yes  4 or more
    ## 4          1       1.5     5       Yes          3
    ## 5       None       1.0     3        No 2 or fewer
    ## 6          1       1.0     8       Yes  4 or more

Visualize the Data
==================

To begin, let's explore some distributions of both price and living area. As you have learned, a historgram is a great option.

Create a histogram showing the distribution of Price
----------------------------------------------------

``` r
p_hist <- zillow %>%
  ggplot(aes(Price)) +
  geom_histogram(binwidth = 50000, fill = "navy", color = "black") +
  labs(title = "Distribution of House Prices", x = "Price", y = "Number of Houses") +
  scale_x_continuous(limits = c(0, 800000),
                   breaks = seq(0, 800000, 50000))
p_hist
```

![](KEY_-_Zillow_Exploratory_Data_Analysis_files/figure-markdown_github/Price%20histogram-1.png)

Create a histogram showing the distribution of Living Area
----------------------------------------------------------

``` r
la_hist <- zillow %>%
          ggplot(aes(Living.Area)) +
  geom_histogram(binwidth = 200, fill = "red", color = "black") +
  labs(title =" Distribution of Living Areas", x = "Living Area (sq-ft)", 
       y = "Number of Houses") +
  scale_x_continuous(limits = c(0, 5600),
                     breaks = seq(0, 5600, 200)) +
  scale_y_continuous(limits = c(0, 300),
                     breaks = seq(0, 300, 50))

la_hist
```

![](KEY_-_Zillow_Exploratory_Data_Analysis_files/figure-markdown_github/Living%20Area%20histogram-1.png)

Summary statistics for histograms
=================================

Let's now look at the summary statistics for the quantitative variables, Price and living area, displayed in the histograms.

Summary Statistics for Price
----------------------------

``` r
ssprice <- zillow %>% 
          summarize(avg = mean(Price), med = median(Price), standard_dev = sd(Price), 
                    iqr = IQR(Price))
ssprice
```

    ##        avg    med standard_dev    iqr
    ## 1 211966.7 189900     98441.39 114000

Summary Statistics for Living Area
----------------------------------

``` r
ssarea <- zillow %>% 
          summarize(avg = mean(Living.Area), med = median(Living.Area), 
                    standard_dev = sd(Living.Area), 
                    iqr = IQR(Living.Area))
ssarea
```

    ##        avg    med standard_dev    iqr
    ## 1 1754.976 1634.5     619.9356 837.75

Summary Statistics for Price and Living Area
--------------------------------------------

Here is another way to calculate summary statistics for multiple variables at the same time.

``` r
ss_area_price <- zillow %>%
                  select(Price, Living.Area) %>%
                  skim()
ss_area_price
```

    ## Skim summary statistics
    ##  n obs: 1728 
    ##  n variables: 2 
    ## 
    ## ── Variable type:integer ──────────────────────────────────────────────────
    ##     variable missing complete    n      mean       sd   p0    p25      p50
    ##  Living.Area       0     1728 1728   1754.98   619.94  616   1300   1634.5
    ##        Price       0     1728 1728 211966.71 98441.39 5000 145000 189900  
    ##        p75   p100     hist
    ##    2137.75   5228 ▃▇▅▂▁▁▁▁
    ##  259000    775000 ▁▇▅▂▁▁▁▁

Relationship Between Price and Living Area
==========================================

Create a Scatter Plot
---------------------

Now let's see if there is an association between the price and living area. Create a scatter plot of Price vs Living Area.

``` r
pvsa <- zillow %>%
          ggplot(aes(x= Living.Area, y = Price)) +
          geom_point(alpha = 0.25) +
          stat_smooth(method = 'lm') +
          labs(title ="Price vs. Living Area", 
               x = "Living Area (sq. ft)", y = "Price") +
          scale_y_continuous(limits = c(0, 800000),
                     breaks = seq(0, 800000, 250000))
pvsa
```

![](KEY_-_Zillow_Exploratory_Data_Analysis_files/figure-markdown_github/scatter%20plot-1.png)

Creating the Model
------------------

Let's now build a linear model to gain more insight into the relationship between Prices and Living Area.

``` r
zillowmodel <- lm(Price ~ Living.Area, zillow)
zillowmodel
```

    ## 
    ## Call:
    ## lm(formula = Price ~ Living.Area, data = zillow)
    ## 
    ## Coefficients:
    ## (Intercept)  Living.Area  
    ##     13439.4        113.1

``` r
cor(zillow$Price, zillow$Living.Area)
```

    ## [1] 0.7123902

Conditional Distributions
=========================

Houses can use different types of heat. Let's see if there is different between the types of heat in newly constructed houses and not new constructed houses.

``` r
heat_construction <-  zillow %>%
                    ggplot(aes(x = New.Construct, fill = Heat.Type)) +
                    geom_bar(position = "fill")
heat_construction
```

![](KEY_-_Zillow_Exploratory_Data_Analysis_files/figure-markdown_github/stacked%20bar%20and%20two%20way%20table-1.png)

Create a Two-Way Table
----------------------

``` r
heattotals <- zillow %>%
            group_by(New.Construct, Heat.Type) %>%
            summarize(num = n()) %>%
            spread(New.Construct, num)
heattotals
```

    ## # A tibble: 3 x 3
    ##   Heat.Type    No   Yes
    ##   <chr>     <int> <int>
    ## 1 Electric    304     1
    ## 2 Hot Air    1042    79
    ## 3 Hot Water   301     1
