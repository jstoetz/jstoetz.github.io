---
layout: collection
title:  "Data Basics - Filling Missing Data and NA Values"
---

## Filling Data in Columns

Filling values *down* or *up* a column is a necessary part of managing our time series data.  It is also a crucial decision to make regarding how you will handle financial time series data.

### Functions Mentioned
na.locf(), fill_(), na.fill(), merge(), merge.xts(), getSymbols(), na.omit(), na.approx(), na_locf()

### Packages Mentioned
[zoo](https://cran.r-project.org/web/packages/zoo/index.html), tidyr, QuantTools, quantmod

### Filling Down a Column With ```na.locf()``` - First Rows Equal to NA

The easiest example of when this comes in handy is when matching up two data frames where one data frame has more rows with values than the other.  NA values generally appear when doing calculations on columns of data where a look back period is in place.

```
library(zoo)
df1 <- data.frame(
  "series_1" = c(1:10),
  "series_2" = c(NA,NA,3:10)
  )

df1
#   series_1 series_2
#1         1       NA
#2         2       NA
#3         3        3
#4         4        4
#5         5        5
```

If you are comparing ```df$series_1``` with ```df$series_2```, you may end up with errors due to the NA values in the first 2 rows.

One solution is to use the ```na.locf()``` function in the ```zoo()``` package.

```
na.locf(df1, fromLast = TRUE)
#   series_1 series_2
#1         1        3
#2         2        3
#3         3        3
#4         4        4
#5         5        5
```

Setting ```fromLast = TRUE``` fills from the bottom up; that is, it tells the ```na.locf()``` function to start from the bottom of the column, look for the last non-NA value and then use that value (in this case, the "3") to fill *up* for all of the NA values.

### Filling Down a Column With ```na.locf()``` - Interior Rows Equal to NA

Consider a case where you have missing values in *interior* rows of the data frame.  This usually arises when matching two data series.  For instance, comparing a 1-minute chart of a stock with a 1-minute chart of an ATM option.  The option (in general) will have far fewer entries because it trades less frequently than the underlying stock.  Thus, there will be several minutes that will not have data.  A simple example of interior row NAs:

```
df2 <- data.frame(
  "series_1" = c(1:10),
  "series_2" = c(1, 2, NA,NA,5:10)
  )

df2
#   series_1 series_2
#1         1        1
#2         2        2
#3         3       NA
#4         4       NA
#5         5        5
```

Rows 3 and 4 have missing values which are represented by NA.  We want to get rid of those values and carry the last non-NA value down (ie, carry the number "2" down into row 3 and 4).

```
na.locf(df2, fromLast = FALSE)
#   series_1 series_2
#1         1        1
#2         2        2
#3         3        2
#4         4        2
#5         5        5
```

### Filling Down a Column With ```na.locf()``` - Last Rows Equal to NA

By now you can easily figure out how to replace the last 2 NA values filling *down*.

```
df3 <- data.frame(
  "series_1" = c(1:5),
  "series_2" = c(1:3, NA, NA)
  )

df3
#  series_1 series_2
#1        1        1
#2        2        2
#3        3        3
#4        4       NA
#5        5       NA

na.locf(df3, fromLast = FALSE)
#  series_1 series_2
#1        1        1
#2        2        2
#3        3        3
#4        4        3
#5        5        3
```

### Filling Down a Column With ```fill_()```

The ```tidyr``` package offers ```fill_()``` to handle NA values in the first row, interior rows or at the end.  Instead of the ```fromLast``` argument in ```na.locf()``` you input ```.direction``` as either ```c('up', 'down')```.  You can also specify columns with the ```fill_cols``` argument.

```
library(tidyr)
# NA values at the beginning
df1 <- data.frame(
    "series_1" = c(1:10),
    "series_2" = c(NA,NA,3:10)
)

fill_(df1, fill_cols = "series_2", .direction = "up")
#   series_1 series_2
#1         1        3
#2         2        3
#3         3        3
#4         4        4
#5         5        5

# NA values in interior rows
df2 <- data.frame(
    "series_1" = c(1:10),
    "series_2" = c(1:2,NA,NA,5:10)
)

fill_(df2, fill_cols = "series_2", .direction = "down")
#  series_1 series_2
#1         1        1
#2         2        2
#3         3        2
#4         4        2
#5         5        5

# NA values at the end of the column
df3 <- data.frame(
    "series_1" = c(1:5),
    "series_2" = c(1:3,NA,NA)
)

fill_(df3, fill_cols = "series_2", .direction = "down")
#  series_1 series_2
#1        1        1
#2        2        2
#3        3        3
#4        4        3
#5        5        3
```

### Filling Down Multiple Columns Using ```fill_()```

Most times there is more than one column of data you need to fill up or down.  Use ```fill_()``` and set ```fill_cols``` argument to return the ```names()``` of the data frame.

```
library(tidyr)
df <- fill_(df, fill_cols = names(df))
head(df)
#   col1 col2 col3
#1    1   NA    b
#2    1    3    b
#3    2    4    a
#4    2    4    a
#5    2    1    a
#6    3    4    a
```

Alternatively, you can use ```fill_()``` with the ```everything()``` command.  For example:

```
df %>% fill(everything()) %>% fill(everything(), .direction = 'up')
```

Changing the ```.direction``` argument to ```up``` will copy values from the bottom up, rather than top down.

### Filling Multiple Columns - NA Values At the Beginning, Middle and End

Often times you have a ```data.frame``` with NA values mixed in at random.

```
df4 <- data.frame(
  "series_1" = c(NA, 2, 2, NA, NA, 3, NA, 4, NA, NA),
  "series_2" = c(2, NA, 1, 1, NA, 2, NA, 5, 7, NA))

df4
#   series_1 series_2
#1        NA        2
#2         2       NA
#3         2        1
#4        NA        1
#5        NA       NA
#6         3        2
#7        NA       NA
#8         4        5
#9        NA        7
#10       NA       NA
```

If you set the ```.direction``` argument to ```"down"``` it will carry the last observation forward.  This would fix ```series_2``` column but not the ```series_1``` column since it *starts* with an NA value.

```
fill_(df4, fill_cols = names(df4), .direction = "down")
#   series_1 series_2
#1        NA        2
#2         2        2
#3         2        1
#4         2        1
#5         2        1
#6         3        2
#7         3        2
#8         4        5
#9         4        7
#10        4        7
```

If you set the ```.direction``` argument to ```"up"``` it will carry the first observation backward.  This would not fix ```series_1``` column or the ```series_2``` column since they both *end* with an NA value.

```
fill_(df4, fill_cols = names(df4), .direction = "up")
#   series_1 series_2
#1         2        2
#2         2        1
#3         2        1
#4         3        1
#5         3        2
#6         3        2
#7         4        5
#8         4        5
#9        NA        7
#10       NA       NA
```

The solution is provided by the ```fill_()``` function.  In addition to ```up``` and ```down```, you can also set the ```.direction``` argument to either ```downup``` or ```updown```.  ```downup``` first carries the last observation forward then the first observation backward. Conversely, ```updown``` does the opposite; it first carries the first observation backward then the last observation forward.

```
fill_(df4, fill_cols = names(df4), .direction = "downup")
#   series_1 series_2
#1         2        2
#2         2        2
#3         2        1
#4         2        1
#5         2        1
#6         3        2
#7         3        2
#8         4        5
#9         4        7
#10        4        7

fill_(df4, fill_cols = names(df4), .direction = "updown")
#   series_1 series_2
#1         2        2
#2         2        1
#3         2        1
#4         3        1
#5         3        2
#6         3        2
#7         4        5
#8         4        5
#9         4        7
#10        4        7
```

Results are different in each case so choose between ```"updown"``` and ```"downup"``` carefully.  

## Filling Down Data for Financial and Time-Series Applications

For most financial and stock applications, you want to follow the "carry forward" rule.  This means that you always carry the last observation forward since this helps you avoid "predicting the future".  In terms of ```na.locf()``` that means that you want to set ```.direction = "downup"```.  This will ensure that the data frame object is filled with future

To illustrate the "carry forward" concept at work, consider stock data that has been merged with an ATM option.  In this example, the underlying stock will likely have more periods and fewer gaps than the option (this is not always the case).

```
library(quantmod)

# download the underlying stock data
getSymbols("AAPL",
  from = "2020/01/01",
  to = "2020/09/22",
  periodicity = "daily")

getSymbols("AAPL200925P00115000",
  from = "2020/01/01",
  to = "2020/09/22",
  periodicity = "daily")
```

Comparing the length of each object reveals the difference in the number of periods that the stock has versus the option.  This makes sense though - weekly options are generally not issued until 5-6 weeks prior to expiration - so there should be multiple dates prior to the option issue that the stock traded.

```
> length(AAPL$AAPL.Open)
[1] 182
> length(AAPL200925P00115000$AAPL200925P00115000.Open)
[1] 18
```

Any periods (after the option is issued) where the option did not trade is represented by an NA in the OHLC data for the option.  The most conservative approach is to roll the closing price from the prior period forward.  This assumes (sometimes incorrectly) that the option *would have* traded for the closing price the following period.  Whether or not this is true is up to the technician.  A good rule of thumb is that the *shorter* the period, the more likely it is that the carry forward value is going to be accurate.  Rolling forward 1-minute data will likely be more accurate than rolling forward daily data, especially if the underlying stock trades overnight.

Other approaches to filling in the missing gaps include: (1) approximating the value between the 2 known values (either by an average or some other means) by interpolating a value, and (2) just simply omitting any gaps in the data.  ```zoo::na.approx()``` does a good job of approximating by using the time series data to interpolate a missing value.  ```data.table::na.omit()``` does a good job getting rid of unnecessary NA values.

### Filling in Values Other than NA with ```na.fill()```

Sometimes you don't want to fill NA values with the previous or future observation.  Sometimes you *know* what value should be there, and you would like to replace the NA accordingly.  That is where ```na.fill()``` comes in.

```
df5 <-
  data.frame(
    "series_1" = c(NA, 2, 2, NA, NA, 3, NA, 4, NA, NA),
    "series_2" = c(2, NA, 1, 1, NA, 2, NA, 5, 7, NA)
  )

data.frame(na.fill(df5, fill = 0))
#   series_1 series_2
#1         0        2
#2         2        0
#3         2        1
#4         0        1
#5         0        0
#6         3        2
#7         0        0
#8         4        5
#9         0        7
#10        0        0
```

Anything passed into the ```fill``` argument will be substituted for NA in the data frame.  This could be a ```list``` of items as well (in the form of ```c(a, b, c)```.  Anything unused in the ```list``` will be recycled.

### Financial and Stock-Focused ```fill()``` Functions

The ```QuantTools``` package has a useful function for financial time-series data called ```na_locf(x, na = NA)```.  It has two arguments:  (1) ```x``` is a list or vector to roll through, and (2) ```na``` tells the function what to do with leading NA - it defaults to leaving them as NA or you can choose to change it to 0 (so that it represents something like ```na.fill()```.)

## Notes and Research
-  [zoo: S3 Infrastructure for Regular and Irregular Time Series (Z's Ordered Observations)](https://cran.r-project.org/package=zoo)
-  See [R: fill down multiple columns - Stack Overflow](https://stackoverflow.com/questions/37648980/r-fill-down-multiple-columns)
-  See [Forward and backward fill data frame in R](https://stackoverflow.com/questions/42915636/forward-and-backward-fill-data-frame-in-r)
-  See [Replacing NAs with latest non-NA value](https://stackoverflow.com/questions/7735647/replacing-nas-with-latest-non-na-value)
-  For more information on how Yahoo notates their option symbols, check out [How to read an option symbol](https://uk.help.yahoo.com/kb/option-symbol-sln13884.html).  "The components of an options symbol are:  Root symbol (ticker symbol) + Expiration Year (yy) + Expiration Month (mm) + Expiration Day (dd) + Call/Put Indicator (C or P) + Strike Price."  The decimal point for the strike price falls 3 places from the right in the options symbol: 00000.000.  Thus, an option symbol with a $123 strike price would be written as 00123000.  The option symbol leaves out the decimal point BUT you still have to include the trailing 3 zeroes to the right.
