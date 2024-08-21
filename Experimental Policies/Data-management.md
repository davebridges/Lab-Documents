---
title: "Data Storage and Management"
author: "Dave Bridges"
date: "May 15, 2017, Last Updated 2024-08-11"
output:
  html_document:
    toc: yes
    keep_md: yes
    toc_float: true
    highlight: tango
bibliography: references.bib    
---




Most of the data we generate in the lab is numeric data, so it is very important that we are consistent in the way we record and structure our saved data.
Below are some best practices on how to save and store your data in our group.

# Raw Data

Data can be *raw* or *processed*.  It is the best practice to save files in the most raw format possible, preferably a **csv file**.  As an example, a datafile might contain absorbance readings, that are converted by a standard curve into protein amounts.  The raw data is the absorbances, the protein amounds would be processed data.  Raw data is the most valuable and should be saved in that form.  Sometimes you may have data that you know you are going to exclude.  Include this in the raw data, and exclude it during the analysis with a note describing why.

## What Should Data Look Like?

Data shoiuld be **tidy**.  This means that there should be continuous rows which contain columns that describe the conditions related to that data.  If there are missing values, leave them as blank and do not enter as zero.  For more details about tidy data read this paper: @Wickham2014.  

## What Format Should I Save Raw Data In?

The preferred format is a CSV file.  This is a *non-proprietary* open format.  This is important because it is the smallest file size and allows anyone to view/open your data without needing any proprietary software like Microsoft Excel.  This may seem like a minor concern, but 20 years from now someone may want to open your data and Excel may be difficult to find.  You can still use Excel if you prefer to record raw data, but make sure you Save it as a CSV file.  To make a CSV file from Microsoft Excel click on File -> Save As... then under File Format select CSV UTF-8 from the dropdown list.  UTF-8 is the character encoding.  Excel may prompt you to see if you are sure.  It is also a good practice to make raw data files **read only**.  To do this on an Apple computer, right click on it, go to Get Info then check the "Locked" checkbox.  This prevents you from inadvertently changing raw data.

## Where Do I Store Data?

**All data** needs to be stored on our shared drives. Most of this data should be on the U-drive (except for human data or data on GreatLakes, see below).  These folders are secure, and backed up.  You can't forget it on the bus, or drop it in the sewer by accident.  Data should only be on USB sticks when you are moving between computers and should only be on your laptop when you do not have internet connectivity.  You can organize your folder anyway you like, but some good subfolders might be:

* Data
* Writing
* Diagrams

Keep experimental results in the Data folder, which you should organize into subfolders as appropriate.  When you do an experiment, create a folder describing that experiment (for example Effects of Rapamycin on Glycolysis).  Inside that folder create a new subfolder for each replicate of that experiment.  Everything about that replicate should be in the subfolder(s) including raw and processed data and analysis files.

**Human data** must be stored in HIPAA compliant folders.  These are restricted to those individuals with IRB access to those folders.  For data from Michigan Precision Health, this data is on Armis2, the secure storage system.  

### What About Other Experimental Details?

Oftentimes a csv file is insufficient to totally describe what was done, and other details about the experiment.  For each experiment create a file that describes the raw data.  This should be contained in the  Rmd file that analyses the data, but if this is not possible, create a README.md file that describes the files and their contents.  Think of this as an online version of your experimental notes and should describe what datafiles were generated, what the columns indicate and anything you may need to know about analysing the data. There are more details about how to structure your Rmarkdown files below.

## How Do We Convert Raw Data into Graphs or Statistical Outputs?

While it may seem convenient to have the raw data, and analysed data all in the same file, we prefer instead to have an analysis layer between raw data and the output.  For our lab, this is done using Rmarkdown or [Quarto](https://quarto.org/docs/computations/r.html) files (such as this one).  This is a script file that will combine normal writing (in markdown format, see https://daringfireball.net/projects/markdown/syntax for syntax) and all the relevant analysis steps.  This is useful for many reasons, the main one is reproducibility.  By using a script, it is very clear how you get from raw data to your eventual outputs, and can be re-run if you want to add a new graph or test easily.  Scripts can also be easily copied and modified to a similar experiment.  For example if you are repeating an experiment the Rmarkdown file could be copied over to the new experiment folder (with new raw data).  Think of the raw data as a chunk of steak, the script as a meat grinder and the processed data as a hamburger.  Remember, its impossible to make a steak from a hamburger, but its easy to go the other direction.

# Processed Data

Processed data could be calculated data, or figures.  These are created by the Rmarkdown or Quarto file, and are over-written every time the analysis is re-run.

## What Sections Should My Rmarkdown/Quarto Files Contain?

There is a lot of details about the syntax of your Rmarkdown file here: 

Generally your Rmarkdown files should have these sections.  If you want a template as a starting point try using this one https://raw.githubusercontent.com/BridgesLab/Lab-Documents/master/Experimental%20Policies/Rmarkdown-template.Rmd

**Purpose**: Make a brief description of why you did the experiment, ideally explicitly stating your hypothesis.

**Experimental Details**: Link to the protocol used (permalink preferred) for the experiment and include any notes relevant to your analysis.  This might include specifics not in the general protocol such as cell lines, treatment doses etc.

**Raw Data**: Describe your raw data files, including what the columns mean (and what units they are in).

**Analysis**: Describing the analysis as you intersperse code chunks

**Interpretation**: A brief summary of what the interpretation of these results were

**Session Information**: A block with the sessionInfo() command printing out 

**References**: If needed, using Rmarkdown citation tools (see this link for more information: http://rmarkdown.rstudio.com/authoring_bibliographies_and_citations.html)

### How Do I Load Raw Data into R

The preferred package for reading csv files is called **readr**.  This is a useful package that can read in a variety of formats including CSV.  The syntax is fairly simple:


``` r
library(readr) #loads the readr package
filename <- 'testfile.csv' #make this a separate line, you can use any variable you want
data <- read_csv(filename)
```

```
## Rows: 60 Columns: 3
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): supp
## dbl (2): len, dose
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

As shown above, readr tries to guess the datatype in each column as above.  This can be over-ridden by explicitly stating column format in the read_csv command.  Think carefully about whether data should be an integer, character or factor.  For grouping factors, its best to explicitly state levels and reference values immediately after the data import.  More details about loading data can be found here: http://r4ds.had.co.nz/data-import.html.

Immediately after you load your data in the Rmarkdown file have a block of text such as this:

```{eval=FALSE, echo=TRUE}
These data can be found in /Users/davebrid/Documents/GitHub/Lab-Documents/Experimental Policies in a file named testfile.csv.  This script was most recently updated on Sun Aug 11 15:03:55 2024.
```

This will render as this:

These data can be found in /Users/davebrid/Documents/GitHub/Lab-Documents/Experimental Policies in a file named testfile.csv.  This script was most recently updated on Sun Aug 11 15:03:55 2024.

This clearly indicates where to find the file, and when it was most recently updated.  This is helpful for when printing out your Rmarkdown files and finding the data from your notes on the data server.

# References


