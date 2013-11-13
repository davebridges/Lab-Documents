General Statistical Methods
=============================

There are several important concepts that we will adhere to in our group.  These involve design considerations, execution considerations and analysis concerns.




Experimental Design
--------------------

Where possible, prior to performing an experiment or study perform a power analysis.  This is mainly to determine the appropriate sample sizes.  To do this, you need to know a few of things:

* Either the sample size or the difference.  The difference is provided in standard deviations.  This means that you need to know the standard deviation of your measurement in question.  It is a good idea to keep a log of these for your data, so that you can approximate what this is.  If you hope to detect a correlation you will need to know the expected correlation coefficient.
* The desired false positive rate (normally 0.05).  This is the rate at which you find a difference where there is none.  This is also known as the type I error rate.
* The desired power (normally 0.8).  This indicates that 80% of the time you will detect the effect if there is one.  This is also known as 1 minus the false negative rate or 1 minus the Type II error rate.

We use the R package **pwr** to do a power analysis <a href="http://CRAN.R-project.org/package=pwr">Champely (2012)</a>.  Here is an example:

### Pairwise Comparasons


```r
require(pwr)
false.negative.rate <- 0.05
statistical.power <- 0.8
sd <- 3.5  #this is calculated from known measurements
difference <- 3  #you hope to detect a difference 
pwr.t.test(d = difference, sig.level = false.negative.rate, power = statistical.power)
```

```
## 
##      Two-sample t test power calculation 
## 
##               n = 3.07
##               d = 3
##       sig.level = 0.05
##           power = 0.8
##     alternative = two.sided
## 
## NOTE: n is number in *each* group
```


This tells us that in order to see a difference of at least 3, with at standard devation of 3.5 we need at least **3** observations in each group.

### Correlations

The following is an example for detecting a correlation.  


```r
require(pwr)
false.negative.rate <- 0.05
statistical.power <- 0.8
correlation.coefficient <- 0.6  #note that this is the r, to get the R2 value you will have to square this result.
pwr.r.test(r = correlation.coefficient, sig.level = false.negative.rate, power = statistical.power)
```

```
## 
##      approximate correlation power calculation (arctangh transformation) 
## 
##               n = 19.22
##               r = 0.6
##       sig.level = 0.05
##           power = 0.8
##     alternative = two.sided
```


This tells us that in order to detect a correlation coefficient of at least 0.6 (or an R^2 of 0.36) you need more than **19** observations.

Corrections for Multiple Observations
--------------------------------------

The best illustration I have seen for the need for multiple observation corrections is this cartoon from XKCD (see http://xkcd.com/882/):

![Significance by XKCD.  Image is from http://imgs.xkcd.com/comics/significant.png](http://imgs.xkcd.com/comics/significant.png "Significance by XKCD.  Image is from http://imgs.xkcd.com/comics/significant.png")


Any conceptually coherent set of observations must therefore be corrected for multiple observations.  In most cases, we will use the method of Benjamini and Hochberg since our p-values are not entirely independent.  Some sample code for this is here:


```r
p.values <- c(0.023, 0.043, 0.056, 0.421, 0.012)
data.frame(unadjusted = p.values, adjusted = p.adjust(p.values, method = "BH"))
```

```
##   unadjusted adjusted
## 1      0.023   0.0575
## 2      0.043   0.0700
## 3      0.056   0.0700
## 4      0.421   0.4210
## 5      0.012   0.0575
```


References
-----------


- Stephane Champely,   (2012) pwr: Basic functions for power analysis.  [http://CRAN.R-project.org/package=pwr](http://CRAN.R-project.org/package=pwr)

