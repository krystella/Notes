Title: Week 4 Quiz

Author: Krystella Rattan

Date: 17-March-2022

---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE, eval = FALSE)
```

### Q.1

The ACS distributes downloadable data about the US communities. Download the 2006 microdata survey about housing for the state of Idaho using download.file() from here (already downloaded for previous assignment) and load the data into R.

Apply strsplit() to split all the names of the data frame on the characters "wgtp". What is the value of the 123 element of the resulting list?

```{r}
# Data previously downloaded and loaded into R object acs_idaho:

acs_idaho <- read.csv("acs_idaho.csv")

splitNames = strsplit(names(acs_idaho), "wgtp")

splitNames[123]

# Answer: [1] ""   "15"
```


### Q.2 

Load the GDP data for the 190 ranked countries in this data set (already downloaded for previous assignment). Remove the commas from the GDP numbers in millions pf dollars and average them. What is the average?

```{r}
# Data previously downloaded as gdp

gdp <- read.csv("gdp.csv", skip = 4, nrows = 190)

head(gdp)

gdp_DF <- data.frame(gdp[,c(1,2,4,5)])

colnames(gdp_DF) <- c("CountryCode", "Rank", "Country", "GDP")


# Remove commas using gsub() function

new_gdp <- as.numeric(gsub("\\,", "", gdp_DF$GDP))

mean(new_gdp, na.rm = TRUE)

# Answer: [1] 377652.4

```


### Q.3

In the data set from Q.2, what is a regular expression that would allow you to count the number of countries whose name begins with "United"? Assume that the variable with the country names in it is named "countryNames". How many countries begin with United?

```{r}
# ^ metacharacter represents start of a line

countryNames <- gdp_DF$Country

grep("^United", countryNames, value = TRUE)

# [1] "United States"        "United Kingdom"       "United Arab Emirates"
# Answer: 3

```


### Q.4

(Using previously downloaded GDP and EDU data). Match the data based on the country shortcode. Of the countries for which the end fiscal year is available, how many end in June?

```{r}
# Load the gdp data; load the edu data
#
# Ensure both data sets contain a variable named "CountryCode"
# 
# Use merge() to merge both data frames by the column "CountryCode"
# 
# Use a metacharacter with grep() to identify fiscal years ending in June

```

```{r}

gdp_edu <- merge(gdp_DF, edu_DF, by = "CountryCode")

length(grep("Fiscal year end: June", gdp_edu$Special.Notes))

# Answer: [1] 13

```


### Q.5

You can use the quantmod package to get historical stock prices for publicly traded companies on the NASDAQ and NYSE. Use the following code to download data on Amazon's stock price and get the times the data was sampled.
How many values were collected in 2012? How many values were collected on Mondays in 2012?

```{r}
library(quantmod)
amzn = getSymbols("AMZN", auto.assign=FALSE)
sampleTimes = index(amzn)

```

```{r}
# How many values were collected in 2012?

amzn2012 <- (grep("^2012", sampleTimes, value = TRUE))

length(amzn2012)

# Answer: [1] 250

```

```{r}

# How many values were collected on Mondays in 2012?

# class(amzn2012) : "character" ; convert to "date" by parsing with lubridate

library(lubridate)

amzn2012_parsed <- ymd(amzn2012)

class(amzn2012_parsed) # "Date"

length(amzn2012_parsed[weekdays(amzn2012_parsed) == "Monday"])

# Answer: [1] 47

```






