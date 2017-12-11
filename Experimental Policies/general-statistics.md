---
title: "General Statistics"
author: "Dave Bridges"
date: "May 9, 2014"
output:
  html_document:
    highlight: tango
    keep_md: yes
    number_sections: yes
    toc: yes
  pdf_document:
    highlight: tango
    keep_tex: yes
    number_sections: yes
    toc: yes
---



# General Statistical Methods

There are several important concepts that we will adhere to in our group.  These involve design considerations, execution considerations and analysis concerns.



## Experimental Design

Where possible, prior to performing an experiment or study perform a power analysis.  This is mainly to determine the appropriate sample sizes.  To do this, you need to know a few of things:

* Either the sample size or the difference.  The difference is provided in standard deviations.  This means that you need to know the standard deviation of your measurement in question.  It is a good idea to keep a log of these for your data, so that you can approximate what this is.  If you hope to detect a correlation you will need to know the expected correlation coefficient.
* The desired false positive rate (normally 0.05).  This is the rate at which you find a difference where there is none.  This is also known as the type I error rate.
* The desired power (normally 0.8).  This indicates that 80% of the time you will detect the effect if there is one.  This is also known as 1 minus the false negative rate or 1 minus the Type II error rate.

We use the R package **pwr** to do a power analysis Champely (2012).  Here is an example:

### Pairwise Comparasons


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


```
## 
##      approximate correlation power calculation (arctangh transformation) 
## 
##               n = 18.6
##               r = 0.6
##       sig.level = 0.05
##           power = 0.8
##     alternative = two.sided
```

This tells us that in order to detect a correlation coefficient of at least 0.6 (or an R^2 of 0.36) you need more than **18** observations.

Corrections for Multiple Observations
--------------------------------------

The best illustration I have seen for the need for multiple observation corrections is this cartoon from XKCD (see http://xkcd.com/882/):

![Significance by XKCD.  Image is from http://imgs.xkcd.com/comics/significant.png](http://imgs.xkcd.com/comics/significant.png "Significance by XKCD.  Image is from http://imgs.xkcd.com/comics/significant.png")


Any conceptually coherent set of observations must therefore be corrected for multiple observations.  In most cases, we will use the method of Benjamini and Hochberg since our p-values are not entirely independent.  Some sample code for this is here:


```
##   unadjusted adjusted
## 1      0.023   0.0575
## 2      0.043   0.0700
## 3      0.056   0.0700
## 4      0.421   0.4210
## 5      0.012   0.0575
```

# Session Information


```r
sessionInfo()
```

```
## R version 3.4.2 (2017-09-28)
## Platform: x86_64-apple-darwin15.6.0 (64-bit)
## Running under: macOS High Sierra 10.13.1
## 
## Matrix products: default
## BLAS: /Library/Frameworks/R.framework/Versions/3.4/Resources/lib/libRblas.0.dylib
## LAPACK: /Library/Frameworks/R.framework/Versions/3.4/Resources/lib/libRlapack.dylib
## 
## locale:
## [1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
## [1] pwr_1.2-1           knitcitations_1.0.9 dplyr_0.7.4        
## [4] tidyr_0.7.2         knitr_1.17         
## 
## loaded via a namespace (and not attached):
##  [1] Rcpp_0.12.14       xml2_1.1.1         bindr_0.1         
##  [4] magrittr_1.5       R6_2.2.2           rlang_0.1.4       
##  [7] bibtex_0.4.2       plyr_1.8.4         httr_1.3.1        
## [10] stringr_1.2.0      tools_3.4.2        htmltools_0.3.6   
## [13] yaml_2.1.15        rprojroot_1.2      digest_0.6.12     
## [16] assertthat_0.2.0   tibble_1.3.4       bindrcpp_0.2      
## [19] purrr_0.2.4        RefManageR_0.14.20 glue_1.2.0        
## [22] evaluate_0.10.1    rmarkdown_1.8      stringi_1.1.6     
## [25] compiler_3.4.2     backports_1.1.1    lubridate_1.7.1   
## [28] jsonlite_1.5       pkgconfig_2.0.1
```

# References

<a name=bib-pwr></a>[[1]](#cite-pwr) S. Champely. _pwr: Basic
functions for power analysis_. R package version 1.1.1. 2012. URL:
[http://CRAN.R-project.org/package=pwr](http://CRAN.R-project.org/package=pwr).
