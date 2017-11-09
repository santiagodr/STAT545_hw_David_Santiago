Data Wrangling wrap up
================
Santiago David
2017-11-07

#### Load data and packages

``` r
suppressPackageStartupMessages(library(tidyverse))
suppressPackageStartupMessages(library(gapminder))
suppressPackageStartupMessages(library(singer))
suppressPackageStartupMessages(library(forcats))
suppressPackageStartupMessages(library(purrr))
suppressPackageStartupMessages(library(stringr))
data("gapminder")
```

Writing functions
=================

#### **Objective**:

Write one (or more) functions that do something useful to pieces of the Gapminder or Singer data. Make it something that’s not trivial to do with the simple dplyr verbs.

**Process**: The homework provide a good starting point with the gapminder data following Jenny's [tutorial](http://stat545.com/block012_function-regress-lifeexp-on-year.html) to create a linear regression of life expectancy on year. However, First things first, let's plot the data!

``` r
ggplot(gapminder, aes(year, lifeExp, colour = continent)) +
  geom_point() +
  facet_wrap(~ continent) +
  labs(title = "Life expectancy for each continent 1952-2007", x = "Years", y = "Life Expectancy (years)") +
  guides(colour = FALSE) +
  theme_classic() 
```

![](data_wrangling_hw06_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-2-1.png)

From this graph, we can see that there is an increasing trend of life expectancy over years, However, a priori, we dont really know if the relationship between Life expectancy and year is linear, or cuadratic, or whether a robust regression is more apropiate given the possibility that some values are outliers or influential observations. For example, what about that steep line of points in Europe?, or that odd red dot in Africa in the 90's?. For this reason, it is worth to create functions for different models that we can fit either to each continent, or country or other data.

The ideal function, will be one that takes a variable `life expectancty` and `year` as input, and return coefficients `intercept`, and `slope` and some measure of the , such as

``` r
summary(lm(lifeExp ~ I(year - 1952), gapminder))
```

    ## 
    ## Call:
    ## lm(formula = lifeExp ~ I(year - 1952), data = gapminder)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -39.949  -9.651   1.697  10.335  22.158 
    ## 
    ## Coefficients:
    ##                Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)    50.51208    0.53000   95.31   <2e-16 ***
    ## I(year - 1952)  0.32590    0.01632   19.96   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 11.63 on 1702 degrees of freedom
    ## Multiple R-squared:  0.1898, Adjusted R-squared:  0.1893 
    ## F-statistic: 398.6 on 1 and 1702 DF,  p-value: < 2.2e-16