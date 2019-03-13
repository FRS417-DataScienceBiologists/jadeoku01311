---
title: "Untitled"
output: 
  html_document: 
    keep_md: yes
---



```r
#install.packages("tidyverse")
#install.packages("skimr")
library(tidyverse)
```

```
## -- Attaching packages --------------------------------------------------------------------- tidyverse 1.2.1 --
```

```
## v ggplot2 3.1.0     v purrr   0.3.0
## v tibble  2.0.1     v dplyr   0.7.8
## v tidyr   0.8.2     v stringr 1.4.0
## v readr   1.3.1     v forcats 0.3.0
```

```
## -- Conflicts ------------------------------------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(skimr)
```

```r
getwd()
```

```
## [1] "C:/Users/Isha/Desktop/Animals-master"
```

```r
getwd()
```

```
## [1] "C:/Users/Isha/Desktop/Animals-master"
```



```r
bee1 <- 
   readr::read_csv("C:/Users/Isha/Desktop/Animals-master/finley-bee-colony-statistical-data-from-1987-2017/Bee Colony Survey Data by State.csv")
```

```
## Parsed with column specification:
## cols(
##   Year = col_double(),
##   Period = col_character(),
##   `Week Ending` = col_logical(),
##   State = col_character(),
##   `State ANSI` = col_double(),
##   Watershed = col_logical(),
##   `Data Item` = col_character(),
##   Value = col_number(),
##   `CV (%)` = col_logical()
## )
```

```r
bee1
```

```
## # A tibble: 3,396 x 9
##     Year Period `Week Ending` State `State ANSI` Watershed `Data Item`
##    <dbl> <chr>  <lgl>         <chr>        <dbl> <lgl>     <chr>      
##  1  2017 JAN T~ NA            ALAB~            1 NA        ADDED & RE~
##  2  2017 JAN T~ NA            ARIZ~            4 NA        ADDED & RE~
##  3  2017 JAN T~ NA            ARKA~            5 NA        ADDED & RE~
##  4  2017 JAN T~ NA            CALI~            6 NA        ADDED & RE~
##  5  2017 JAN T~ NA            COLO~            8 NA        ADDED & RE~
##  6  2017 JAN T~ NA            CONN~            9 NA        ADDED & RE~
##  7  2017 JAN T~ NA            FLOR~           12 NA        ADDED & RE~
##  8  2017 JAN T~ NA            GEOR~           13 NA        ADDED & RE~
##  9  2017 JAN T~ NA            HAWA~           15 NA        ADDED & RE~
## 10  2017 JAN T~ NA            IDAHO           16 NA        ADDED & RE~
## # ... with 3,386 more rows, and 2 more variables: Value <dbl>, `CV
## #   (%)` <lgl>
```

```r
#install.packages("readxl")
library(readxl)
```


```r
bee2 <- 
  readxl::read_excel("C:/Users/Isha/Desktop/Animals-master/finley-bee-colony-statistical-data-from-1987-2017/Bee Colony Loss.xlsx")
bee2
```

```
## # A tibble: 365 x 8
##    Year  Season ` State` ` Total Annual ~ ` Beekeepers` ` Beekeepers Ex~
##    <chr> <chr>  <chr>               <dbl>         <dbl>            <dbl>
##  1 2016~ Annual Massach~            0.159            87            0.943
##  2 2016~ Annual Montana             0.171            21            0.524
##  3 2016~ Annual Nevada              0.23             13            0.923
##  4 2016~ Annual Maine               0.233            65            0.938
##  5 2016~ Annual Wyoming             0.234            18            0.778
##  6 2016~ Annual Hawaii              0.262            10            1    
##  7 2016~ Annual Mississ~            0.263             9            0.222
##  8 2016~ Annual West Vi~            0.266            52            0.942
##  9 2016~ Annual Idaho               0.273            30            0.833
## 10 2016~ Annual Florida             0.292            62            0.823
## # ... with 355 more rows, and 2 more variables: ` Colonies` <dbl>,
## #   ` Colonies Exclusive to State` <dbl>
```


```r
## totalBeeData <- merge(bee1, bee2, by="State")
##totalBeeData
```

## Bee loss vs state over the years  (jade) 
#Colonies per state  (bar plot) - isha 
#Beekeeper vs bee loss  (scatter plot) 
#Bee loss over the years (valerie 
#Bee loss over the years for each state


#Questions:
#There's a dataset in the folder called, "Bee colony Census Data by County"  and we wanted help in merging the data sets. 
#Also in the "Bee colony Census Data by County" data set, there's a column named "value" and we've looked in the glossary but we couldn't find any info on it. We were wondering if you knew what we could use the "value" column for or if it's even worth using anything in the "Bee colony Census Data by County" data set. 


