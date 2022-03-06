---
title: "Summarizing Data"
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

## Summarizing Data

```{r}
knitr::opts_chunk$set(echo = TRUE, eval = FALSE)

if(!file.exists("./data")){dir.create("./data")}

    # If file does not exist, create directory "./data"

fileUrl <- 
  "https://data.baltimorecity.gov/api/views/k5ry-ef3g/rows.csv?accessType=DOWNLOAD"

download.file(fileUrl, destfile = "./data/restaurants.csv")

```


Assign the csv to an R object called 'restData'

```{r}

restData <- read.csv("./data/restaurants.csv")

```


#### Reading the first or last parts of the data frame only

Use the 'head()' function to read the first part of the data frame. n=3 
indicates how many rows  to read.

```{r}

head(restData, n=3)

```


Use the 'tail()' function to read the last part of the data frame. n=3 
indicates how many rows from the bottom to read.

```{r}

tail(restData, n=3)

```


#### Summarising the data

Quickly get a summary of all the data in the data frame using the function
'summary()'

```{r}

summary(restData)

```


#### More in-depth information

Get more in-depth information using the function 'str()', which provides info 
such as how many observations and variables there are, and the types of 
variables, etc.

```{r}

str(restData)

```


#### Quantiles of quantitative variables

Use 'quantile()' to look at the variability of the variables, e.g. the smallest,
biggest, or median numbers, percentiles, etc.
    
```{r}

quantile(restData$cncldst, na.rm = TRUE)

quantile(restData$cncldst, probs = c(0.5, 0.75,0.9))

```


#### Making tables

Use 'table() to make tables using specific variables.
   
```{r}

table(restData$zipcode, useNA = "ifany")

```

Use ' useNA="ifany" ' to determine if/how many missing values there are. By 
default, the table() function does not show missing values


Creating a 2-dimensional table, e.g. a table showing data by zipcode and district:

```{r}

table(restData$cncldst, restData$zipcode)

```


#### Check for missing values

Use the 'is.na()' function to determine if there are any missing values. is.na()
will return a 1 if there is a missing value or a 0 if there are no missing values.
Use function 'sum()' to determine how many missing values are present.

```{r}

sum(is.na(restData$cncldst))

```


Use 'any(is.na())' to determine if any of the values are missing. This functions
returns a TRUE or FALSE output
    
```{r}

any(is.na(restData$cncldst))

```


Use all() to determine if every value satisfies the condition
    
```{r}

all(restData$zipcode > 0)

```


#### Row and column sums

Use 'colSums()' to determine the sums of each column. In this example, 
is.na(restData) will return whether there are any missing values in the dataset,
colSums can be applied to this function to determine the number of missing values
in each column.
    
```{r}

colSums(is.na(restData))

```


Applying all() to that function returns a TRUE or FALSE output whether there are
any missing functions in the dataset or not

```{r}

all(colSums(is.na(restData))==0)

```


#### Values with specific characteristics

Use the %in% operator to find specific bits of information, e.g. the following 
line returns how many restaurants there are in the specific zipcode 21212:
    
```{r}

table(restData$zipcode %in% c("21212"))

table(restData$zipcode %in% c("21212", "21213"))

```

Subset a dataset: e.g. Suppose you want to get the rows of restaurants that 
belong to either 21212 or 21213:
    
```{r}

restData[restData$zipcode %in% c("21212", "21213"),]

```


#### Cross tabs

A cross tab is a way of summarizing only the data you are interested in, in an 
easy-to-read format such as a dataframe, and in a way that would allow you to 
see what relationships exists between these variables.
    
```{r}

data("UCBAdmissions")
DF = as.data.frame(UCBAdmissions)
summary(DF)

    # The summary shows 4 variables: Admit, Gender, Dept, Frequency

    # In the following example code, Freq is the information that you'd actually
    # want to see in the table, and that data is further broken down by Gender 
    # and whether they were admitted or not

xt <- xtabs(Freq ~ Gender + Admit, data=DF)
xt

```


#### Flat tables

```{r}

warpbreaks$replicate <- rep(1:9, len = 54)
xt = xtabs(breaks ~., data=warpbreaks)
xt

```


To summarise that data in a smaller, more compact form, create a 'flat table' 
by applying the command ftable() to the crosspath previously created.

```{r}

ftable(xt)

```


#### Size of a dataset

```{r}
object.size(restData)

print(object.size(restData), units = "Mb")

```













