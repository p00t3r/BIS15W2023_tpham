---
title: "Lab 3 Homework"
author: "Tim Pham"
date: "2023-01-18"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Mammals Sleep
1. For this assignment, we are going to use built-in data on mammal sleep patterns. From which publication are these data taken from? Since the data are built-in you can use the help function in R.
The data is from the National Academy of Sciences.


```r
?msleep
```

```
## starting httpd help server ... done
```

2. Store these data into a new data frame `sleep`.

```r
sleep <- msleep
```

3. What are the dimensions of this data frame (variables and observations)? How do you know? Please show the *code* that you used to determine this below.  

There are 11 variables and 83 observations.

```r
dim(sleep)
```

```
## [1] 83 11
```

```r
nrow(sleep)
```

```
## [1] 83
```

```r
ncol(sleep)
```

```
## [1] 11
```

4. Are there any NAs in the data? How did you determine this? Please show your code.

Yes, there are NAs in the data.

```r
my_missing <- NA
is.na(my_missing)
```

```
## [1] TRUE
```

5. Show a list of the column names is this data frame.

```r
names(sleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"        "conservation"
##  [6] "sleep_total"  "sleep_rem"    "sleep_cycle"  "awake"        "brainwt"     
## [11] "bodywt"
```

6. How many herbivores are represented in the data?

There are 32 herbivores in the data.

```r
table(sleep$vore)
```

```
## 
##   carni   herbi insecti    omni 
##      19      32       5      20
```

7. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 1kg body weight and large as greater than or equal to 200kg body weight. Make two new dataframes (large and small) based on these parameters.


```r
small_mammals <- filter(sleep, bodywt<=1)
large_mammals <- filter(sleep, bodywt>=200)
```

8. What is the mean weight for both the small and large mammals?

The mean weight for small mammals is 0.259 kg, while the mean weight for large mammals is 1747.07 kg.

```r
mean(small_mammals$bodywt)
```

```
## [1] 0.2596667
```

```r
mean(large_mammals$bodywt)
```

```
## [1] 1747.071
```

9. Using a similar approach as above, do large or small animals sleep longer on average?  

Small animals sleep longer on average.

```r
mean(small_mammals$sleep_total)
```

```
## [1] 12.65833
```


```r
mean(large_mammals$sleep_total)
```

```
## [1] 3.3
```

10. Which animal is the sleepiest among the entire dataframe?

The little brown bat is the sleepiest among the entire dataframe.

```r
max(sleep$sleep_total)
```

```
## [1] 19.9
```


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
