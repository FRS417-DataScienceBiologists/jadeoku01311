---
title: "Lab 5 Homework"
author: "Jade Shi"
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code, keep track of your versions using git, and push your final work to our [GitHub repository](https://github.com/FRS417-DataScienceBiologists). I will randomly select a few examples of student work at the start of each session to use as examples so be sure that your code is working to the best of your ability.

## Load the tidyverse


```r
#install.packages("tidyverse")
```


```r
library(tidyverse)
```

## Mammals Life History
Let's revisit the mammal life history data to practice our `ggplot` skills. Some of the tidy steps will be a repeat from the homework, but it is good practice. The [data](http://esapubs.org/archive/ecol/E084/093/) are from: *S. K. Morgan Ernest. 2003. Life history characteristics of placental non-volant mammals. Ecology 84:3402.*

1. Load the data.

```r
life_history <-
  readr::read_csv(file = "C:/Users/JIN/Downloads/class_files-master/data/mammal_lifehistories_v2.csv")
```

```
## Parsed with column specification:
## cols(
##   order = col_character(),
##   family = col_character(),
##   Genus = col_character(),
##   species = col_character(),
##   mass = col_double(),
##   gestation = col_double(),
##   newborn = col_double(),
##   weaning = col_double(),
##   `wean mass` = col_double(),
##   AFR = col_double(),
##   `max. life` = col_double(),
##   `litter size` = col_double(),
##   `litters/year` = col_double()
## )
```


2. Use your preferred function to have a look. Do you notice any problems?

```r
glimpse(life_history)
```

```
## Observations: 1,440
## Variables: 13
## $ order          <chr> "Artiodactyla", "Artiodactyla", "Artiodactyla",...
## $ family         <chr> "Antilocapridae", "Bovidae", "Bovidae", "Bovida...
## $ Genus          <chr> "Antilocapra", "Addax", "Aepyceros", "Alcelaphu...
## $ species        <chr> "americana", "nasomaculatus", "melampus", "buse...
## $ mass           <dbl> 45375.0, 182375.0, 41480.0, 150000.0, 28500.0, ...
## $ gestation      <dbl> 8.13, 9.39, 6.35, 7.90, 6.80, 5.08, 5.72, 5.50,...
## $ newborn        <dbl> 3246.36, 5480.00, 5093.00, 10166.67, -999.00, 3...
## $ weaning        <dbl> 3.00, 6.50, 5.63, 6.50, -999.00, 4.00, 4.04, 2....
## $ `wean mass`    <dbl> 8900, -999, 15900, -999, -999, -999, -999, -999...
## $ AFR            <dbl> 13.53, 27.27, 16.66, 23.02, -999.00, 14.89, 10....
## $ `max. life`    <dbl> 142, 308, 213, 240, -999, 251, 228, 255, 300, 3...
## $ `litter size`  <dbl> 1.85, 1.00, 1.00, 1.00, 1.00, 1.37, 1.00, 1.00,...
## $ `litters/year` <dbl> 1.00, 0.99, 0.95, -999.00, -999.00, 2.00, -999....
```

## -999 is not presented as NA, untidy
3. There are NA's. How are you going to deal with them?

```r
life_history_na <-
  life_history %>% 
  na_if("-999")
life_history_na
```

```
## # A tibble: 1,440 x 13
##    order family Genus species   mass gestation newborn weaning `wean mass`
##    <chr> <chr>  <chr> <chr>    <dbl>     <dbl>   <dbl>   <dbl>       <dbl>
##  1 Arti~ Antil~ Anti~ americ~ 4.54e4      8.13   3246.    3           8900
##  2 Arti~ Bovid~ Addax nasoma~ 1.82e5      9.39   5480     6.5           NA
##  3 Arti~ Bovid~ Aepy~ melamp~ 4.15e4      6.35   5093     5.63       15900
##  4 Arti~ Bovid~ Alce~ busela~ 1.50e5      7.9   10167.    6.5           NA
##  5 Arti~ Bovid~ Ammo~ clarkei 2.85e4      6.8      NA    NA             NA
##  6 Arti~ Bovid~ Ammo~ lervia  5.55e4      5.08   3810     4             NA
##  7 Arti~ Bovid~ Anti~ marsup~ 3.00e4      5.72   3910     4.04          NA
##  8 Arti~ Bovid~ Anti~ cervic~ 3.75e4      5.5    3846     2.13          NA
##  9 Arti~ Bovid~ Bison bison   4.98e5      8.93  20000    10.7       157500
## 10 Arti~ Bovid~ Bison bonasus 5.00e5      9.14  23000.    6.6           NA
## # ... with 1,430 more rows, and 4 more variables: AFR <dbl>, `max.
## #   life` <dbl>, `litter size` <dbl>, `litters/year` <dbl>
```


4. Where are the NA's? This is important to keep in mind as you build plots.

```r
life_history_na %>% 
  purrr::map_df(~ sum(is.na(.))) %>% ##map to a new data frame the sum results of the is.na function for all columns
  tidyr::gather(variables, num_nas) %>% 
  arrange(desc(num_nas))
```

```
## # A tibble: 13 x 2
##    variables    num_nas
##    <chr>          <int>
##  1 wean mass       1039
##  2 max. life        841
##  3 litters/year     689
##  4 weaning          619
##  5 AFR              607
##  6 newborn          595
##  7 gestation        418
##  8 mass              85
##  9 litter size       84
## 10 order              0
## 11 family             0
## 12 Genus              0
## 13 species            0
```


5. Some of the variable names will be problematic. Let's rename them here as a final tidy step.

```r
life_history_na<-
  life_history_na %>% 
  rename(
          genus        = Genus,
          wean_mass    = `wean mass`,
          max_life     = `max. life`,
          litter_size  = `litter size`,
          litters_yr   = `litters/year`
          )
life_history_na
```

```
## # A tibble: 1,440 x 13
##    order family genus species   mass gestation newborn weaning wean_mass
##    <chr> <chr>  <chr> <chr>    <dbl>     <dbl>   <dbl>   <dbl>     <dbl>
##  1 Arti~ Antil~ Anti~ americ~ 4.54e4      8.13   3246.    3         8900
##  2 Arti~ Bovid~ Addax nasoma~ 1.82e5      9.39   5480     6.5         NA
##  3 Arti~ Bovid~ Aepy~ melamp~ 4.15e4      6.35   5093     5.63     15900
##  4 Arti~ Bovid~ Alce~ busela~ 1.50e5      7.9   10167.    6.5         NA
##  5 Arti~ Bovid~ Ammo~ clarkei 2.85e4      6.8      NA    NA           NA
##  6 Arti~ Bovid~ Ammo~ lervia  5.55e4      5.08   3810     4           NA
##  7 Arti~ Bovid~ Anti~ marsup~ 3.00e4      5.72   3910     4.04        NA
##  8 Arti~ Bovid~ Anti~ cervic~ 3.75e4      5.5    3846     2.13        NA
##  9 Arti~ Bovid~ Bison bison   4.98e5      8.93  20000    10.7     157500
## 10 Arti~ Bovid~ Bison bonasus 5.00e5      9.14  23000.    6.6         NA
## # ... with 1,430 more rows, and 4 more variables: AFR <dbl>,
## #   max_life <dbl>, litter_size <dbl>, litters_yr <dbl>
```



##`ggplot()`
For the questions below, try to use the aesthetics you have learned to make visually appealing and informative plots. Make sure to include labels for the axes and titles.

```r
options(scipen=999) #cancels the use of scientific notation for the session
```

6. What is the relationship between newborn body mass and gestation? Make a scatterplot that shows this relationship. 

```r
life_history_na %>% 
  ggplot(aes(newborn,gestation))+
  geom_point()
```

```
## Warning: Removed 673 rows containing missing values (geom_point).
```

![](lab5_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->


7. You should notice that because of the outliers in newborn mass, we need to make some changes. We didn't talk about this in lab, but you can use `scale_x_log10()` as a layer to correct for this issue. This will log transform the y-axis values.

```r
life_history_na %>% 
  ggplot(aes(newborn,gestation))+
  geom_point()+
  scale_x_log10()
```

```
## Warning: Removed 673 rows containing missing values (geom_point).
```

![](lab5_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->


8. Now that you have the basic plot, color the points by taxonomic order.


9. Lastly, make the size of the points proportional to body mass.


10. Make a plot that shows the range of lifespan by order.


## Push your final code to [GitHub](https://github.com/FRS417-DataScienceBiologists)
Make sure that you push your code into the appropriate folder. Also, be sure that you have check the `keep md` file in the knit preferences.
