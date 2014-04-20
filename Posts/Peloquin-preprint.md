The Story Behind the (Pre-) Paper
==================================

We recently posted a preprint to bioRxiv called **Weight Loss in Response to Food Deprivation Predicts The Extent of Diet Induced Obesity in C57BL/6J Mice**, as well as to a standard peer reviewed journal.  I discussed why we chose to publish the preprint over on my [old blog](http://dave-bridges.blogspot.com/2014/04/preprints-trying-something-new-in.html).  The raw data and revision history for the analysis is in a public [GitHub reposoitory](http://bridgeslab.github.io/PredictorsDietInducedObesity/) and the paper can be found [here](http://dx.doi.org/10.1101/004283).  We would very much like to recieve feedback on this work, so please do so either here, or on the preprint site.

What Were We Looking At?
---------------------------
Obesity is a major health problem, maybe the most dramatic alteration in human physiology in a concerted time period ever.  Genetics has told us that 40-70% of obesity is in some way heritable, but outside of a limited number of cases of monogenic obesity disorders, the susceptibility to obesity is hard to predict.  We wanted to know, in our model organism of obesity (the inbred C57BL/6J mouse fed a high fat diet) how well we could predict obesity.

What Did We Do?
---------------------

We collected several cohorts of data on mouse weights, all sourced from the [genetic stability program](http://jaxmice.jax.org/genetichealth/stability.html) at Jackson labs.  This program is engineered to reduce genetic drift in these inbred lines.  These mice are not genetically identical (the mutation rate in mice is quite [high](http://mouseclique.jax.org/dear-jaxy-how-rapid-is-genetic-drift-in-mouse-colonies/)), but they are as close as practically posssible.

Within this background, where the gentic and environmental differences are minimized we wanted to ask the question, how predictable is weight gain.  The answer is, its not that predictable!  There was a very broad rangle of endpoint weights after diet induced obesity, moreso than control diet fed mice, or genetically obese mice.

We then looked back at samples and data we had collected before the mice started the diet to try to figure out retrospectively what best predicted their weight gain.  Surprisingly, their glucose homeostasis, metabolic hormone levels and initial body weights were not predictive of their eventual weight gain at all.  

<a href="https://raw.githubusercontent.com/BridgesLab/PredictorsDietInducedObesity/master/scripts/figure/gain-plot-pct.png">
<div style="float: right">
    <img src="https://raw.githubusercontent.com/BridgesLab/PredictorsDietInducedObesity/master/scripts/figure/gain-plot-pct.png" alt="weight-gain-correlation" title="Effects of fasting induced weight loss pre-diet on weight gain during the diet"/ style="width: 200px;">
</div>
</a>

What we did find?
------------------

We fasted mice before the diet, in order to collect fasting blood samples.   Even though those blood samples had no predictive utility, we also noted their body weights both before and after fasting.  This fasting induced weight loss turned out to be highly predictive of their eventual weight gain on diet, accounting for more than 35% of the variation between mice and almost 70% when we include the effects of diet.  This implies that fasting induced weight loss is highly predictive of diet-induced obesity in these inbred mice.

What Does it Mean?
---------------------

Figuring out who is susceptible to diet induced obesity and why is really important.  This gives us some interesting new directions, since we now have pretty good evidence that there is something different between mice that have a large fasting response (followed by low diet induced obesity) and those mice that have a blunted fasting response (which is then followed by a lack of weight gain).  Trying to figure out the nature of these differences will be the next big step in this project.
