---
title: "Data Visualization 1"
author: "Joel Ledford"
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
    toc: yes
    toc_float: yes
  pdf_document:
    toc: yes
---

## Where have we been, and where are we going?
At this point you should feel reasonably comfortable working in RStudio and using dplyr and tidyr. You also know how to produce statistical summaries of data and deal with NA's. It is OK if you need to go back through the labs and find bits of code that work for you, but try and force yourself to originate new chunks.  

## Group Project
Meet with your group and decide on a data set that you will use for your project. Be prepared to discuss these data, where you found them, and what you hope to learn.  

##Resources
- [ggplot2 cheatsheet](https://www.rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf)
- [R for Data Science](https://r4ds.had.co.nz/)
- [R Cookbook](http://www.cookbook-r.com/)
- [`ggplot` themes](https://ggplot2.tidyverse.org/reference/ggtheme.html)
- [Rebecca Barter `ggplot` Tutorial](http://www.rebeccabarter.com/blog/2017-11-17-ggplot2_tutorial/)

## Learning Goals
*At the end of this exercise, you will be able to:*    
1. Understand and apply the syntax of building plots using `ggplot2`.  
2. Build a boxplot using `ggplot2`.  
3. Build a scatterplot using `ggplot2`.  
4. Build a barplot using `ggplot2` and show the difference between `stat=count` and `stat=identity`.  

## Plots are built in layers

## Load the libraries

```r
#install.packages("tidyverse")
```


```r
#install.packages("skimr")
```




```r
library(tidyverse)
library(skimr)
```

## Grammar of Graphics
The ability to quickly produce and edit beautiful graphs and charts is a strength of R. These data visualizations are produced by the package `ggplot2` and it is a core part of the tidyverse. The syntax for using ggplot is specific and common to all of the plots. This is what Hadley Wickham calls a [Grammar of Graphics](http://vita.had.co.nz/papers/layered-grammar.pdf). The "gg" in `ggplot` stands for grammar of graphics.

## Philosophy
What makes a good chart? In my opinion a good chart is elegant in its simplicity. It provides a clean, clear visual of the data without being overwhelming to the reader. This can be hard to do and takes some careful thinking. Always keep in mind that the reader will almost never know the data as well as you do so you need to be mindful about presenting the facts.  

## Data Types
While this isn't a statistics class, we need to define some of the data types we will use to build plots.  

+ `discrete` quantitative data that only contains integers
+ `continuous` quantitative data that can take any numerical value
+ `categorical` qualitative data that can take on a limited number of values

## Basics
The syntax used by ggplot takes some practice to get used to, especially for customizing plots, but the basic elements are the same. It is helpful to think of plots as being built up in layers. In short, **plot= data + geom_ + aesthetics**.  

We start by calling the ggplot function, identifying the data, and specifying the axes. We then add the `geom` type to describe which type of plot we want to make. Each `geom_` works with specific types of data and R is capable of building plots of single variables, multiple variables, and even maps. Lastly, we add aesthetics.

## Example
To make things easy, let's start with some built in data.

```r
?iris
names(iris)
```

```
## [1] "Sepal.Length" "Sepal.Width"  "Petal.Length" "Petal.Width" 
## [5] "Species"
```

```r
iris
```

```
##     Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
## 1            5.1         3.5          1.4         0.2     setosa
## 2            4.9         3.0          1.4         0.2     setosa
## 3            4.7         3.2          1.3         0.2     setosa
## 4            4.6         3.1          1.5         0.2     setosa
## 5            5.0         3.6          1.4         0.2     setosa
## 6            5.4         3.9          1.7         0.4     setosa
## 7            4.6         3.4          1.4         0.3     setosa
## 8            5.0         3.4          1.5         0.2     setosa
## 9            4.4         2.9          1.4         0.2     setosa
## 10           4.9         3.1          1.5         0.1     setosa
## 11           5.4         3.7          1.5         0.2     setosa
## 12           4.8         3.4          1.6         0.2     setosa
## 13           4.8         3.0          1.4         0.1     setosa
## 14           4.3         3.0          1.1         0.1     setosa
## 15           5.8         4.0          1.2         0.2     setosa
## 16           5.7         4.4          1.5         0.4     setosa
## 17           5.4         3.9          1.3         0.4     setosa
## 18           5.1         3.5          1.4         0.3     setosa
## 19           5.7         3.8          1.7         0.3     setosa
## 20           5.1         3.8          1.5         0.3     setosa
## 21           5.4         3.4          1.7         0.2     setosa
## 22           5.1         3.7          1.5         0.4     setosa
## 23           4.6         3.6          1.0         0.2     setosa
## 24           5.1         3.3          1.7         0.5     setosa
## 25           4.8         3.4          1.9         0.2     setosa
## 26           5.0         3.0          1.6         0.2     setosa
## 27           5.0         3.4          1.6         0.4     setosa
## 28           5.2         3.5          1.5         0.2     setosa
## 29           5.2         3.4          1.4         0.2     setosa
## 30           4.7         3.2          1.6         0.2     setosa
## 31           4.8         3.1          1.6         0.2     setosa
## 32           5.4         3.4          1.5         0.4     setosa
## 33           5.2         4.1          1.5         0.1     setosa
## 34           5.5         4.2          1.4         0.2     setosa
## 35           4.9         3.1          1.5         0.2     setosa
## 36           5.0         3.2          1.2         0.2     setosa
## 37           5.5         3.5          1.3         0.2     setosa
## 38           4.9         3.6          1.4         0.1     setosa
## 39           4.4         3.0          1.3         0.2     setosa
## 40           5.1         3.4          1.5         0.2     setosa
## 41           5.0         3.5          1.3         0.3     setosa
## 42           4.5         2.3          1.3         0.3     setosa
## 43           4.4         3.2          1.3         0.2     setosa
## 44           5.0         3.5          1.6         0.6     setosa
## 45           5.1         3.8          1.9         0.4     setosa
## 46           4.8         3.0          1.4         0.3     setosa
## 47           5.1         3.8          1.6         0.2     setosa
## 48           4.6         3.2          1.4         0.2     setosa
## 49           5.3         3.7          1.5         0.2     setosa
## 50           5.0         3.3          1.4         0.2     setosa
## 51           7.0         3.2          4.7         1.4 versicolor
## 52           6.4         3.2          4.5         1.5 versicolor
## 53           6.9         3.1          4.9         1.5 versicolor
## 54           5.5         2.3          4.0         1.3 versicolor
## 55           6.5         2.8          4.6         1.5 versicolor
## 56           5.7         2.8          4.5         1.3 versicolor
## 57           6.3         3.3          4.7         1.6 versicolor
## 58           4.9         2.4          3.3         1.0 versicolor
## 59           6.6         2.9          4.6         1.3 versicolor
## 60           5.2         2.7          3.9         1.4 versicolor
## 61           5.0         2.0          3.5         1.0 versicolor
## 62           5.9         3.0          4.2         1.5 versicolor
## 63           6.0         2.2          4.0         1.0 versicolor
## 64           6.1         2.9          4.7         1.4 versicolor
## 65           5.6         2.9          3.6         1.3 versicolor
## 66           6.7         3.1          4.4         1.4 versicolor
## 67           5.6         3.0          4.5         1.5 versicolor
## 68           5.8         2.7          4.1         1.0 versicolor
## 69           6.2         2.2          4.5         1.5 versicolor
## 70           5.6         2.5          3.9         1.1 versicolor
## 71           5.9         3.2          4.8         1.8 versicolor
## 72           6.1         2.8          4.0         1.3 versicolor
## 73           6.3         2.5          4.9         1.5 versicolor
## 74           6.1         2.8          4.7         1.2 versicolor
## 75           6.4         2.9          4.3         1.3 versicolor
## 76           6.6         3.0          4.4         1.4 versicolor
## 77           6.8         2.8          4.8         1.4 versicolor
## 78           6.7         3.0          5.0         1.7 versicolor
## 79           6.0         2.9          4.5         1.5 versicolor
## 80           5.7         2.6          3.5         1.0 versicolor
## 81           5.5         2.4          3.8         1.1 versicolor
## 82           5.5         2.4          3.7         1.0 versicolor
## 83           5.8         2.7          3.9         1.2 versicolor
## 84           6.0         2.7          5.1         1.6 versicolor
## 85           5.4         3.0          4.5         1.5 versicolor
## 86           6.0         3.4          4.5         1.6 versicolor
## 87           6.7         3.1          4.7         1.5 versicolor
## 88           6.3         2.3          4.4         1.3 versicolor
## 89           5.6         3.0          4.1         1.3 versicolor
## 90           5.5         2.5          4.0         1.3 versicolor
## 91           5.5         2.6          4.4         1.2 versicolor
## 92           6.1         3.0          4.6         1.4 versicolor
## 93           5.8         2.6          4.0         1.2 versicolor
## 94           5.0         2.3          3.3         1.0 versicolor
## 95           5.6         2.7          4.2         1.3 versicolor
## 96           5.7         3.0          4.2         1.2 versicolor
## 97           5.7         2.9          4.2         1.3 versicolor
## 98           6.2         2.9          4.3         1.3 versicolor
## 99           5.1         2.5          3.0         1.1 versicolor
## 100          5.7         2.8          4.1         1.3 versicolor
## 101          6.3         3.3          6.0         2.5  virginica
## 102          5.8         2.7          5.1         1.9  virginica
## 103          7.1         3.0          5.9         2.1  virginica
## 104          6.3         2.9          5.6         1.8  virginica
## 105          6.5         3.0          5.8         2.2  virginica
## 106          7.6         3.0          6.6         2.1  virginica
## 107          4.9         2.5          4.5         1.7  virginica
## 108          7.3         2.9          6.3         1.8  virginica
## 109          6.7         2.5          5.8         1.8  virginica
## 110          7.2         3.6          6.1         2.5  virginica
## 111          6.5         3.2          5.1         2.0  virginica
## 112          6.4         2.7          5.3         1.9  virginica
## 113          6.8         3.0          5.5         2.1  virginica
## 114          5.7         2.5          5.0         2.0  virginica
## 115          5.8         2.8          5.1         2.4  virginica
## 116          6.4         3.2          5.3         2.3  virginica
## 117          6.5         3.0          5.5         1.8  virginica
## 118          7.7         3.8          6.7         2.2  virginica
## 119          7.7         2.6          6.9         2.3  virginica
## 120          6.0         2.2          5.0         1.5  virginica
## 121          6.9         3.2          5.7         2.3  virginica
## 122          5.6         2.8          4.9         2.0  virginica
## 123          7.7         2.8          6.7         2.0  virginica
## 124          6.3         2.7          4.9         1.8  virginica
## 125          6.7         3.3          5.7         2.1  virginica
## 126          7.2         3.2          6.0         1.8  virginica
## 127          6.2         2.8          4.8         1.8  virginica
## 128          6.1         3.0          4.9         1.8  virginica
## 129          6.4         2.8          5.6         2.1  virginica
## 130          7.2         3.0          5.8         1.6  virginica
## 131          7.4         2.8          6.1         1.9  virginica
## 132          7.9         3.8          6.4         2.0  virginica
## 133          6.4         2.8          5.6         2.2  virginica
## 134          6.3         2.8          5.1         1.5  virginica
## 135          6.1         2.6          5.6         1.4  virginica
## 136          7.7         3.0          6.1         2.3  virginica
## 137          6.3         3.4          5.6         2.4  virginica
## 138          6.4         3.1          5.5         1.8  virginica
## 139          6.0         3.0          4.8         1.8  virginica
## 140          6.9         3.1          5.4         2.1  virginica
## 141          6.7         3.1          5.6         2.4  virginica
## 142          6.9         3.1          5.1         2.3  virginica
## 143          5.8         2.7          5.1         1.9  virginica
## 144          6.8         3.2          5.9         2.3  virginica
## 145          6.7         3.3          5.7         2.5  virginica
## 146          6.7         3.0          5.2         2.3  virginica
## 147          6.3         2.5          5.0         1.9  virginica
## 148          6.5         3.0          5.2         2.0  virginica
## 149          6.2         3.4          5.4         2.3  virginica
## 150          5.9         3.0          5.1         1.8  virginica
```

To make a plot, we need to first specify the data and map the aesthetics. The aesthetics include how each variable in our dataset will be used. In the example below, I am using the aes() function to identify the x and y variables in the plot.

```r
ggplot(data=iris, mapping=aes(x=Species, y=Petal.Length))
```

![](lab5_1_rev_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

Notice that we have a nice background, labeled axes, and even values of our variables- but no plot. This is because we need to tell ggplot what type of plot we want to make. This is called the geometry or `geom()`.

Here we specify that we want a boxplot, indicated by `geom_boxplot()`.

```r
ggplot(data=iris, mapping=aes(x=Species, y=Petal.Length))+
  geom_boxplot()
```

![](lab5_1_rev_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

## Practice
1. Take a moment to practice. Use the iris data to build a scatterplot that compares sepal length vs. sepal width. Use the cheatsheet to find the correct `geom_` for a scatterplot.

```r
ggplot(data=iris, mapping=aes(x=Sepal.Length, y= Sepal.Width))+
  geom_point()
```

![](lab5_1_rev_files/figure-html/unnamed-chunk-7-1.png)<!-- -->


## Scatterplots, barplots, and boxplots
Now that we have a general idea of the syntax, let's start by working with two standard plots: 1) scatterplots and 2) barplots.

## Data
For the following examples, I am going to use data about vertebrate home range sizes.  

**Database of vertebrate home range sizes.**  
Reference: Tamburello N, Cote IM, Dulvy NK (2015) Energy and the scaling of animal space use. The American Naturalist 186(2):196-211. http://dx.doi.org/10.1086/682070.  
Data: http://datadryad.org/resource/doi:10.5061/dryad.q5j65/1  

```r
getwd()
```

```
## [1] "/Users/clmuser/Desktop/class_files-master"
```



```r
homerange <- readr::read_csv("/Users/clmuser/Desktop/class_files-master/Tamburelloetal_HomeRangeDatabase.csv")
```

A little bit of cleaning to focus on the variables of interest. Good `dplyr` practice!

```r
homerange <- 
  homerange %>%  
  select(common.name, taxon, family, genus, species, mean.mass.g, log10.mass, mean.hra.m2, log10.hra, preymass, log10.preymass, trophic.guild)
```


```r
names(homerange)
```

```
##  [1] "common.name"    "taxon"          "family"         "genus"         
##  [5] "species"        "mean.mass.g"    "log10.mass"     "mean.hra.m2"   
##  [9] "log10.hra"      "preymass"       "log10.preymass" "trophic.guild"
```

### 1. Scatter Plots
Scatter plots are good at revealing relationships that are not readily visible in the raw data. For now, we will not add regression lines or calculate any r^2^ values. In this case, we are exploring whether or not there is a relationship between animal mass and homerange. We are using log transformed values because there is a very large difference in mass and homerange among the different  species in the data.

```r
ggplot(data=homerange, mapping=aes(x=log10.mass, y=log10.hra)) +
  geom_point()
```

![](lab5_1_rev_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

In big data sets with lots of similar values, overplotting can be an issue. `geom_jitter()` is similar to `geom_point()` but it helps with overplotting by adding some random noise to the data and separating some of the individual points.

```r
ggplot(data=homerange, mapping=aes(x=log10.mass, y=log10.hra)) +
  geom_jitter()
```

![](lab5_1_rev_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

You want to see the regression line, right?

```r
ggplot(data=homerange, mapping=aes(x=log10.mass, y=log10.hra)) +
  geom_jitter()+
  geom_smooth(method=lm, se=FALSE) #lm=linear model
```

![](lab5_1_rev_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

### 2A. Bar Plot: `stat="count"`
When making a bar graph, the default is to count the number of observations in the specified column. This is best for categorical data and is defined by the aesthetic `stat="count"`. Here, I want to know how many carnivores vs. herbivores are in the data.  

Notice that we can use pipes and the `mapping=` function is implied by `aes` and so is often left out.  

```r
homerange %>% 
  ggplot(aes(x=trophic.guild))+
  geom_bar()#(stat="count") # I am identifying stat="count" but this isn't necessary since it is default
```

![](lab5_1_rev_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

### 2B. Bar Plot: `stat="identity"`
`stat="identity"` allows us to map a variable to the y axis so that we aren't restricted to count. Let's use our dplyr skills to first filter out carnivorous mammals and see which families have the highest mean body weight.

```r
carni_mammals <- 
  homerange %>% 
  filter(taxon=="mammals", 
         trophic.guild=="carnivore") %>% 
  group_by(family) %>% 
  summarize(count=n(),
            mean_body_wt=mean(log10.mass)) %>% 
  arrange(desc(mean_body_wt))
carni_mammals
```

```
## # A tibble: 18 x 3
##    family          count mean_body_wt
##    <chr>           <int>        <dbl>
##  1 ursidae             1        4.99 
##  2 felidae            19        4.16 
##  3 hyanidae            1        4    
##  4 eupleridae          1        3.98 
##  5 canidae             7        3.73 
##  6 viverridae          3        3.49 
##  7 herpestidae         5        3.16 
##  8 mustelidae         15        3.08 
##  9 peramelidae         2        2.74 
## 10 erinaceidae         2        2.69 
## 11 tachyglossidae      1        2.41 
## 12 dasyuridae          4        2.32 
## 13 macroscelididae     3        2.27 
## 14 chrysochloridae     2        2.00 
## 15 talpidae            4        1.90 
## 16 cricetidae          2        1.39 
## 17 didelphidae         2        1.38 
## 18 soricidae           6        0.882
```

Now let's plot the data using `stat="identity"` to help visualize these differences.

```r
carni_mammals%>% 
  ggplot(aes(x=family, y=mean_body_wt))+
  geom_bar(stat="identity")#do not use count here
```

![](lab5_1_rev_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

This looks nice, but the x axis is a mess and needs adjustment. We make these adjustments using `theme()`.

```r
carni_mammals%>% 
  ggplot(aes(x=family, y=mean_body_wt))+
  geom_bar(stat="identity")+
  theme(axis.text.x = element_text(angle = 45, hjust=1))
```

![](lab5_1_rev_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

We can make this a little bit better by reordering.

```r
carni_mammals%>% 
  ggplot(aes(x=reorder(family, -mean_body_wt), y=mean_body_wt))+
  geom_bar(stat="identity")+
  theme(axis.text.x = element_text(angle = 60, hjust=1))
```

![](lab5_1_rev_files/figure-html/unnamed-chunk-19-1.png)<!-- -->

Or we can flip the coordinates.

```r
carni_mammals%>% 
  ggplot(aes(x=reorder(family, -mean_body_wt), y=mean_body_wt))+
  geom_bar(stat="identity")+
  coord_flip()
```

![](lab5_1_rev_files/figure-html/unnamed-chunk-20-1.png)<!-- -->

## Practice
Filter the `homerange` data to include `mammals` only.

```r
mammals <- 
  homerange %>% 
  filter(taxon=="mammals") %>% 
  select(common.name, family, genus, species, trophic.guild, mean.mass.g, log10.mass, mean.hra.m2, log10.hra, preymass, log10.preymass)
```


```r
mammals
```

```
## # A tibble: 238 x 11
##    common.name family genus species trophic.guild mean.mass.g log10.mass
##    <chr>       <chr>  <chr> <chr>   <chr>               <dbl>      <dbl>
##  1 giant gold… chrys… chry… trevel… carnivore            437.       2.64
##  2 Grant's go… chrys… erem… granti  carnivore             23        1.36
##  3 pronghorn   antil… anti… americ… herbivore          46100.       4.66
##  4 impala      bovid… aepy… melamp… herbivore          63504.       4.80
##  5 hartebeest  bovid… alce… busela… herbivore         136000.       5.13
##  6 barbary sh… bovid… ammo… lervia  herbivore         167498.       5.22
##  7 American b… bovid… bison bison   herbivore         629999.       5.80
##  8 European b… bovid… bison bonasus herbivore         628001.       5.80
##  9 goat        bovid… capra hircus  herbivore          51000.       4.71
## 10 Spanish ib… bovid… capra pyrena… herbivore          69499.       4.84
## # … with 228 more rows, and 4 more variables: mean.hra.m2 <dbl>,
## #   log10.hra <dbl>, preymass <dbl>, log10.preymass <dbl>
```

1. Are there more herbivores or carnivores in mammals? Make a bar plot that shows their relative proportions.

```r
mammals %>% 
  ggplot(aes(x=trophic.guild))+
  geom_bar()
```

![](lab5_1_rev_files/figure-html/unnamed-chunk-23-1.png)<!-- -->


2. Is there a positive or negative relationship between mass and homerange? How does this compare between herbivores and carnivores? Make two plots that illustrate these eamples below.  


```r
mammals %>% 
  filter(trophic.guild =="carnivore") %>% 
  ggplot(aes(x=log10.mass, y=log10.hra))+
  geom_point()+
  geom_smooth(method=lm, se=FALSE)
```

![](lab5_1_rev_files/figure-html/unnamed-chunk-24-1.png)<!-- -->

```r
mammals %>% 
  filter(trophic.guild =="herbivore") %>% 
  ggplot(aes(x=log10.mass, y=log10.hra))+
  geom_point()+
  geom_smooth(method=lm, se=FALSE)
```

![](lab5_1_rev_files/figure-html/unnamed-chunk-25-1.png)<!-- -->


3. Make a barplot that shows the masses of the top 10 smallest mammals in terms of mass. Be sure to use `stat'="identity"`.
## Can we do this?

```r
smallest_mammals <-
  mammals %>% 
  arrange(mean.mass.g) %>% 
  filter(mean.mass.g<=22)
smallest_mammals
```

```
## # A tibble: 10 x 11
##    common.name family genus species trophic.guild mean.mass.g log10.mass
##    <chr>       <chr>  <chr> <chr>   <chr>               <dbl>      <dbl>
##  1 cinereus s… soric… sorex cinere… carnivore            4.17      0.620
##  2 slender sh… soric… sorex gracil… carnivore            4.37      0.640
##  3 arctic shr… soric… sorex arctic… carnivore            8.13      0.910
##  4 crowned sh… soric… sorex corona… carnivore            9.33      0.970
##  5 greater wh… soric… croc… russula carnivore           10         1    
##  6 salt marsh… crice… reit… ravive… herbivore           11.0       1.04 
##  7 long-clawe… soric… sorex unguic… carnivore           14.1       1.15 
##  8 Northern t… didel… mono… americ… carnivore           19.5       1.29 
##  9 wood mouse  murid… apod… sylvat… herbivore           21.2       1.33 
## 10 southern g… crice… onyc… torrid… carnivore           22         1.34 
## # … with 4 more variables: mean.hra.m2 <dbl>, log10.hra <dbl>,
## #   preymass <dbl>, log10.preymass <dbl>
```


```r
homerange %>% 
  filter(taxon =="mammals") %>% 
  select(common.name, mean.mass.g) %>% 
  arrange(mean.mass.g)
```

```
## # A tibble: 238 x 2
##    common.name                    mean.mass.g
##    <chr>                                <dbl>
##  1 cinereus shrew                        4.17
##  2 slender shrew                         4.37
##  3 arctic shrew                          8.13
##  4 crowned shrew                         9.33
##  5 greater white-footed shrew           10   
##  6 salt marsh harvest mouse             11.0 
##  7 long-clawed shrew                    14.1 
##  8 Northern three-striped opossum       19.5 
##  9 wood mouse                           21.2 
## 10 southern grasshopper mouse           22   
## # … with 228 more rows
```


```r
mammals %>% 
  top_n(-10,log10.mass) %>% 
  ggplot(aes(x=reorder(common.name, log10.mass), y=log10.mass))+
  geom_bar(stat = "identity")+
  coord_flip()
```

![](lab5_1_rev_files/figure-html/unnamed-chunk-28-1.png)<!-- -->

## Let's Take a Break!
