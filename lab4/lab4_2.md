---
title: "Transforming data 2: `filter()`"
date: "2023-01-23"
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
1. Use the functions of dplyr (filter, select, arrange) to organize and sort data frames.  
2. Use `mutate()` to calculate a new column from existing columns.  

## Review  
In the previous lab, we used `select()` to extract columns of interest from a data frame. This helps us focus our attention on the variables relevant to our question. However, it doesn't allow us to extract observations from within the data frame. The `filter()` function allows us to extract data that meet specific criteria. When combined with `select()`, we have the power to transform, shape, and explore data with the potential to make new discoveries.  

## Load the tidyverse

```r
library("tidyverse")
```

## Load the data
For these exercises we will use the data from lab 4_1.  

```r
fish <- readr::read_csv("data/Gaeta_etal_CLC_data.csv")
```

```
## Rows: 4033 Columns: 6
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (2): lakeid, annnumber
## dbl (4): fish_id, length, radii_length_mm, scalelength
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


```r
mammals <- readr::read_csv("data/mammal_lifehistories_v2.csv")
```

```
## Rows: 1440 Columns: 13
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): order, family, Genus, species
## dbl (9): mass, gestation, newborn, weaning, wean mass, AFR, max. life, litte...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

To keep things tidy, I am going to rename the variables in the mammals data.  

```r
mammals <- rename(mammals, genus=Genus, wean_mass="wean mass", max_life="max. life", litter_size="litter size", litters_per_year="litters/year")
```

## `filter()`
Unlike `select()`, `filter()` allows us to extract data that meet specific criteria within a variable. Let's say that we are interested only in the fish that occur in lake "AL". We can use `filter()` to extract these observations.  

```r
filter(fish, lakeid == "AL")
```

```
## # A tibble: 383 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         299 1            167            1.31        2.70
##  4 AL         300 EDGE         175            3.02        3.02
##  5 AL         300 3            175            2.67        3.02
##  6 AL         300 2            175            2.14        3.02
##  7 AL         300 1            175            1.23        3.02
##  8 AL         301 EDGE         194            3.34        3.34
##  9 AL         301 3            194            2.97        3.34
## 10 AL         301 2            194            2.29        3.34
## # … with 373 more rows
```

Similarly, if we are only interested in fish with a length greater than or equal to 350 we can use `filter()` to extract these observations.  

```r
filter(fish, length >= 350)
```

```
## # A tibble: 890 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         306 EDGE         350            6.94        6.94
##  2 AL         306 10           350            6.46        6.94
##  3 AL         306 9            350            6.16        6.94
##  4 AL         306 8            350            5.88        6.94
##  5 AL         306 7            350            5.42        6.94
##  6 AL         306 6            350            4.90        6.94
##  7 AL         306 5            350            4.46        6.94
##  8 AL         306 4            350            3.75        6.94
##  9 AL         306 3            350            2.93        6.94
## 10 AL         306 2            350            2.14        6.94
## # … with 880 more rows
```

+ `filter()` allows all of the expected operators; i.e. >, >=, <, <=, != (not equal), and == (equal).  

Using the `!` operator allows for the exclusion of specific observations.

```r
filter(fish, lakeid != "AL")
```

```
## # A tibble: 3,650 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AR         269 EDGE         140            2.01        2.01
##  2 AR         269 1            140            1.48        2.01
##  3 AR         270 EDGE         193            2.66        2.66
##  4 AR         270 3            193            2.39        2.66
##  5 AR         270 2            193            2.03        2.66
##  6 AR         270 1            193            1.42        2.66
##  7 AR         271 EDGE         220            3.50        3.50
##  8 AR         271 5            220            3.13        3.50
##  9 AR         271 4            220            2.86        3.50
## 10 AR         271 3            220            2.63        3.50
## # … with 3,640 more rows
```

## Using `filter()` with multiple observations  
Filtering multiple values within the same variable requires the `%in%` [operator](https://www.tutorialspoint.com/r/r_operators.htm).    

```r
filter(fish, length %in% c(167, 175))
```

```
## # A tibble: 18 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167           2.70         2.70
##  2 AL         299 2            167           2.04         2.70
##  3 AL         299 1            167           1.31         2.70
##  4 AL         300 EDGE         175           3.02         3.02
##  5 AL         300 3            175           2.67         3.02
##  6 AL         300 2            175           2.14         3.02
##  7 AL         300 1            175           1.23         3.02
##  8 BO         397 EDGE         175           2.67         2.67
##  9 BO         397 3            175           2.39         2.67
## 10 BO         397 2            175           1.59         2.67
## 11 BO         397 1            175           0.830        2.67
## 12 LSG         45 EDGE         175           3.21         3.21
## 13 LSG         45 3            175           2.92         3.21
## 14 LSG         45 2            175           2.44         3.21
## 15 LSG         45 1            175           1.60         3.21
## 16 RD         103 EDGE         167           2.80         2.80
## 17 RD         103 2            167           2.10         2.80
## 18 RD         103 1            167           1.31         2.80
```

Alternatively, you can use `between` if you are looking for a range of specific values.  

```r
filter(fish, between(scalelength, 2.5, 2.55))
```

```
## # A tibble: 10 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 LSG         56 EDGE         145            2.55        2.55
##  2 LSG         56 2            145            1.94        2.55
##  3 LSG         56 1            145            1.20        2.55
##  4 LSG         57 EDGE         143            2.52        2.52
##  5 LSG         57 2            143            2.13        2.52
##  6 LSG         57 1            143            1.19        2.52
##  7 UB          80 EDGE         153            2.55        2.55
##  8 UB          80 3            153            2.10        2.55
##  9 UB          80 2            153            1.62        2.55
## 10 UB          80 1            153            1.14        2.55
```

You can also extract observations "near" a certain value but you need to specify a tolerance.  

```r
filter(fish, near(radii_length_mm, 2, tol = 0.2))
```

```
## # A tibble: 291 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 2            167            2.04        2.70
##  2 AL         300 2            175            2.14        3.02
##  3 AL         302 2            324            2.19        6.07
##  4 AL         303 2            325            2.04        6.79
##  5 AL         306 2            350            2.14        6.94
##  6 AL         308 2            355            1.86        6.67
##  7 AL         312 2            367            2.17        6.81
##  8 AL         313 2            367            2.06        6.47
##  9 AL         315 2            372            2.04        6.47
## 10 AL         316 2            372            1.82        6.35
## # … with 281 more rows
```

## Practice
1. Filter the `fish` data to include the samples from lake "BO".

```r
filter(fish, lakeid=="BO")
```

```
## # A tibble: 197 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 BO         389 EDGE         104           1.50         1.50
##  2 BO         389 1            104           0.736        1.50
##  3 BO         390 EDGE         105           1.59         1.59
##  4 BO         390 1            105           0.698        1.59
##  5 BO         391 EDGE         107           1.43         1.43
##  6 BO         391 1            107           0.695        1.43
##  7 BO         392 EDGE         124           2.11         2.11
##  8 BO         392 2            124           1.36         2.11
##  9 BO         392 1            124           0.792        2.11
## 10 BO         393 EDGE         141           2.16         2.16
## # … with 187 more rows
```

2. Filter the data to include all lakes except "AR".

```r
filter(fish, lakeid!="AR")
```

```
## # A tibble: 3,771 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         299 1            167            1.31        2.70
##  4 AL         300 EDGE         175            3.02        3.02
##  5 AL         300 3            175            2.67        3.02
##  6 AL         300 2            175            2.14        3.02
##  7 AL         300 1            175            1.23        3.02
##  8 AL         301 EDGE         194            3.34        3.34
##  9 AL         301 3            194            2.97        3.34
## 10 AL         301 2            194            2.29        3.34
## # … with 3,761 more rows
```

3. Filter the fish data to include all fish with a scalelength within 0.25 of 8.

```r
filter(fish, near(radii_length_mm, 8, tol = 0.25))
```

```
## # A tibble: 34 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         309 EDGE         355            7.89        7.89
##  2 AL         318 EDGE         373            7.97        7.97
##  3 AL         321 EDGE         394            8.12        8.12
##  4 AL         321 13           394            7.76        8.12
##  5 AL         323 EDGE         395            7.80        7.80
##  6 AL         324 EDGE         406            8.21        8.21
##  7 AL         324 14           406            7.91        8.21
##  8 AL         326 EDGE         413            7.76        7.76
##  9 AL         327 15           413            8.11        8.33
## 10 AL         327 14           413            7.81        8.33
## # … with 24 more rows
```

4. Filter the fish data to include fish with a scalelength between 2 and 4.

```r
filter(fish,between(scalelength, 2, 4))
```

```
## # A tibble: 723 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         299 1            167            1.31        2.70
##  4 AL         300 EDGE         175            3.02        3.02
##  5 AL         300 3            175            2.67        3.02
##  6 AL         300 2            175            2.14        3.02
##  7 AL         300 1            175            1.23        3.02
##  8 AL         301 EDGE         194            3.34        3.34
##  9 AL         301 3            194            2.97        3.34
## 10 AL         301 2            194            2.29        3.34
## # … with 713 more rows
```

## Using `filter()` on multiple conditions
You can also use `filter()` to extract data based on multiple conditions. Below we extract only the fish that have lakeid "AL" and length >350.

```r
filter(fish, lakeid == "AL" & length > 350) #and
```

```
## # A tibble: 314 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         307 EDGE         353            7.55        7.55
##  2 AL         307 13           353            7.28        7.55
##  3 AL         307 12           353            6.98        7.55
##  4 AL         307 11           353            6.73        7.55
##  5 AL         307 10           353            6.48        7.55
##  6 AL         307 9            353            6.22        7.55
##  7 AL         307 8            353            5.92        7.55
##  8 AL         307 7            353            5.44        7.55
##  9 AL         307 6            353            5.06        7.55
## 10 AL         307 5            353            4.37        7.55
## # … with 304 more rows
```

Notice that the `|` operator generates a different result.

```r
filter(fish, lakeid == "AL" | length > 350) #or
```

```
## # A tibble: 948 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         299 1            167            1.31        2.70
##  4 AL         300 EDGE         175            3.02        3.02
##  5 AL         300 3            175            2.67        3.02
##  6 AL         300 2            175            2.14        3.02
##  7 AL         300 1            175            1.23        3.02
##  8 AL         301 EDGE         194            3.34        3.34
##  9 AL         301 3            194            2.97        3.34
## 10 AL         301 2            194            2.29        3.34
## # … with 938 more rows
```

Rules:  
+ `filter(condition1, condition2)` will return rows where both conditions are met.  
+ `filter(condition1, !condition2)` will return all rows where condition one is true but condition 2 is not.  
+ `filter(condition1 | condition2)` will return rows where condition 1 or condition 2 is met.  
+ `filter(xor(condition1, condition2)` will return all rows where only one of the conditions is met, and not when both conditions are met.  

In this case, we filter out the fish with a length over 400 and a scale length over 11 or a radii length over 8.

```r
filter(fish, length > 400, (scalelength > 11 | radii_length_mm > 8))
```

```
## # A tibble: 23 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         324 EDGE         406            8.21        8.21
##  2 AL         327 EDGE         413            8.33        8.33
##  3 AL         327 15           413            8.11        8.33
##  4 AL         328 EDGE         420            8.71        8.71
##  5 AL         328 16           420            8.41        8.71
##  6 AL         328 15           420            8.14        8.71
##  7 WS         180 EDGE         403           11.0        11.0 
##  8 WS         180 16           403           10.6        11.0 
##  9 WS         180 15           403           10.3        11.0 
## 10 WS         180 14           403            9.93       11.0 
## # … with 13 more rows
```

## Practice  
1. Have a look at the mammals data using the summary functions of your choosing.    

```r
glimpse(mammals)
```

```
## Rows: 1,440
## Columns: 13
## $ order            <chr> "Artiodactyla", "Artiodactyla", "Artiodactyla", "Arti…
## $ family           <chr> "Antilocapridae", "Bovidae", "Bovidae", "Bovidae", "B…
## $ genus            <chr> "Antilocapra", "Addax", "Aepyceros", "Alcelaphus", "A…
## $ species          <chr> "americana", "nasomaculatus", "melampus", "buselaphus…
## $ mass             <dbl> 45375.0, 182375.0, 41480.0, 150000.0, 28500.0, 55500.…
## $ gestation        <dbl> 8.13, 9.39, 6.35, 7.90, 6.80, 5.08, 5.72, 5.50, 8.93,…
## $ newborn          <dbl> 3246.36, 5480.00, 5093.00, 10166.67, -999.00, 3810.00…
## $ weaning          <dbl> 3.00, 6.50, 5.63, 6.50, -999.00, 4.00, 4.04, 2.13, 10…
## $ wean_mass        <dbl> 8900, -999, 15900, -999, -999, -999, -999, -999, 1575…
## $ AFR              <dbl> 13.53, 27.27, 16.66, 23.02, -999.00, 14.89, 10.23, 20…
## $ max_life         <dbl> 142, 308, 213, 240, -999, 251, 228, 255, 300, 324, 30…
## $ litter_size      <dbl> 1.85, 1.00, 1.00, 1.00, 1.00, 1.37, 1.00, 1.00, 1.00,…
## $ litters_per_year <dbl> 1.00, 0.99, 0.95, -999.00, -999.00, 2.00, -999.00, 1.…
```

2. What are the names of the variables in the mammals data?  

```r
names(mammals)
```

```
##  [1] "order"            "family"           "genus"            "species"         
##  [5] "mass"             "gestation"        "newborn"          "weaning"         
##  [9] "wean_mass"        "AFR"              "max_life"         "litter_size"     
## [13] "litters_per_year"
```

3.  From the `mammals` data, filter all members of the family Bovidae with a mass greater than 450000.

```r
filter(mammals, family == "Bovidae" & mass > 450000)
```

```
## # A tibble: 7 × 13
##   order        family genus species   mass gesta…¹ newborn weaning wean_…²   AFR
##   <chr>        <chr>  <chr> <chr>    <dbl>   <dbl>   <dbl>   <dbl>   <dbl> <dbl>
## 1 Artiodactyla Bovid… Bison bison   4.98e5    8.93  20000    10.7   157500  29.4
## 2 Artiodactyla Bovid… Bison bonasus 5   e5    9.14  23000.    6.6     -999  30.0
## 3 Artiodactyla Bovid… Bos   fronta… 8   e5    9.02  23033.    4.5     -999  24.2
## 4 Artiodactyla Bovid… Bos   javani… 6.67e5    9.83   -999     9.5     -999  25.5
## 5 Artiodactyla Bovid… Buba… bubalis 9.5 e5   10.5   37500     7.5     -999  19.9
## 6 Artiodactyla Bovid… Sync… caffer  5.05e5   11.0   42862.    9.18  166000  47.9
## 7 Artiodactyla Bovid… Taur… derbia… 6.8 e5    8.67   -999  -999       -999  36.4
## # … with 3 more variables: max_life <dbl>, litter_size <dbl>,
## #   litters_per_year <dbl>, and abbreviated variable names ¹​gestation,
## #   ²​wean_mass
```

4. Let's say we are only interested in family, genus, species, mass, and gestation. Relimit the mammals data frame to these variables.  

```r
relimit <- select(mammals, "family", "genus", "species", "mass", "gestation")
relimit
```

```
## # A tibble: 1,440 × 5
##    family         genus       species          mass gestation
##    <chr>          <chr>       <chr>           <dbl>     <dbl>
##  1 Antilocapridae Antilocapra americana      45375       8.13
##  2 Bovidae        Addax       nasomaculatus 182375       9.39
##  3 Bovidae        Aepyceros   melampus       41480       6.35
##  4 Bovidae        Alcelaphus  buselaphus    150000       7.9 
##  5 Bovidae        Ammodorcas  clarkei        28500       6.8 
##  6 Bovidae        Ammotragus  lervia         55500       5.08
##  7 Bovidae        Antidorcas  marsupialis    30000       5.72
##  8 Bovidae        Antilope    cervicapra     37500       5.5 
##  9 Bovidae        Bison       bison         497667.      8.93
## 10 Bovidae        Bison       bonasus       500000       9.14
## # … with 1,430 more rows
```

5. Use the output from #4 to focus on the family Bovidae with a mass greater than 450000.

```r
filter(relimit, family=="Bovidae" & mass>450000)
```

```
## # A tibble: 7 × 5
##   family  genus       species      mass gestation
##   <chr>   <chr>       <chr>       <dbl>     <dbl>
## 1 Bovidae Bison       bison     497667.      8.93
## 2 Bovidae Bison       bonasus   500000       9.14
## 3 Bovidae Bos         frontalis 800000       9.02
## 4 Bovidae Bos         javanicus 666667.      9.83
## 5 Bovidae Bubalus     bubalis   950000      10.5 
## 6 Bovidae Syncerus    caffer    504667.     11.0 
## 7 Bovidae Taurotragus derbianus 680000       8.67
```

6. From the `mammals` data, build a data frame that compares `mass`, `gestation`, and `newborn` among the primate genera `Lophocebus`, `Erythrocebus`, and `Macaca`. Among these genera, which species has the smallest `newborn` mass?

```r
primate_genera <- filter(mammals, genus %in% c("Lohopcebus", "Erythrocebus", "Macaca"))
primate_genera
```

```
## # A tibble: 14 × 13
##    order    family   genus species   mass gesta…¹ newborn weaning wean_…²    AFR
##    <chr>    <chr>    <chr> <chr>    <dbl>   <dbl>   <dbl>   <dbl>   <dbl>  <dbl>
##  1 Primates Cercopi… Eryt… patas    5883.    5.56    546.    5.33   1950    32.8
##  2 Primates Cercopi… Maca… thibet… 10037.    5.67    533.   18.8    2400  -999  
##  3 Primates Cercopi… Maca… fuscata  8858.    5.72    505.   12.1    1768.   55.0
##  4 Primates Cercopi… Maca… maura    5575     5.43    390.   16.7    -999    60  
##  5 Primates Cercopi… Maca… sylvan…  9753.    5.49    450     7      2420    49.8
##  6 Primates Cercopi… Maca… arctoi…  7308.    6       486.   11.2    2300    47.8
##  7 Primates Cercopi… Maca… nigra    6212.    5.78    458.    7      -999    63.2
##  8 Primates Cercopi… Maca… sinica   3495  -999       446    13.0    -999    54  
##  9 Primates Cercopi… Maca… silenus  4875     5.94    418    12      -999    53.2
## 10 Primates Cercopi… Maca… mulatta  5413.    5.47    476.    9.47   1454    41.1
## 11 Primates Cercopi… Maca… cyclop…  6317.    5.4     401     6.87   -999  -999  
## 12 Primates Cercopi… Maca… nemest…  6133.    5.71    476.   10.7    1416.   43.6
## 13 Primates Cercopi… Maca… fascic…  3456.    5.49    408.    9.58    990.   43.6
## 14 Primates Cercopi… Maca… radiata  3735     5.43    391.   10.7    2000    45.4
## # … with 3 more variables: max_life <dbl>, litter_size <dbl>,
## #   litters_per_year <dbl>, and abbreviated variable names ¹​gestation,
## #   ²​wean_mass
```


```r
select(primate_genera, genus, species, mass, gestation, newborn)
```

```
## # A tibble: 14 × 5
##    genus        species        mass gestation newborn
##    <chr>        <chr>         <dbl>     <dbl>   <dbl>
##  1 Erythrocebus patas         5883.      5.56    546.
##  2 Macaca       thibetana    10037.      5.67    533.
##  3 Macaca       fuscata       8858.      5.72    505.
##  4 Macaca       maura         5575       5.43    390.
##  5 Macaca       sylvanus      9753.      5.49    450 
##  6 Macaca       arctoides     7308.      6       486.
##  7 Macaca       nigra         6212.      5.78    458.
##  8 Macaca       sinica        3495    -999       446 
##  9 Macaca       silenus       4875       5.94    418 
## 10 Macaca       mulatta       5413.      5.47    476.
## 11 Macaca       cyclopis      6317.      5.4     401 
## 12 Macaca       nemestrina    6133.      5.71    476.
## 13 Macaca       fascicularis  3456.      5.49    408.
## 14 Macaca       radiata       3735       5.43    391.
```

## Wrap-up  

Please review the learning goals and be sure to use the code here as a reference when completing the homework.  
-->[Home](https://jmledford3115.github.io/datascibiol/)
