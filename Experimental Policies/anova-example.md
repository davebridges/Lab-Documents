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
##   circle   square 
## 0.005547 0.038849
```

```r
with(balloon.data, tapply(size, list(color, shape), FUN = function(x) shapiro.test(x)$p.value))
```

```
##        circle square
## blue  0.08744 0.8102
## green 0.16414 0.8346
## red   0.07083 0.2541
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
## shape        1    5.5    5.51    0.58   0.45
## Residuals   22  207.2    9.42
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
## t = -0.7646, df = 22, p-value = 0.4526
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -3.556  1.640
## sample estimates:
## mean in group circle mean in group square 
##                2.739                3.697
```

```r

# one way anova (3 types)
summary(aov(size ~ color, data = balloon.data))
```

```
##             Df Sum Sq Mean Sq F value Pr(>F)
## color        2    1.5    0.76    0.08   0.93
## Residuals   21  211.2   10.06
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
## color        2    1.5    0.76    0.08   0.92
## shape        1    5.5    5.51    0.57   0.46
## color:shape  2   31.4   15.72    1.62   0.22
## Residuals   18  174.2    9.68
```

```r

# two way anova, without interaction
summary(aov(size ~ color + shape, data = balloon.data))
```

```
##             Df Sum Sq Mean Sq F value Pr(>F)
## color        2    1.5    0.76    0.07   0.93
## shape        1    5.5    5.51    0.54   0.47
## Residuals   20  205.7   10.28
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
## group  1       0   0.99
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
## Kruskal-Wallis chi-squared = 0.165, df = 2, p-value = 0.9208
```

```r
# anova for comparason
summary(aov(size ~ color, data = balloon.data))
```

```
##             Df Sum Sq Mean Sq F value Pr(>F)
## color        2    1.5    0.76    0.08   0.93
## Residuals   21  211.2   10.06
```

```r

# test for normality by runnning a shapiro test on the residuals
shapiro.test(residuals(aov(size ~ color + shape, data = balloon.data)))
```

```
## 
## 	Shapiro-Wilk normality test
## 
## data:  residuals(aov(size ~ color + shape, data = balloon.data))
## W = 0.8272, p-value = 0.0008499
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

