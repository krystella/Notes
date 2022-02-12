---
title: "Learning Log"
author: "Krystella"
date: "2/10/2022"
output:
  pdf_document: default
  html_document: default
---

## 10 February 2022

### Technical

* Connect to, and disconnect from, a database

* Passing commands to a database through R

* Determining what tables are present in a database and how many there are

* Determining the dimensions of a table: columns and rows

* Extracting or subsetting data from the tables
  
### Other

* If experiencing problems with install.packages(), try running in the console window or manually installing under the Packages tab

* To show code in a markdown file but NOT results as a default:
  knitr::opts_chunk$set(echo = T, results = "hide")

* Individual code chunks can be modified to show results if needed:
  {r analysis, results="markup"}

* Sometimes warnings or error messages appear that do not affect the code. These     may be "hidden" in the markdown file by using:
  knitr::opts_chunk$set(warning = FALSE, message = FALSE)
  
* To hide warnings in individual code chunks:
  suppressWarnings(expr, class="warning"), where expr is the statement for which     you want to hide warnings
  
* For error messages prompting to "close resultset", try dbClearResult() following a dbGetQuery or dbSendQuery command

### Some useful chunk options

* eval = TRUE or FALSE : whether to evaluate a code chunk
* echo = TRUE or FALSE : whether to include the code in the output document
* results = "hide", "markup" : whether to include code results in the output document
* warning = TRUE or FALSE 
  message = TRUE or FALSE
  error = TRUE or FALSE
      : whether to display warnings, messages, and errors
* include = TRUE or FALSE :  whether to display whole code chunk in output document
  include = FALSE can replace(echo=FALSE, results="hide", warning=FALSE, message=FALSE)

###

## 11 February 2022

* HDF5 - Hieracharchal Data Format
  ** create h5 files **h5createfile()**
  ** create groups and subgroups within the file **h5createGroup*
  ** write matrices and arrays to groups using **h5write()**
  ** read h5 files using **h5read()**

## 12 February 2022

* Git command **git mv oldfilename newfilename** to change file names in the repository

* Different ways to get data from webpages: 
  ** parsing with XML (example didn't work - Error: failed to load external entity. Need to revisit); 
  ** using htmlTreeParse 
  ** command **xpathSApply()** to extract data of interest only
  ** see for more info: http://www.omegahat.net/RSXML/shortIntro.pdf
  ** command **GET** to extract from httr package
  