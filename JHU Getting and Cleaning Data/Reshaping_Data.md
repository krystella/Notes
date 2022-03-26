Title: Reshaping Data

Subtitle: Getting and Cleaning Data - Week 3

Author: Krystella Rattan

Date: 8-March-2022

---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

### Tidy Data

Principles of Tidy Data:
1. Each variable forms a column
2. Each observation forms a row
3. Each table/file stores data about one kind of observation (e.g. people, hospitals)

Data Reshaping focuses on the first two principles


### Melting data frames

melt(dataframe, id variables, measure variables)

```{r}
library(reshape2)
head(mtcars)
```

```{r}
mtcars$carname <- rownames(mtcars)

carMelt <- melt(mtcars, id=c("carname", "gear", "cyl"), measure.vars=c("mpg", "hp"))

head(carMelt, n=3)

tail(carMelt, n=3)

```


### Casting data frames

The dcast() function will re-cast the data frame into a particular shape or data
frame

dcast(dataset, rows ~ columns)

dcast() functions summarizes the dataset, by default summarizes by length

```{r}
cylData <- dcast(carMelt, cyl ~ variable)
cylData

```

```{r}
cylData <- dcast(carMelt, cyl ~ variable, mean)
cylData

```


### Averaging values

```{r}
head(InsectSprays)

```

```{r}
tapply(InsectSprays$count, InsectSprays$spray, sum)

```


### Split

```{r}
spIns = split(InsectSprays$count, InsectSprays$spray)
spIns

```

### Apply

```{r}
sprCount = lapply(spIns, sum)
sprCount

```

### Combine

```{r}
unlist(sprCount)

sapply(spIns, sum)
```


### plyr package

ddply(dataset, .(variables to summarize), summarize, how we want to summarize it)

Note the use of . before (spray), which replaces the need to use "" 

```{r}
ddply(InsectSprays,.(spray), summarize, sum=sum(count))

```


### Creating a new variable

```{r}
spraySums <- ddply(InsectSprays,.(spray),summarize,sum=ave(count,FUN = sum))

dim(spraySums)

head(spraySums)

```


### Other

acast() : for casting multidimensional arrays

arrange() : for faster re-ordering without using order() command

mutate() : for adding new variables


















