---
title: "Subsetting and Sorting"
author: "Krystella Rattan"
date: "3/4/2022"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## 1. Review of Subsetting and Sorting

Create a data frame in R with three variables (columns), where the elements in 
each column are randomized and column 2 "var2" contains two missing numbers

```{r}

set.seed(13435)
X <- data.frame("var1"=sample(1:5), "var2"=sample(6:10), "var3"=sample(11:15))
X <- X[sample(1:5),]; X$var2[c(1,3)] = NA
X

```


#### Understanding the **set.seed()** function

Source: https://statisticsglobe.com/why-and-how-to-set-a-random-seed-in-r

set.seed() produces a "random" number (really a pseudorandom number that 
simulates randomness) which, when used, allows us to make the output of our 
code reproducible.

To understand how the set.seed() function works, see the example below:

```{r}

# Generate some random numbers using the rnorm function

rnorm(5)

# [1]  0.3706279  0.5202165 -0.7505320  0.8168998 -0.8863575

# Execute the same code again to generate some random numbers again
rnorm(5)

# [1] -0.3315776  1.1207127  0.2987237  0.7796219  1.4557851

# Notice how the same code produced a different set of random numbers. This can make it difficult to retrace the steps of code the programmer has made.

# For this reason, it is considered best practice for researchers to set a random seed at the beginning of R scripts that involve a random process.

# Set a seed for reproducibility and generate some random numbers again

set.seed(12345)
rnorm(5)

# [1]  0.5855288  0.7094660 -0.1093033 -0.4534972  0.6058875

# Set the same seed again and run the same code

set.seed(12345)
rnorm(5)

# [1]  0.5855288  0.7094660 -0.1093033 -0.4534972  0.6058875

# Note that using the given seed reproduced the same set of random numbers!

```

#### Subsetting

```{r}
# For the data frame, X, created:
X

#    var1 var2 var3
# 5    2   NA   11
# 4    4   10   12
# 1    3   NA   14
# 2    1    7   15
# 3    5    6   13

# Subset the first column of X:

X[,1]
# [1] 2 4 3 1 5

# Subset a column by variable name:

X[,"var1"]
# [1] 2 4 3 1 5

# Subset by rows and columns at the same time - e.g. first 2 rows of X in the column "var2"

X[1:2, "var2"]
# [1] NA 10

# Subset using logical statements:
# 1. Subset all the rows of X where variable 1 is less than or equal to 3 AND **&** where variable 3 is greater than 11

X[(X$var1 <= 3 & X$var3 > 11),]
  
#    var1  var2  var3
# 1   3     NA    14
# 2   1     7     15

# 2. Subset all the rows of X where variable 1 is less than 3 OR **|** variable 3 is greater than 15

X[(X$var1 <= 3 | X$var3 > 15),]

#    var1  var2  var3
# 5   2     NA    11
# 1   3     NA    14
# 2   1     7     15

# Subset to produce only those rows without missing values using **which**

X[which(X$var2 > 8),]

#    var1  var2  var3
# 4   4     10    12


```


#### Sorting

```{r}
# Sort var1 in **ascending** order:

sort(X$var1)

# [1] 1 2 3 4 5

# Sort var1 in **descending** order:

sort(X$var1, decreasing = TRUE)

# [1] 5 4 3 2 1

# Sort var2 where the missing values are placed last

sort(X$var2, na.last = TRUE)

# [1]  6  7 10 NA NA

```


#### Ordering

```{r}
# Order a data frame by a particular variable

# Re-order the rows of X such that var1 is in ascending order

X[order(X$var1),]

#    var1  var2  var3
# 2   1     7     15
# 5   2     NA    11
# 1   3     NA    14
# 4   4     10    12
# 3   5     6     13

# It is possible to order multiple rows at the same time 

X[order(X$var1, X$var3),]

# In the case of the function above, var1 is ordered first, then, if there were multiple identical numbers in var1, var3 would be ordered for those elements.

```


#### Ordering with plyr

```{r}
library(plyr)
arrange(X, var1)

#    var1 var2 var3
# 1    1    7   15
# 2    2   NA   11
# 3    3   NA   14
# 4    4   10   12
# 5    5    6   13


arrange(X, desc(var1))

#    var1 var2 var3
# 1    5    6   13
# 2    4   10   12
# 3    3   NA   14
# 4    2   NA   11
# 5    1    7   15

```


#### Add rows and columns

```{r}
X$var4 <- rnorm(5)
X

#    var1 var2 var3       var4
# 5    2   NA   11    -0.4150458
# 4    4   10   12     2.5437602
# 1    3   NA   14     1.5545298
# 2    1    7   15    -0.6192328
# 3    5    6   13    -0.9261035


# This can also be done using the **cbind** function to "bind" a new column to the data frame X

Y <- cbind(X,rnorm(5))
Y

#     var1 var2 var3       var4        rnorm(5)
# 5    2    NA   11    -0.4150458   -0.66549949
# 4    4    10   12     2.5437602   -0.02166735
# 1    3    NA   14     1.5545298   -0.17411953
# 2    1    7    15    -0.6192328    0.23900438
# 3    5    6    13    -0.9261035   -1.83245959

# Can also be used to designate position

y <- cbind(rnorm(5),X)
y

#      rnorm(5)  var1 var2 var3       var4
# 5 -0.03718739    2   NA   11   -0.4150458
# 4 -0.44051699    4   10   12    2.5437602
# 1 -1.44826359    3   NA   14    1.5545298
# 2 -0.51824571    1    7   15   -0.6192328
# 3  0.75852718    5    6   13   -0.9261035


# Can also be used to bind rows using rbind

Z <- rbind(X,rnorm(4))
Z

#      var1     var2       var3       var4
# 5 2.0000000       NA 11.0000000 -0.4150458
# 4 4.0000000 10.00000 12.0000000  2.5437602
# 1 3.0000000       NA 14.0000000  1.5545298
# 2 1.0000000  7.00000 15.0000000 -0.6192328
# 3 5.0000000  6.00000 13.0000000 -0.9261035
# 6 0.2849353 -1.53784 -0.3652188 -0.4338298

```





