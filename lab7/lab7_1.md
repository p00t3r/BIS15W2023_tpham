---
title: "Midterm 1 Review and `across()` "
date: "2023-02-02"
output:
  html_document: 
    theme: spacelab
    toc: yes
    toc_float: yes
    keep_md: yes
  pdf_document:
    toc: yes
---

## Learning Goals
*At the end of this exercise, you will be able to:*    
1. Work through midterm 1 and practice some new approaches to solving common problems in R.    
2. Use the `across()` operator to produce summaries across multiple variables. 

## Load the libraries

```r
library("tidyverse")
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
## ✔ ggplot2 3.4.0      ✔ purrr   1.0.0 
## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
## ✔ tidyr   1.2.1      ✔ stringr 1.5.0 
## ✔ readr   2.1.3      ✔ forcats 0.5.2 
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
library("skimr")
library("janitor")
```

```
## 
## Attaching package: 'janitor'
## 
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

```r
library("palmerpenguins")
```

## Midterm 1 Review
Let's briefly review the questions from midterm 1 so you can get an idea of how I was thinking about the problems. Remember, there is more than one way to get at these answers, so don't worry if yours looks different than mine!  

## Penguins from lab 6, part 2
We didn't get through the second part of lab 6 last week, so let's do a quick review of some key functions. As you know, summarize() and group_by() are powerful tools that we can use to produce clean summaries of data. Especially when used together, we can quickly group variables of interest and save time. Let's do some practice with the [palmerpenguins(https://allisonhorst.github.io/palmerpenguins/articles/intro.html) data to refresh our memory.


```r
glimpse(penguins)
```

```
## Rows: 344
## Columns: 8
## $ species           <fct> Adelie, Adelie, Adelie, Adelie, Adelie, Adelie, Adel…
## $ island            <fct> Torgersen, Torgersen, Torgersen, Torgersen, Torgerse…
## $ bill_length_mm    <dbl> 39.1, 39.5, 40.3, NA, 36.7, 39.3, 38.9, 39.2, 34.1, …
## $ bill_depth_mm     <dbl> 18.7, 17.4, 18.0, NA, 19.3, 20.6, 17.8, 19.6, 18.1, …
## $ flipper_length_mm <int> 181, 186, 195, NA, 193, 190, 181, 195, 193, 190, 186…
## $ body_mass_g       <int> 3750, 3800, 3250, NA, 3450, 3650, 3625, 4675, 3475, …
## $ sex               <fct> male, female, female, NA, female, male, female, male…
## $ year              <int> 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007…
```

Recall that group_by() and summarize() work great together. Let's say we were interested in how body mass varied by island. It is reasonable to assume that the islands are different, so maybe the penguins are as well.

```r
penguins %>% 
  group_by(island) %>% 
  summarize(mean_body_mass_g=mean(body_mass_g, na.rm=T)) # remember to remove those NA's!
```

```
## # A tibble: 3 × 2
##   island    mean_body_mass_g
##   <fct>                <dbl>
## 1 Biscoe               4716.
## 2 Dream                3713.
## 3 Torgersen            3706.
```

What if we are interested in the number of observations (penguins) by species and island?

```r
penguins %>% 
  group_by(island, species) %>% 
  summarize(n_penguins=n(), .groups = 'keep')
```

```
## # A tibble: 5 × 3
## # Groups:   island, species [5]
##   island    species   n_penguins
##   <fct>     <fct>          <int>
## 1 Biscoe    Adelie            44
## 2 Biscoe    Gentoo           124
## 3 Dream     Adelie            56
## 4 Dream     Chinstrap         68
## 5 Torgersen Adelie            52
```

Recall from lab 6, part 2 that `count()` works like a combination of `group_by()` and `summarize()` but just shows the number of observations.

```r
penguins %>% 
  count(island, species)     #short data format
```

```
## # A tibble: 5 × 3
##   island    species       n
##   <fct>     <fct>     <int>
## 1 Biscoe    Adelie       44
## 2 Biscoe    Gentoo      124
## 3 Dream     Adelie       56
## 4 Dream     Chinstrap    68
## 5 Torgersen Adelie       52
```

As I showed in lab 6, part 2 `tabyl()` will also produce counts, but the output is different. It is just a matter of personal preference.

```r
penguins %>% 
  tabyl(island, species)   #wide data format. species are considered variable names here (instead of being observations)
```

```
##     island Adelie Chinstrap Gentoo
##     Biscoe     44         0    124
##      Dream     56        68      0
##  Torgersen     52         0      0
```

## Practice
1. Produce a summary of the mean for bill_length_mm, bill_depth_mm, flipper_length_mm, and body_mass_g within Adelie penguins only. Be sure to provide the number of samples.

```r
penguins %>% 
  filter(species == "Adelie") %>% 
  summarize(mean_bill_length_mm = mean(bill_length_mm, na.rm=T),
            mean_bill_depth_mm = mean(bill_depth_mm, na.rm=T),
            mean_flipper_length_mm = mean(flipper_length_mm, na.rm=T),
            mean_body_mass_g = mean(body_mass_g, na.rm=T),
            n_penguins=n())
```

```
## # A tibble: 1 × 5
##   mean_bill_length_mm mean_bill_depth_mm mean_flipper_length_mm mean_b…¹ n_pen…²
##                 <dbl>              <dbl>                  <dbl>    <dbl>   <int>
## 1                38.8               18.3                   190.    3701.     152
## # … with abbreviated variable names ¹​mean_body_mass_g, ²​n_penguins
```

2. How does the mean of `bill_length_mm` compare between penguin species?

```r
penguins %>% 
  group_by(species) %>% 
  summarize(mean_bill_length_mm = mean(bill_length_mm, na.rm=T))
```

```
## # A tibble: 3 × 2
##   species   mean_bill_length_mm
##   <fct>                   <dbl>
## 1 Adelie                   38.8
## 2 Chinstrap                48.8
## 3 Gentoo                   47.5
```


3. For some penguins, their sex is listed as NA. Where do these penguins occur?

```r
penguins %>% 
  filter(is.na(sex))
```

```
## # A tibble: 11 × 8
##    species island    bill_length_mm bill_depth_mm flipper_…¹ body_…² sex    year
##    <fct>   <fct>              <dbl>         <dbl>      <int>   <int> <fct> <int>
##  1 Adelie  Torgersen           NA            NA           NA      NA <NA>   2007
##  2 Adelie  Torgersen           34.1          18.1        193    3475 <NA>   2007
##  3 Adelie  Torgersen           42            20.2        190    4250 <NA>   2007
##  4 Adelie  Torgersen           37.8          17.1        186    3300 <NA>   2007
##  5 Adelie  Torgersen           37.8          17.3        180    3700 <NA>   2007
##  6 Adelie  Dream               37.5          18.9        179    2975 <NA>   2007
##  7 Gentoo  Biscoe              44.5          14.3        216    4100 <NA>   2007
##  8 Gentoo  Biscoe              46.2          14.4        214    4650 <NA>   2008
##  9 Gentoo  Biscoe              47.3          13.8        216    4725 <NA>   2009
## 10 Gentoo  Biscoe              44.5          15.7        217    4875 <NA>   2009
## 11 Gentoo  Biscoe              NA            NA           NA      NA <NA>   2009
## # … with abbreviated variable names ¹​flipper_length_mm, ²​body_mass_g
```



## `across()`
Last time we had some great questions on how to use `filter()` and `select()` across multiple variables. There is a function in dplyr called `across()` which is designed to help make this efficient. Have a look at Rebecca Barter's awesome blog [here](http://www.rebeccabarter.com/blog/2020-07-09-across/) as I am following her below.  

What if we wanted to use `summarize()` to produce distinct counts over multiple variables; i.e. species, island, and sex? Although this isn't a lot of coding you can image that with a lot of variables it would be cumbersome.

```r
penguins %>%
  summarize(distinct_species = n_distinct(species),
            distinct_island = n_distinct(island),
            distinct_sex = n_distinct(sex))
```

```
## # A tibble: 1 × 3
##   distinct_species distinct_island distinct_sex
##              <int>           <int>        <int>
## 1                3               3            3
```

By using `across()` we can reduce the clutter and make things cleaner. 

```r
penguins %>%
  summarize(across(c(species, island, sex), n_distinct))
```

```
## # A tibble: 1 × 3
##   species island   sex
##     <int>  <int> <int>
## 1       3      3     3
```

This is very helpful for continuous variables.

```r
penguins %>%
  summarize(across(contains("mm"), mean, na.rm=T))
```

```
## # A tibble: 1 × 3
##   bill_length_mm bill_depth_mm flipper_length_mm
##            <dbl>         <dbl>             <dbl>
## 1           43.9          17.2              201.
```

`group_by` also works.

```r
penguins %>%
  group_by(sex) %>% 
  summarize(across(contains("mm"), mean, na.rm=T))
```

```
## # A tibble: 3 × 4
##   sex    bill_length_mm bill_depth_mm flipper_length_mm
##   <fct>           <dbl>         <dbl>             <dbl>
## 1 female           42.1          16.4              197.
## 2 male             45.9          17.9              205.
## 3 <NA>             41.3          16.6              199
```

Here we summarize across all variables.

```r
penguins %>%
  summarise_all(n_distinct)
```

```
## # A tibble: 1 × 8
##   species island bill_length_mm bill_depth_mm flipper_leng…¹ body_…²   sex  year
##     <int>  <int>          <int>         <int>          <int>   <int> <int> <int>
## 1       3      3            165            81             56      95     3     3
## # … with abbreviated variable names ¹​flipper_length_mm, ²​body_mass_g
```

Operators can also work, here I am summarizing `n_distinct()` across all variables except `species`, `island`, and `sex`.

```r
penguins %>%
  summarize(across(!c(species, island, sex), 
                   n_distinct))
```

```
## # A tibble: 1 × 5
##   bill_length_mm bill_depth_mm flipper_length_mm body_mass_g  year
##            <int>         <int>             <int>       <int> <int>
## 1            165            81                56          95     3
```

All variables that include "bill"...all of the other dplyr operators also work.

```r
penguins %>%
  summarise(across(starts_with("bill"), n_distinct))
```

```
## # A tibble: 1 × 2
##   bill_length_mm bill_depth_mm
##            <int>         <int>
## 1            165            81
```

## Practice
1. Produce separate summaries of the mean and standard deviation for bill_length_mm, bill_depth_mm, and flipper_length_mm for each penguin species. Be sure to provide the number of samples.  

```r
penguins %>% 
  group_by(species) %>% 
  summarize(across(contains("mm"), mean, na.rm=T), n_penguins=n()) 
```

```
## # A tibble: 3 × 5
##   species   bill_length_mm bill_depth_mm flipper_length_mm n_penguins
##   <fct>              <dbl>         <dbl>             <dbl>      <int>
## 1 Adelie              38.8          18.3              190.        152
## 2 Chinstrap           48.8          18.4              196.         68
## 3 Gentoo              47.5          15.0              217.        124
```


```r
penguins %>% 
  group_by(species) %>% 
  summarize(across(contains("mm"), sd , na.rm=T), n_penguins=n())
```

```
## # A tibble: 3 × 5
##   species   bill_length_mm bill_depth_mm flipper_length_mm n_penguins
##   <fct>              <dbl>         <dbl>             <dbl>      <int>
## 1 Adelie              2.66         1.22               6.54        152
## 2 Chinstrap           3.34         1.14               7.13         68
## 3 Gentoo              3.08         0.981              6.48        124
```

## That's it! Let's take a break and then move on to part 2! 

-->[Home](https://jmledford3115.github.io/datascibiol/)  
