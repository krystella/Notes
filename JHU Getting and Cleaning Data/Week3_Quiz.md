Title: Week 3 Quiz

Subtitle: Getting and Cleaning Data

Author: Krystella Rattan

Date: 13-March-2022

---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE, eval = FALSE)
```


### Q.1 

The ACS distributes downloadable data about US communities. Download the 2006 microdata survey about housing for the state of Idaho using the download.file() from here:
https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv

and load the data into R. The codebook, describing the variable names is here:
https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FPUMSDataDict06.pdf

Create a logical vector that identifies the households on greater than 10 acres who sold more than $10,000 worth of agriculture products. Assign that logical vector to the variable "agricultureLogical". Apply the which() function like this to identify rows of the data frame where the logical vector is TRUE,

which(agricultureLogical)

What are the first 3 values that result?

```{r}
getwd()

setwd("E:/Work/Data Science/R/3 Getting and Cleaning Data/Week 3")

getwd()

```

```{r}
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv"

download.file(url, destfile = "E:/Work/Data Science/R/3 Getting and Cleaning Data/Week 3/acs_idaho.csv")

acs_idaho <- read.csv("acs_idaho.csv")

head(acs_idaho)

```

```{r}
agricultureLogical <- (acs_idaho$ACR == 3 & acs_idaho$AGS == 6)

which(agricultureLogical)

```

Ans.: 125, 238, 262

---

### Q.2

Using the jpeg package read in the following picture of your instructor into R
https://d396qusza40orc.cloudfront.net/getdata%2Fjeff.jpg

Use the parameter native=TRUE. What are the 30th and 80th quantiles of the resulting data? (some linux systems may produce an  answer 638 different for the 30th quantile)

```{r}
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fjeff.jpg"

download.file(url, destfile = "E:/Work/Data Science/R/3 Getting and Cleaning Data/Week 3/jeff.jpeg", mode = "wb")

```

```{r}
jeff <- readJPEG("E:/Work/Data Science/R/3 Getting and Cleaning Data/Week 3/jeff.jpeg", native = TRUE)

```

```{r}
answer <- quantile(jeff, probs = c(0.3, 0.8))

answer

```

Ans.: -15258512 -10575416 
Ans.: -(15258512+638) -10575416 = -15259150 -10575416

---

### Q.3

Load the Gross Domestic Product data for the 190 ranked countries in this data set:
https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FGDP.csv

Load the educational data from this data set:
https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FEDSTATS_Country.csv

Match the data based on the country shortcode. How many of the IDs match? Sort the data frame in descending order by GDP rank (so US is last). What is the 13th country in the resulting data frame?

```{r}
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FGDP.csv"

download.file(url, destfile = "E:/Work/Data Science/R/3 Getting and Cleaning Data/Week 3/gdp.csv")

```

```{r}
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FEDSTATS_Country.csv"

download.file(url, destfile = "E:/Work/Data Science/R/3 Getting and Cleaning Data/Week 3/edu.csv")

```

```{r}
gdp <- read.csv("gdp.csv", skip = 4, header = TRUE)

library(dplyr)

gdp_df <- gdp %>%
  select('CountryCode' = X, 'Rank' = "X.1", 'Country' = "X.3", 'GDP' = "X.4") %>%
  tbl_df %>%
  print

```


**Alternative, longer code:**

gdp <- read.csv("gdp.csv", skip = 4)

gdp_df <- select(gdp, X, X.1, X.3, X.4)

colnames(gdp_df) <- c("CountryCode", "Rank", "Economy", "GDP")

gdp_df <- tbl_df(gdp_df)

gdp_df



```{r}
edu <- read.csv("edu.csv")

edu_df <- tbl_df(edu)

edu_df

```

```{r}
gdp_edu <- merge(gdp_df, edu_df, by = "CountryCode")

gdp_edu_DF <- tbl_df(gdp_edu)

gdp_edu_DF

```


```{r}
sortedDF <- gdp_edu_DF %>% 
  mutate(Rank = as.numeric(Rank)) %>%
  drop_na(Rank) %>%
  arrange(desc(Rank)) %>%
  print

nrow(sortedDF)

# Answer: 189

sortedDF[13, "Country"]

# Answer: St. Kitts and Nevis

```

Answer: 189 rows; St. Kitts and Nevis

---

### Q.4 

What is the average GDP ranking for the "High Income: OECD" and "High Income: non=OECD" group?

```{r}
gdp_edu_DF %>%
  filter(Income.Group == "High income: OECD") %>%
  mutate(Rank = as.numeric(Rank)) %>%
  summarize(avg_Rank = mean(Rank)) %>%
  print

# Answer: 32.96667	
```

```{r}
gdp_edu_DF %>%
  select(Rank, Country, Income.Group) %>%
  filter(Income.Group == "High income: nonOECD") %>%
  mutate(Rank = as.numeric(Rank)) %>%
  drop_na(Rank) %>%
  summarize(avg_Rank = mean(Rank)) %>%
  print

# Answer: 91.91304	

```

Answer: 32.96667, 91.91304	

---

### Q.5 

Cut the GDP ranking into 5 separate quantile groups. Make a table versus Income.Group. How many countries are Lower middle income but among the 38 nations with highest GDP?

```{r}
library(Hmisc)

sortedDF$Quantiles <- cut2(sortedDF$Rank, g=5, labels = c("1", "2", "3", "4", "5"))

table(sortedDF$Quantiles, sortedDF$Income.Group)

#            High income: nonOECD High income: OECD Low income Lower middle income
# [  1, 39)                    4                18          0                   5

# Answer: 5

```

Answer: 5




