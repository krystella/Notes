---
title: "Creating New Variables"
author: "Krystella Rattan"
date: "3/6/2022"
output:
  pdf_document: default
  html_document: default
subtitle: Getting and Cleaning Data - Week 3
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE, eval = FALSE)
```

Using the dataset 'restData' downloaded in previous lecture

### Creating Sequences

Sequences are often used to index different operations that you're going to be
doing on data

"Indexing means selecting a subset of the elements in order to use them in
further analysis or possibly change them" 
https://thomasleeper.com/Rcourse/Tutorials/vectorindexing.html


The function used to sequence in R is **seq()**

e.g. The following example code creates a sequence where the minimum value is 1,
the maximum value is 10, and the operator **by=** indicates how much each new 
value should be increased by:

```{r}
s1 <- seq(1, 10, by=2); s1

```


A sequence can also be created by using the operator **length=** to indicate the
length of the resulting vector

```{r}
s2 <- seq(1, 10, length = 3); s2

```

seq(along=x) will create a vector of the same length as x, but with consecutive 
indices that you can use for looping or accessing subsets of data sets.

```{r}
x <- c(1,3,8,25,100); seq(along = x)

```


### Subsetting variables

Creating a variable which indicates which subset another variable comes from:

```{r}
restData$nearMe = restData$nghbrhd %in% c("Roland Park", "Homeland")

table(restData$nearMe)

```

Here, the %in% command allows you to find only the restaurants which are in those
two specified neighbourhoods.

### Creating binary values

```{r}

restData$zipWrong = ifelse(restData$zipcode < 0, TRUE, FALSE)

table(restData$zipWrong, restData$zipcode < 0)

```

The ifesle command can be used to find the cases where we know the zipcode is 
wrong.

To apply the ifelse command, first set a condition - in this case, 
restData$zipcode < 0.

If the zipcode is less than zero then the zipcode must be wrong, so it will 
return 'TRUE', if not then it will return 'FALSE'


### Creating categorical variables

```{r}
restData$zipGroups = cut(restData$zipcode, breaks=quantile(restData$zipcode))

table(restData$zipGroups)

```

The cut() function breaks a quantitative variable up into a categorical 
variable

Easier cutting:

```{r}
library(Hmisc)

restData$zipGroups = cut2(restData$zipcode, g=4) # breaks up into 4 groups

table(restData$zipGroups)

```


### Creating factor variables

Can change, say, an integer variable into a factor variable

```{r}

restData$zcf <- factor(restData$zipcode)

restData$zcf[1:10]

# Check
class(restData$zcf)

```

Levels of factor variables

```{r}

yesno <- sample(c("yes", "no"), size = 10, replace = TRUE)
yesnofac = factor(yesno, levels = c("yes", "no"))

relevel(yesnofac, ref = "yes")

as.numeric(yesnofac)

```


### Using the mutate function

Use the mutate() function to create a new version of a variable and simultaneously
add it to a new dataset

```{r}

library(Hmisc); library(plyr)

restData2 = mutate(restData, zipGroups=cut2(zipcode, g=4))

table(restData2$zipGroups)

```


### Common transformations

```{r}

abs(x)  # absolute value

sqrt(x)  # square root

ceiling(x) # ceiling(3.475) is 4

floor(x) # floor(3.475) is 3

round(x, digits = n) # round(3.475, digits=2) is 3.48
 
signif(x, digits=n) # signif(3.475, digits=2) is 3.5

cos(x) ; sin(x), # etc

log(x) # natural logarithm

log2(x) ; log10(x), # other common logs

exp(x) # exponentiating x

```

















