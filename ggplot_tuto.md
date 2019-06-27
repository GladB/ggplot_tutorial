``` r
knitr::opts_chunk$set(error = TRUE)
```

ggplot tutorial
===============

What is ggplot?
---------------

BLABLA TODO graphics grammar etc

Actually doing things -- first dive into ggplot2
------------------------------------------------

First step: install and import `ggplot2` (surprising, right)

WHY TIDYVERSE TODO

``` r
# library(ggplot2)
library(tidyverse)
```

    ## ── Attaching packages ────────────────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.1.0       ✔ purrr   0.3.2  
    ## ✔ tibble  2.1.1       ✔ dplyr   0.8.0.1
    ## ✔ tidyr   0.8.3       ✔ stringr 1.4.0  
    ## ✔ readr   1.3.1       ✔ forcats 0.3.0

    ## Warning: package 'tibble' was built under R version 3.5.2

    ## Warning: package 'tidyr' was built under R version 3.5.2

    ## Warning: package 'purrr' was built under R version 3.5.2

    ## Warning: package 'dplyr' was built under R version 3.5.2

    ## Warning: package 'stringr' was built under R version 3.5.2

    ## ── Conflicts ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

Second step: get data (and here you thought nothing could surprise you anymore)

If you ever want to try something out, you should know that there are built-in datasets for you to test things on.

``` r
## see all datasets available
# data()

## load one of them
data(iris)

## take a look at what it looks like
head(iris)
```

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
    ## 1          5.1         3.5          1.4         0.2  setosa
    ## 2          4.9         3.0          1.4         0.2  setosa
    ## 3          4.7         3.2          1.3         0.2  setosa
    ## 4          4.6         3.1          1.5         0.2  setosa
    ## 5          5.0         3.6          1.4         0.2  setosa
    ## 6          5.4         3.9          1.7         0.4  setosa

And now, for the actual plotting...

``` r
ggplot()
```

![](ggplot_tuto_files/figure-markdown_github/test0-1.png)

Here we initialized the plot object. Basically, this tells R: hey, this is going to be a plot. Now we would like this plot to have something in it, and to add layers to a plot, we use "+", so let's try to add points:

``` r
ggplot()+
  geom_point(data=iris)
```

    ## Error: geom_point requires the following missing aesthetics: x, y

![](ggplot_tuto_files/figure-markdown_github/test2-1.png) Oh no, an error! Fortunately, R tells you what is wrong: it requires an aesthetics object containing necessary information (here, x and y). \[learning how to read errors thrown by R: very useful!\]

So here we said "Hey R, display points!" and R answered "Ok, where?"

What are aesthetics? BLABLA TODO

Let's indicate to R in which positions we want points and plot the lengths of petals depending on the lengths of sepals:

``` r
ggplot()+
  geom_histogram(aes(x=Sepal.Length, fill="sepal length", alpha = 0.5), data=iris)+
  geom_histogram(aes(x=Petal.Length, fill="petal length", alpha=0.5), data=iris)+
  facet_wrap(~Species)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](ggplot_tuto_files/figure-markdown_github/first_plot-1.png)

``` r
ggplot()+
  geom_point(aes(x=Sepal.Length, y=Petal.Length), data=iris)
```

![](ggplot_tuto_files/figure-markdown_github/second_plot-1.png) What if we also want, say, a smoothed curve for this data?

``` r
ggplot()+
  geom_point(data=iris, aes(x=Sepal.Length, y=Petal.Length))+
  geom_smooth(data=iris, aes(x=Sepal.Length, y=Petal.Length))
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](ggplot_tuto_files/figure-markdown_github/third_plot-1.png)

Well this is a bit repetitive, isn't it? Does this mean that we will have to repeat the data, x and y values anytime we want to do something new with them?

Well no! We can put everything only once, as an argument of the `ggplot()` function: anything that is written there will be put in common with all the other layers:

``` r
ggplot(data=iris, aes(x=Sepal.Length, y=Petal.Length))+
  geom_point()+
  geom_smooth()
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](ggplot_tuto_files/figure-markdown_github/common_plot-1.png)

``` r
## adding a new layer
ggplot(data=iris, aes(x=Sepal.Length, y=Petal.Length))+
  geom_point()+
  geom_smooth()+
  geom_rug()
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](ggplot_tuto_files/figure-markdown_github/common_plot-2.png)

Other information can be specified about the plot: color, size, shape, fill, transparency, ...

``` r
ggplot(data=iris, aes(x=Sepal.Length, y=Petal.Length))+
  geom_point(color="darkblue")+
  geom_smooth(color="green")
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](ggplot_tuto_files/figure-markdown_github/color_plot-1.png)

If this information depends on the data, it should be specified in the aes:

``` r
ggplot(data=iris, aes(x=Sepal.Length, y=Petal.Length, color=Species))+
  geom_point()+
  geom_smooth()
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](ggplot_tuto_files/figure-markdown_github/color2_plot-1.png)

What?! No! I just wanted the points to be colored based on the species they belong to, not have a different regression for each species! No worries, we can do that too, just specify that information in a layer-specific aes:

``` r
ggplot(data=iris, aes(x=Sepal.Length, y=Petal.Length))+
  geom_point(aes(color=Species))+
  geom_smooth(color="black")
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](ggplot_tuto_files/figure-markdown_github/color3_plot-1.png)

``` r
ggplot(data=iris, aes(x=Sepal.Length, y=Petal.Length))+
  geom_point(aes(color=Species, size=Petal.Width, alpha=0.8))+
  geom_smooth(color="black", fill="yellow")+
  facet_grid(~Species)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](ggplot_tuto_files/figure-markdown_github/another_example_plot-1.png)

Let's do some actual analysis
-----------------------------

YES the order matters:

``` r
ggplot(iris, aes(x=Species, y=Petal.Length, color=Species, fill=Species)) +
  geom_boxplot(width = 0.6, alpha=0.5)+
  geom_jitter(width=0.3, height=0)+
  geom_violin(alpha = 0.5, fill="grey") 
```

![](ggplot_tuto_files/figure-markdown_github/actual_plot-1.png)

``` r
  # geom_boxplot(width = 0.6, alpha=0.5)


ggplot(iris, aes(x=Species, y=Petal.Length, color=Species, fill=Species)) +
  geom_jitter(width=0.1, alpha=0.5)+
  geom_violin(alpha = 0.5, fill="grey", position = position_nudge(x=0.6))+
  geom_boxplot(width = 0.3, alpha=0.5)+
  coord_flip()
```

![](ggplot_tuto_files/figure-markdown_github/actual_plot-2.png)
