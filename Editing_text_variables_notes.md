Title: Editing Text Variables

Subtitle: Getting and Cleaning Data - Week 4

Author: Krystella Rattan

Date: 15-March-2022

---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE, eval = FALSE)
```

## Important points about text in data sets:

Names of variables should be:
- All lower case when possible
- Descriptive (Diagnosis vs Dx)
- Not duplicated
- Not have underscores or dots or white spaces


Variables with character values
- Should usually be made into factor variables (depends on application)
- Should be descriptive (use TRUE/FALSE instead of 0/1 and Male/Female versus 0/1 or M/F)


```{r}
# names(cameraData)

# "address" , "direction" , "street" , "crossStreet" , "intersection" , "Location.1"

```

## tolower(), toupper()

Use tolower() or toupper() to convert letters to all lowercase or uppercase respectively.

```{r}
tolower(names(cameraData))

# [1] "address" , "direction" , "street" , "crossStreet" , "intersection" , "location.1"

```

## strsplit()

Use for splitting variable names, e.g. to remove space holders

```{r}
x <- c("week_1", "week_2", "week_3")

splitNames = strsplit(x, "\\_")

splitNames[1:3]

# [1] "week" "1"   , [1] "week" "2" , [1] "week" "3" 


y <- c("week.1", "week.2", "week.3")

splitNames = strsplit(y, "\\.")

splitNames[1:3]

# [1] "week" "1"   , [1] "week" "2" , [1] "week" "3" 

```

## Lists, Subsetting

```{r}
mylist <- list(letters = c("A", "B", "C"), numbers = 1:3, matrix(1:25, ncol = 5))

head(mylist)

#  $letters
#  [1] "A" "B" "C"

#  $numbers
#  [1] 1 2 3
#
#  [[3]]
#       [,1] [,2] [,3] [,4] [,5]
#  [1,]    1    6   11   16   21
#  [2,]    2    7   12   17   22
#  [3,]    3    8   13   18   23
#  [4,]    4    9   14   19   24
#  [5,]    5   10   15   20   25

mylist[1]

# $letters
# [1] "A" "B" "C"

mylist$letters

# [1] "A" "B" "C"

mylist[[1]]

# [1] "A" "B" "C"

```


## sapply()

- Applies a function to each element in a vector

```{r}
# Say some column names are split and we only want to return the first element of each name
# Example:

splitNames[[6]][1]
    # Returns the first element of the sixth item

# [1] "Location"

# sapply() can be applied to a function to only return the first element of each

firstElement <- function(x){x[1]}

sapply(splitNames, firstElement)

# [1] "address" , "direction" , "street" , "crossStreet" , "intersection" , "Location"

```


## sub(), gsub()

Used to remove special characters 

```{r}
reviews <- c("id" , "solution_id" , "reviewer_id" , "start" , "stop" , "time_left", "accept")

sub("_", "", reviews)

# [1] "id"         "solutionid" "reviewerid" "start"      "stop"       "timeleft"   "accept"

```

```{r}
testName <- "this_is_a_test"

sub("_", "", testName)

# [1] "thisis_a_test"

gsub("_", "", testName)

# [1] "thisisatest"

```


## grep(), grepl()

Used to search for specific values or specific variable names

```{r}
grep("Alameda", cameraData$intersection)

#      Returns all of the elements in the intersection variable in which "Alameda" is found

# [1] 4 5 36
#      "Alameda" is found in the 4th, 5th and 36th elements of intersection

#      Using the parameter 'value=TRUE' returns the element itself

grep("Alameda", cameraData$intersection, value = TRUE)

# [1] "The Alameda & 33rd St"  "E 33rd & The Alameda"  "Hartford \n & The Alameda"

# grep() can also be used to check if a particular value appears in the dataset

grep("JeffStreet", cameraData$intersection)

# integer(0)

length(grep("JeffStreet", cameraData$intersection))

# [1] 0

```

```{r}
#      The grepl() function searches for the term and returns a TRUE or FALSE result

table(grepl("Alameda", cameraData$intersection))

#  FALSE  TRUE
#     77     3

#      Subsetting a dataset which consists of all the elements in which "Alameda" does not appear

cameraData2 <- cameraData[!grepl("Alameda", cameraData$intersection),]


```


## String functions, stringr

```{r}
library(stringr)

#     Number of characters that appear in a particular string
nchar("Jeffrey Leek")
# [1] 12


#     Subset part of a string, e.g. 1st - 7th letters of name:
substr("Jeffrey Leek", 1, 7)
# "Jeffrey"


#     Paste 2 strings together, can use 'sep=' to set the separator
paste("Jeffrey", "Leek")
# [1] "Jeffrey Leek"

#     Or paste with no space:
paste0("Jeffrey", "Leek")
# [1] "JeffreyLeek"


#     String trim will trim off any extra space at the end of a string
str_trim("Jeff    ")
# [1] "Jeff"

```























