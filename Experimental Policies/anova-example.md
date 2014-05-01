Explanation of How to Do An ANOVA
===================================


```r
balloon.data <- data.frame(color = c(rep("green", 6), rep("red", 12), rep("blue", 
    6)), shape = rep(c("square", "circle"), 12), size = rpois(24, lambda = 5) * 
    abs(rnorm(24)))
```


Check Distribution of Your Data
---------------------------------

First check the distribution of your groups of data using a Shapiro-Wilk Test and then then the presumption of equal variance with a Levene's Test


```r
# super duper easy shapiro test shortcut
with(balloon.data, tapply(size, shape, FUN = function(x) shapiro.test(x)$p.value))
```

```
##    circle    square 
## 0.2335224 0.0004579
```

```r
with(balloon.data, tapply(size, list(color, shape), FUN = function(x) shapiro.test(x)$p.value))
```

```
##       circle    square
## blue  0.1513 0.4988157
## green 0.7133 0.5227583
## red   0.6997 0.0006704
```


One Way ANOVA
--------------
This is used when you have one variable, with several types.  In this example you have several colors of balloons and are testing the hypothesis that size is affected by balloon color.


```r
# one way anova (two types)
summary(aov(size ~ shape, data = balloon.data))
```

```
##             Df Sum Sq Mean Sq F value Pr(>F)
## shape        1     22    21.5    0.69   0.41
## Residuals   22    682    31.0
```

```r
# same as a t-test
t.test(size ~ shape, data = balloon.data, var.equal = T)
```

```
## 
## 	Two Sample t-test
## 
## data:  size by shape
## t = 0.8334, df = 22, p-value = 0.4136
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -2.819  6.606
## sample estimates:
## mean in group circle mean in group square 
##                6.091                4.198
```

```r

# one way anova (3 types)
summary(aov(size ~ color, data = balloon.data))
```

```
##             Df Sum Sq Mean Sq F value Pr(>F)
## color        2      8     3.8    0.12   0.89
## Residuals   21    695    33.1
```


Two-Way ANOVA
---------------
This is used when you have two (or more) potential variables which affect your dependent variable.


```r
# two way anova, with interaction
summary(aov(size ~ color * shape, data = balloon.data))
```

```
##             Df Sum Sq Mean Sq F value Pr(>F)
## color        2      8     3.8    0.11   0.90
## shape        1     22    21.5    0.60   0.45
## color:shape  2     25    12.6    0.35   0.71
## Residuals   18    649    36.0
```

```r

# two way anova, without interaction
summary(aov(size ~ color + shape, data = balloon.data))
```

```
##             Df Sum Sq Mean Sq F value Pr(>F)
## color        2      8     3.8    0.11   0.89
## shape        1     22    21.5    0.64   0.43
## Residuals   20    674    33.7
```


Assumptions of ANOVA Analyses
-------------------------------
Although when group sizes are equal, ANOVA is quite robust to non-normality and non-equal variance, you should test these assumptions.  Normality was tested above.  If the data is not normal, you could transform your dependent variable (ie look at log(size) rather than size).  You could also use a non-parametric test such as a Kruskal-Wallis test


```r
# back to the levene's test
library(car)
with(balloon.data, leveneTest(size ~ shape * shape))
```

```
## Levene's Test for Homogeneity of Variance (center = median)
##       Df F value Pr(>F)
## group  1    0.05   0.82
##       22
```

```r

# if not normally distributed try a kruskal wallis test for non-parametric
# data
kruskal.test(size ~ color, data = balloon.data)
```

```
## 
## 	Kruskal-Wallis rank sum test
## 
## data:  size by color
## Kruskal-Wallis chi-squared = 0.66, df = 2, p-value = 0.7189
```

```r
# anova for comparason
summary(aov(size ~ color, data = balloon.data))
```

```
##             Df Sum Sq Mean Sq F value Pr(>F)
## color        2      8     3.8    0.12   0.89
## Residuals   21    695    33.1
```


Session Information
--------------------


```
## R version 3.1.0 (2014-04-10)
## Platform: x86_64-apple-darwin13.1.0 (64-bit)
## 
## locale:
## [1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
## [1] car_2.0-19 knitr_1.5 
## 
## loaded via a namespace (and not attached):
## [1] evaluate_0.5.3 formatR_0.10   MASS_7.3-31    nnet_7.3-8    
## [5] stringr_0.6.2  tools_3.1.0
```

