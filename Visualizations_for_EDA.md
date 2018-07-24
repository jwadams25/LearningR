Visualizations for EDA
================
John Adams

Visualizations with ggplot
==========================

An essential aspect of exploratory data analysis is visualizing your data. It allows you to draw conclusions more easily. This document will walk through a few visualizations you can create to see the story your data tells.

Load Tidyverse Library
----------------------

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

Histogram
---------

A histogram is a great way of seeing the distribution of quantitative data. Let's start by analyzing the distribution of the price of diamonds in the diamonds dataset.

``` r
price_hist <- diamonds %>%
            ggplot(aes(price)) +
            geom_histogram()
price_hist
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](Visualizations_for_EDA_files/figure-markdown_github/histogram%20distribution%20of%20prices-1.png)

Now let's add some labels to your axes and give the histogram a title. To start, I copied and pasted the code from above.

``` r
price_hist <- diamonds %>%
            ggplot(aes(price)) +
            geom_histogram() +
            labs(x = "Price", y = "Number of Diamonds", title = "Price of Diamonds")
price_hist
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](Visualizations_for_EDA_files/figure-markdown_github/histogram%20distribution%20of%20price%20with%20labels-1.png)

You'll see that a message popped up that says in part "Pick a better value with 'binwidth'" Let's make that change by adding in bindwith = 500. Reference data visualization principles to make an appropriate binwidth for your analysis.

``` r
price_hist <- diamonds %>%
            ggplot(aes(price)) +
            geom_histogram(binwidth = 500) +
            labs(x = "Price", y = "Number of Diamonds", title = "Price of Diamonds")
price_hist
```

![](Visualizations_for_EDA_files/figure-markdown_github/histogram%20distribution%20of%20prices%20new%20bins-1.png)

Now let's change the color and outline of the bars.

``` r
price_hist <- diamonds %>%
            ggplot(aes(price)) +
            geom_histogram(binwidth = 500, color = "white", fill = "blue") +
            labs(x = "Price", y = "Number of Diamonds", title = "Price of Diamonds")
price_hist
```

![](Visualizations_for_EDA_files/figure-markdown_github/histogram%20distribution%20of%20prices%20style%20changes-1.png)

To finish let's set the scales of the x and y axes and add in a line for the median (because it is skewed to the right)

``` r
price_hist <- diamonds %>%
            ggplot(aes(price)) +
            geom_histogram(binwidth = 500, color = "white", fill = "blue") +
            labs(x = "Price", y = "Number of Diamonds", title = "Price of Diamonds") +
            scale_x_continuous(limits = c(0, 20000),
                     breaks = seq(0, 20000, 2500))+
            scale_y_continuous(limits = c(0, 10000),
                     breaks = seq(0, 10000, 1000)) +
            geom_vline(aes(xintercept = median(price)))
           
price_hist
```

![](Visualizations_for_EDA_files/figure-markdown_github/histogram%20distribution%20of%20prices%20scales%20and%20median%20line-1.png)

Density Curve
-------------

There are times when a density curve will more clearly show the distribution of quantitative data. In this case, it would make sense because we have a large range of prices and a lot of bins. Let's say you are going through your EDA and create a histogram first and realize a density curve would be a better visualization, copy and paste your code, change the name, change geom\_histogram to geom\_density, take out the scales code, and change the y-axis label.

``` r
price_den <- diamonds %>%
            ggplot(aes(price)) +
            geom_density(color = "white", fill = "blue", alpha = 0.5) +
            labs(x = "Price", y = "Number of Diamonds", title = "Price of Diamonds") +
            geom_vline(aes(xintercept = median(price)))
price_den
```

![](Visualizations_for_EDA_files/figure-markdown_github/Density%20Curves%20distribution%20of%20prices-1.png)

Box Plots
---------

Box plots are great for comparing distributions. Let's use them to compare the prices of each color diamond.

``` r
pcol_boxes <- diamonds %>%
            ggplot(aes(x = color , y = price)) +
            geom_boxplot()
pcol_boxes
```

![](Visualizations_for_EDA_files/figure-markdown_github/Box%20Plots%20to%20Compare%20Distributions-1.png)

Before adding and style changes to this visualization, we want to make it easier for our reader to make conclusions. Therefore, we want to reorder these boxes from the greatest median to the least.

``` r
pcol_boxes <- diamonds %>%
            ggplot(aes(x = reorder(color, price, FUN = median) , y = price)) +
            geom_boxplot()
pcol_boxes
```

![](Visualizations_for_EDA_files/figure-markdown_github/Box%20Plots%20to%20Compare%20Distributions%20reorder-1.png)

Typically box plots are easier to read when they are stacked on top of one another. Use coord\_flip to do that. Let's also add in labels while we are here. You will do that using the labs code like you did when making the histograms and density curves.

``` r
pcol_boxes <- diamonds %>%
            ggplot(aes(x = reorder(color, price, FUN = median) , y = price)) +
            geom_boxplot() +
            labs(x = "Color", y = "Price", 
                 title = "Distribution of Diamond Prices by Color") +
            coord_flip()
pcol_boxes
```

![](Visualizations_for_EDA_files/figure-markdown_github/Box%20Plots%20to%20Compare%20Distributions%20add%20titles%20and%20flip-1.png)

Bar Plots
---------

We'll use the bar plot to visualize categorical data. In this first example, we will look at the marginal distribution of color. In other words, the heights of the bars will represent the number of diamonds that are each color.

``` r
colbar <- diamonds %>%
          ggplot(aes(x = color)) +
          geom_bar()
colbar
```

![](Visualizations_for_EDA_files/figure-markdown_github/Bar%20plot%20basic-1.png)

Again, we want these to be in order from greatest to least to tell the story in the most precise possible way.

Let's start by making a table using the skills we learned in previous activities to see the number of diamonds for each color. After, we will use that data frame to make our bar plot.

``` r
color_count <- diamonds %>%
            group_by(color) %>%
            summarize(count = n()) %>%
            arrange(desc(count))
```

    ## Warning: package 'bindrcpp' was built under R version 3.4.4

``` r
color_count
```

    ## # A tibble: 7 x 2
    ##   color count
    ##   <ord> <int>
    ## 1 G     11292
    ## 2 E      9797
    ## 3 F      9542
    ## 4 H      8304
    ## 5 D      6775
    ## 6 I      5422
    ## 7 J      2808

``` r
colbar <- color_count %>%
          ggplot(aes(x = reorder(color, count), y = count)) +
          geom_bar(stat = 'identity')
colbar
```

![](Visualizations_for_EDA_files/figure-markdown_github/Bar%20plot%20ordering%20bars-1.png)

100% Stacked Bar Graph
----------------------

When you want to examine conditional distributions, a good option is a 100% stacked bar graph. For this example, we want to look at the proportion of different cuts for each color diamond. i.e., You will be able to answer the question, "What proportion of diamonds that have a G color is an ideal cut?" It's important to remember that you use this when studying two categorical variables.

``` r
colcut <- diamonds %>%
            ggplot(aes(color, fill = cut)) +
            geom_bar(position = "fill") +
            labs(x = "Color of Diamond", y = "Proportion")
colcut
```

![](Visualizations_for_EDA_files/figure-markdown_github/100%20stacked-1.png)
