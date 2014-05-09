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
## 0.5592173 0.0008791
```

```r
with(balloon.data, tapply(size, list(color, shape), FUN = function(x) shapiro.test(x)$p.value))
```

```
##       circle square
## blue  0.5226 0.4274
## green 0.2452 0.1140
## red   0.1722 0.2862
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
## shape        1   14.8    14.8    1.29   0.27
## Residuals   22  252.8    11.5
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
## t = 1.134, df = 22, p-value = 0.269
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -1.301  4.439
## sample estimates:
## mean in group circle mean in group square 
##                4.348                2.778
```

```r

# one way anova (3 types)
summary(aov(size ~ color, data = balloon.data))
```

```
##             Df Sum Sq Mean Sq F value Pr(>F)
## color        2   11.4    5.68    0.47   0.63
## Residuals   21  256.2   12.20
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
## color        2   11.4    5.68    0.47   0.63
## shape        1   14.8   14.78    1.22   0.28
## color:shape  2   24.1   12.06    1.00   0.39
## Residuals   18  217.3   12.07
```

```r

# two way anova, without interaction
summary(aov(size ~ color + shape, data = balloon.data))
```

```
##             Df Sum Sq Mean Sq F value Pr(>F)
## color        2   11.4    5.68    0.47   0.63
## shape        1   14.8   14.78    1.22   0.28
## Residuals   20  241.4   12.07
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
## group  1       0   0.98
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
## Kruskal-Wallis chi-squared = 0.8067, df = 2, p-value = 0.6681
```

```r
# anova for comparason
summary(aov(size ~ color, data = balloon.data))
```

```
##             Df Sum Sq Mean Sq F value Pr(>F)
## color        2   11.4    5.68    0.47   0.63
## Residuals   21  256.2   12.20
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
## W = 0.8428, p-value = 0.001611
```


Post-Hoc Tests
----------------

If you get a significant result from the omnibus ANOVA, you often will want to do further post-hoc testing to look at pairwise comparisons.  There are three main ways to do this:

* Pairwise T-Tests (remember to check the assumptions of normality and equal variance when choosing a test)
* Tukey's Honest Significant Difference -- Compares all combinations
* Dunnett's Test -- Compares all values to a control


```r
# save an anova object
balloon.aov <- aov(size ~ color, data = balloon.data)
# can run a Tukey's test on this
TukeyHSD(balloon.aov)
```

```
##   Tukey multiple comparisons of means
##     95% family-wise confidence level
## 
## Fit: aov(formula = size ~ color, data = balloon.data)
## 
## $color
##               diff    lwr   upr  p adj
## green-blue -1.3787 -6.462 3.704 0.7754
## red-blue   -1.6604 -6.062 2.742 0.6151
## red-green  -0.2817 -4.684 4.120 0.9858
```

```r
# this is the same as:
library(multcomp)
```

```
## Loading required package: mvtnorm
## Loading required package: survival
## Loading required package: splines
## Loading required package: TH.data
```

```r
summary(glht(balloon.aov, linfct = mcp(color = "Tukey")))
```

```
## 
## 	 Simultaneous Tests for General Linear Hypotheses
## 
## Multiple Comparisons of Means: Tukey Contrasts
## 
## 
## Fit: aov(formula = size ~ color, data = balloon.data)
## 
## Linear Hypotheses:
##                   Estimate Std. Error t value Pr(>|t|)
## green - blue == 0   -1.379      2.017   -0.68     0.77
## red - blue == 0     -1.660      1.746   -0.95     0.61
## red - green == 0    -0.282      1.746   -0.16     0.99
## (Adjusted p values reported -- single-step method)
```

```r

# Can also use glht for Dunnett's tests define the control group to what you
# want to compare to
balloon.data$color <- relevel(balloon.data$color, ref = "blue")
# run a dunnett's test
summary(glht(balloon.aov, linfct = mcp(color = "Dunnett")))
```

```
## 
## 	 Simultaneous Tests for General Linear Hypotheses
## 
## Multiple Comparisons of Means: Dunnett Contrasts
## 
## 
## Fit: aov(formula = size ~ color, data = balloon.data)
## 
## Linear Hypotheses:
##                   Estimate Std. Error t value Pr(>|t|)
## green - blue == 0    -1.38       2.02   -0.68     0.71
## red - blue == 0      -1.66       1.75   -0.95     0.53
## (Adjusted p values reported -- single-step method)
```

```r
# default is single-step but can manually set FDR adjustments
summary(glht(balloon.aov, linfct = mcp(color = "Dunnett")), adjusted(type = "BH"))
```

```
## 
## 	 Simultaneous Tests for General Linear Hypotheses
## 
## Multiple Comparisons of Means: Dunnett Contrasts
## 
## 
## Fit: aov(formula = size ~ color, data = balloon.data)
## 
## Linear Hypotheses:
##                   Estimate Std. Error t value Pr(>|t|)
## green - blue == 0    -1.38       2.02   -0.68      0.5
## red - blue == 0      -1.66       1.75   -0.95      0.5
## (Adjusted p values reported -- BH method)
```

```r
summary(glht(balloon.aov, linfct = mcp(color = "Dunnett")), adjusted(type = "bonferroni"))
```

```
## 
## 	 Simultaneous Tests for General Linear Hypotheses
## 
## Multiple Comparisons of Means: Dunnett Contrasts
## 
## 
## Fit: aov(formula = size ~ color, data = balloon.data)
## 
## Linear Hypotheses:
##                   Estimate Std. Error t value Pr(>|t|)
## green - blue == 0    -1.38       2.02   -0.68     1.00
## red - blue == 0      -1.66       1.75   -0.95     0.71
## (Adjusted p values reported -- bonferroni method)
```

```r

# to do pairwise t tests can do
pairwise.t.test(balloon.data$size, balloon.data$color)
```

```
## 
## 	Pairwise comparisons using t tests with pooled SD 
## 
## data:  balloon.data$size and balloon.data$color 
## 
##       blue green
## green 1    -    
## red   1    1    
## 
## P value adjustment method: holm
```

```r
# default is Holm but can set to whatever you want or none
pairwise.t.test(balloon.data$size, balloon.data$color, p.adjust.method = "BH")
```

```
## 
## 	Pairwise comparisons using t tests with pooled SD 
## 
## data:  balloon.data$size and balloon.data$color 
## 
##       blue green
## green 0.75 -    
## red   0.75 0.87 
## 
## P value adjustment method: BH
```

```r
pairwise.t.test(balloon.data$size, balloon.data$color, p.adjust.method = "none")
```

```
## 
## 	Pairwise comparisons using t tests with pooled SD 
## 
## data:  balloon.data$size and balloon.data$color 
## 
##       blue green
## green 0.50 -    
## red   0.35 0.87 
## 
## P value adjustment method: none
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
## [1] splines   stats     graphics  grDevices utils     datasets  methods  
## [8] base     
## 
## other attached packages:
## [1] multcomp_1.3-3   TH.data_1.0-3    survival_2.37-7  mvtnorm_0.9-9999
## [5] car_2.0-19       knitr_1.5       
## 
## loaded via a namespace (and not attached):
##  [1] evaluate_0.5.3  formatR_0.10    grid_3.1.0      lattice_0.20-29
##  [5] MASS_7.3-31     nnet_7.3-8      sandwich_2.3-0  stringr_0.6.2  
##  [9] tools_3.1.0     zoo_1.7-11
```

