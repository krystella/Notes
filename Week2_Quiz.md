---
title: "Week 2 Quiz"
author: "Krystella Rattan"
date: "2/14/2022"
output:
  pdf_document: default
  html_document: default
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```


## Quiz Q.1

```{r}
# install.packages("jsonlite")
library(jsonlite)

# install.packages("httpuv")
library(httpuv)

# install.packages("httr")
library(httr)

```

### 1. Find OAuth settings for github:

http://developer.github.com/v3/oauth/

```{r}
oauth_endpoints("github")

# ?oauth_endpoints for more info:
# Some examples of oauth_endpoints: linkedin, twitter, vimeo, google, facebook, 
# github, azure)

```


### 2. To make your own application, register at

https://github.com/settings/developers. Use any URL for the homepage URL

(http://github.com is fine) and  http://localhost:1410 as the callback url

Replace your key and secret below.

```{r}
myapp <- oauth_app(appname = "JH_Course3_Week2_Quiz",
  key = "<Client Key>",
  secret = "<Client Secret>"
)

```

### 3. Get OAuth credentials

```{r}
github_token <- oauth2.0_token(oauth_endpoints("github"), myapp)

```

### 4. Use API

```{r}
knitr::opts_chunk$set(echo = TRUE, results = "hide")

gtoken <- config(token = github_token)
req <- GET("https://api.github.com/repos/jtleek/datasharing", gtoken)
stop_for_status(req)
content(req)

```

### OR:

```{r}
knitr::opts_chunk$set(echo = TRUE, eval=FALSE, results = "hide")

req <- with_config(gtoken,
                   GET("https://api.github.com/repos/jtleek/datasharing"))
stop_for_status(req)

leek_repos <- content(req)

```

```{r}


# Extract content from a request
json1 = content(req)

# Convert to a data.frame
gitDF = jsonlite::fromJSON(jsonlite::toJSON(json1))

#Subset data.frame
gitDF[gitDF$fullname == "jtleek/datasharing", "created_at"]

```

Ans.: 2013-11-07T13:25:07Z


## Q2. The sqldf package allows for execution of SQL commands on R data frames. 
## We will use the sqldf package to practice the queries we might send with the 
## dbSendQuery command in RMySQL. 

Download the American Community Survey data and load it into an R object called 

```{r}
# First, install and load the sqldf package

install.packages("sqldf")
library(sqldf)

# Prompted to install the following packages

library(gsubfn)
library(proto)
library(RSQLite)

```

```{r}
# Download the ACS data (given url for .csv file) and assign it to an R object 
# called acs

# Check current directory
getwd()

# If necessary, set your directory
setwd("E:/Work/Data Science/R/3 Getting and Cleaning Data/Week 2")

# Check current directory to confirm change
getwd()

url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06pid.csv"

download.file(url, 
destfile = "E:/Work/Data Science/R/3 Getting and Cleaning Data/Week 2/acs.csv")

# Note that destfile should be a file name and not a folder or directory. 
# Doing so results in error "permission denied"

```

```{r}
# Assign the downloaded file to an R object called acs

acs <- read.csv("acs.csv")

# Use head() function to get the first parts of the dataframe or table

head(acs)

```

Which of the following commands will select only the data for the probability 
weights pwgtp1 with ages less than 50:
a. sqldf ("select pwgtp1 from acs where AGEP<50")
b. sqldf ("select * from acs where AGEP<50")
c. sqldf ("select * from acs")
d. sqldf ("select pwgtp1 from acs")

```{r}
sqldf ("select pwgtp1 from acs where AGEP<50")

```
```{r}

sqldf ("select * from acs where AGEP<50")

```
```{r}

sqldf ("select * from acs")

```
```{r}

sqldf ("select pwgtp1 from acs")

```

Ans.: A


## Q.3 Using the same data frame you created in the previous problem, what is 
## the equivalent function to unique(acs$AGEP)

```{r}
# This can be done by comparing the result of the function unique(acs$AGEP) with
# each of the options listed, OR by using the command function 'identical'.

# The function 'identical' will allow for comparison of large datasets that may 
# be difficult to compare manually.

U <- unique(acs$AGEP)

A <- sqldf("select AGEP where unique from acs")
identical(U, A)
```
```{r}
B <- sqldf("select distinct AGEP from acs")

identical(U, B$AGEP)

```
```{r}
C <- sqldf("select unique AGEP from acs")
identical(U, C$AGEP)

```
```{r}
D <- sqldf("select distinct pwgtp1 from acs")
identical(U, D$AGEP)

```

Ans.: B

Note: the format of the results matter when using the function **identical()**. 
The function unique(acs$AGEP) returns a result in 'R Console', whereas, B 
produced a dataframe. Although the results matched correctly, **identical()** 
returned "FALSE" since the format was not the same. Adjusting the format to read
as 'R Console' (B$AGEP) returned a positive match "TRUE"


## Q.4 How many characters are in the 10th, 20th, 30th and 100th lines of HTML 
## from this page:
http://biostat.jhsph.edu/~jleek/contact.html
(Hint: the nchar() function in R may be helpful)

```{r}
?nchar()

# nchar() takes a character vector as an argument and returns a vector whose 
# elements contain the sizes of the corresponding elements of x

```

```{r}
htmlurl <- url("http://biostat.jhsph.edu/~jleek/contact.html")
HTML <- readLines(htmlurl)
close(htmlurl)

head(HTML)

```

```{r}
# Using nchar() to determine the number of characters in the 10th, 20th, 30th 
# and 100th lines:

# nchar(HTML[10], HTML[20], HTML[30], HTML[100])
# Error: invalid 'type' argument

c(nchar(HTML[10]), nchar(HTML[20]), nchar(HTML[30]), nchar(HTML[100]))

```

Ans.: 45 31 7 25


## Q.5 Read this data set into R and report the sum of the numbers in the fourth
## of the nine columns.
https://d396qusza40orc.cloudfront.net/getdata%2Fwksst8110.for
Original Data: https://www.cpc.ncep.noaa.gov/data/indices/wksst8110.for
(Hint: This is a fixed width file format)


Fixed width files are those in which the columns each have a fixed number of 
characters, differing from other file formats where columns may be separated 
by other delimiters, e.g. commas in csv files

Fixed width files may be read into R using the function read.fwf, where the 
widths of each column must be stated. There is no way to determine the widths 
of the columns through R. 

Using the link to the original data (as provided in the question), the number of
characters in each column can be manually counted. It is also clear in this 
table that the data begins in the 5th row, hence, there will be a need to skip 
the first 4 rows when calculating the sum of the 4th column.

widths = c(10, 9, 4, 9, 4, 9, 4, 9, 4)
skip = 4


```{r}
fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fwksst8110.for"
SST <- read.fwf(fileUrl, skip = 4, widths = c(10, 9, 4, 9, 4, 9, 4, 9, 4))

head(SST)

```
```{r}
# Sum of the numbers in the 4th column:

sum(SST[,4])

```

Ans.: 32426.7
























