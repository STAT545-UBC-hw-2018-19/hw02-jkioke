hw02-gapminder
================

``` r
library(gapminder)
library(tidyverse)
```

    ## -- Attaching packages -------------------------------------------- tidyverse 1.2.1 --

    ## v ggplot2 3.0.0     v purrr   0.2.5
    ## v tibble  1.4.2     v dplyr   0.7.6
    ## v tidyr   0.8.1     v stringr 1.3.0
    ## v readr   1.1.1     v forcats 0.3.0

    ## -- Conflicts ----------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

What is Gapminder, anyway?
--------------------------

``` r
class(gapminder)
```

    ## [1] "tbl_df"     "tbl"        "data.frame"

Gapminder is a data frame, so it should have numerous variables arranged in unique columns that each have the same number of rows.

``` r
summary(gapminder)
```

    ##         country        continent        year         lifeExp     
    ##  Afghanistan:  12   Africa  :624   Min.   :1952   Min.   :23.60  
    ##  Albania    :  12   Americas:300   1st Qu.:1966   1st Qu.:48.20  
    ##  Algeria    :  12   Asia    :396   Median :1980   Median :60.71  
    ##  Angola     :  12   Europe  :360   Mean   :1980   Mean   :59.47  
    ##  Argentina  :  12   Oceania : 24   3rd Qu.:1993   3rd Qu.:70.85  
    ##  Australia  :  12                  Max.   :2007   Max.   :82.60  
    ##  (Other)    :1632                                                
    ##       pop              gdpPercap       
    ##  Min.   :6.001e+04   Min.   :   241.2  
    ##  1st Qu.:2.794e+06   1st Qu.:  1202.1  
    ##  Median :7.024e+06   Median :  3531.8  
    ##  Mean   :2.960e+07   Mean   :  7215.3  
    ##  3rd Qu.:1.959e+07   3rd Qu.:  9325.5  
    ##  Max.   :1.319e+09   Max.   :113523.1  
    ## 

``` r
nrow(gapminder)
```

    ## [1] 1704

The **6** columns in the Gapminder data frame are

-   Country (country)
-   Continent (continent)
-   Year (year)
-   Life Expectancy (lifeExp)
-   Population (pop)
-   GDP per Capita (gdpPercap)

There are **1704** observations for each variable, and so 1704 rows.Each variable is a list of either categorical (country, continent) or quantitative (pop, lifeExp) values.

Exploring Gapminder
-------------------

#### Categorical

Looking at the Continent column, we can see there are only a few unique values this variable can take:

``` r
cont <- select(gapminder, continent)
unique(cont)
```

    ## # A tibble: 5 x 1
    ##   continent
    ##   <fct>    
    ## 1 Asia     
    ## 2 Europe   
    ## 3 Africa   
    ## 4 Americas 
    ## 5 Oceania

The histogram below provides a summary of the distribution of values throughout the "continent" column.

``` r
ggplot(gapminder, aes(continent)) +
  stat_count()
```

![](hw02-gapminder_files/figure-markdown_github/unnamed-chunk-5-1.png)

#### Quantitative

GDP per Capita is fairly different from the Continent variable, as it can assume any number within a certain range. In the table below we can see the minimum value of GDP per capita is $241.20, the maximum is $113,523.10. The mean GDP per Capita is $7215.30, which is much closer to the minimum than the maximum (75% of the data is below $9325.5).

``` r
select(gapminder, gdpPercap) %>% 
  summary()
```

    ##    gdpPercap       
    ##  Min.   :   241.2  
    ##  1st Qu.:  1202.1  
    ##  Median :  3531.8  
    ##  Mean   :  7215.3  
    ##  3rd Qu.:  9325.5  
    ##  Max.   :113523.1

The density plot below visualizes the distribution of GDP per Capita.

``` r
ggplot(gapminder, aes(gdpPercap)) +
  geom_density()
```

![](hw02-gapminder_files/figure-markdown_github/unnamed-chunk-7-1.png)

#### Filter

Would how would the plot change if we only look at observations from Europe?

``` r
gapminder %>% 
  filter(continent == "Europe") %>% 
  ggplot(aes(gdpPercap)) +
  geom_density()
```

![](hw02-gapminder_files/figure-markdown_github/unnamed-chunk-8-1.png)

#### What might the GDP per Capita look like for different continents?

``` r
ggplot(gapminder, aes(gdpPercap)) +
  geom_histogram() +
  facet_wrap( ~ continent)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](hw02-gapminder_files/figure-markdown_github/unnamed-chunk-9-1.png)

Scatterplot
-----------

Please enjoy this miscellaneous scatterplot of Life Expectancy vs. Population!

``` r
ggplot(gapminder, aes(lifeExp, pop)) +
  geom_point()
```

![](hw02-gapminder_files/figure-markdown_github/unnamed-chunk-10-1.png)

Doing More
----------

The following is a valid way to isolate the data pertaining to Afghanistan and Rwanda.

``` r
filter(gapminder, country == c("Rwanda", "Afghanistan"))
```

    ## # A tibble: 12 x 6
    ##    country     continent  year lifeExp      pop gdpPercap
    ##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
    ##  1 Afghanistan Asia       1957    30.3  9240934      821.
    ##  2 Afghanistan Asia       1967    34.0 11537966      836.
    ##  3 Afghanistan Asia       1977    38.4 14880372      786.
    ##  4 Afghanistan Asia       1987    40.8 13867957      852.
    ##  5 Afghanistan Asia       1997    41.8 22227415      635.
    ##  6 Afghanistan Asia       2007    43.8 31889923      975.
    ##  7 Rwanda      Africa     1952    40.0  2534927      493.
    ##  8 Rwanda      Africa     1962    43.0  3051242      597.
    ##  9 Rwanda      Africa     1972    44.6  3992121      591.
    ## 10 Rwanda      Africa     1982    46.2  5507565      882.
    ## 11 Rwanda      Africa     1992    23.6  7290203      737.
    ## 12 Rwanda      Africa     2002    43.4  7852401      786.

You could then pipe the Rwanda/Afghanistan data into a plot, though there are only 12 points.

``` r
filter(gapminder, country == c("Rwanda", "Afghanistan")) %>% 
  ggplot(aes(lifeExp, gdpPercap)) + 
  geom_point()
```

![](hw02-gapminder_files/figure-markdown_github/unnamed-chunk-12-1.png)
