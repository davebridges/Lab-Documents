The Story Behind the Paper: The role of TORC1 in muscle development in Drosophila
===================================================================================

<span class="Z3988" title="ctx_ver=Z39.88-2004&rft_val_fmt=info%3Aofi%2Ffmt%3Akev%3Amtx%3Ajournal&rft.jtitle=Scientific+Reports&rft_id=info%3Adoi%2F10.1038%2Fsrep09676&rfr_id=info%3Asid%2Fresearchblogging.org&rft.atitle=The+role+of+TORC1+in+muscle+development+in+Drosophila&rft.issn=2045-2322&rft.date=2015&rft.volume=5&rft.issue=&rft.spage=9676&rft.epage=&rft.artnum=http%3A%2F%2Fwww.nature.com%2Fdoifinder%2F10.1038%2Fsrep09676&rft.au=Hatfield%2C+I.&rft.au=Harvey%2C+I.&rft.au=Yates%2C+E.&rft.au=Redd%2C+J.&rft.au=Reiter%2C+L.&rft.au=Bridges%2C+D.&rfe_dat=bpr3.included=1;bpr3.tags=Biology%2CMedicine%2CDevelopmental+Biology%2C+Aging%2C+Metabolism">Hatfield, I., Harvey, I., Yates, E., Redd, J., Reiter, L., & Bridges, D. (2015). The role of TORC1 in muscle development in Drosophila <span style="font-style: italic;">Scientific Reports, 5</span> DOI: <a rev="review" href="http://dx.doi.org/10.1038/srep09676">10.1038/srep09676</a></span>

This work was recently published in Scientific Reports and can be freely accessed [here](http://dx.doi.org/10.1038/srep09676 "The role of TORC1 in muscle development in Drosophila").  Earlier preprints were uploaded to [bioRxiv](http://dx.doi.org/10.1101/010991).  Izzy Hatfield, an undergraduate who was the first person hired in the lab did most of this work, with Innocence Harvey, a graduate student doing most of the cell culture studies during her rotation project.  All the fly work was done in collaboration with the Reiter lab, and the UTHSC Drosophila Transgenic Core.  The data and code for this project are available at [Github](http://bridgeslab.github.io/DrosophilaMuscleFunction/) and are archived at [Zenodo](http://dx.doi.org/10.5281/zenodo.16241 "Dataset for Drosophila Muscle Function Studies")

<span style="float: left; padding: 5px;"><a href="http://www.researchblogging.org"><img alt="ResearchBlogging.org" src="http://www.researchblogging.org/public/citation_icons/rb2_large_gray.png" style="border:0;"/></a></span>


What Were We Looking At?
---------------------------

Initially, we wanted to know what the role of mTORC1 is in muscle and how it affected lifespan and muscle function.  Really elegant work in flies, worms, yeast and mice had shown that inhibition of mTORC1 increased lifespan, but many of these studies reduced mTORC1 function in many tissues at once.  We wanted to know if muscle mTORC1 specifically was important in aging and muscle function.  Our plan was to reduce mTORC1 activity (by knocking down *Raptor*, its essential subunit) using the UAS-GAL4 system in flies, specifically in muscle tissues.

However very early on, we figured out our approach wasn't going to work, for one simple reason, we didn't get any *Raptor* knockdown flies in our crosses.  This paper is our reporting of what we think was going on.


What Did We Do?
----------------

We used several different muscle fly drivers to knockdown *Raptor* and reduce mTORC1 activity.  We also activated mTORC1 activity by knocking down the negative regulator *Tsc1*.  Because one of our possible interpretations was that myocyte differentiation was defective, we also used a mouse C2C12 cell line to examine the role of mTORC1 in myocyte differentiation.

What we did find?
------------------

The first major finding, was that the knockdown of *Raptor* in GAL4 drivers that were expressed early in the differentiation process (24B, C179 and Mef2) was lethal or nearly lethal.  The flies looked normal up to their eclosure from their pupal cases, but never emerged.  If we "helped" them emerge by manually opening their cases they could crawl out but they were very weak and died very early (see figure to the right <img style="float: right" src="http://www.nature.com/srep/2015/150407/srep09676/thumbs_article/srep09676-f4.jpg" alt="*Raptor* knockdown flies fail to eclose and were weaker if assisted">
).  We thought that this meant the flies had impaired muscular development.

We went back and did some characterization of differentiation in a mouse C2C12 line and found that while *Mef2c* was expressed early in differentiation, myosin heavy chain *Mhc* was expressed quite late.  We then went back and used the muscle *Mhc-GAL4* strain and repeated the *Raptor* knockdown experiments.  In these cases, the flies could emerge in normal numbers and had normal initial muscle function, and but still died earlier than their wild-type controls.  In this case though, it wasn't shortly after eclosure, but was 10-20% less than normal lifespans.


What Does it Mean?
---------------------

It had been known for quite some time that mTORC1 was important for muscle differentiation, but the mouse *Rptor* knockout model (see [Bentzinger *et al.* 2008](http://dx.doi.org/10.1016/j.cmet.2008.10.002 "Skeletal muscle-specific ablation of raptor, but not of rictor, causes metabolic changes and results in muscle dystrophy.")) used a Cre that did not affect differentiation.  Therefore this data shows that *Raptor* and mTORC1 activity are required for both muscle development *in vivo* but then after that for maintenance of muscle function.  We also think that this suggests that inhibition of mTORC1 function in muscle is not the mechanism by which rapamycin extends lifespan. 
