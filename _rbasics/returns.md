---
layout: collection
title:  "Rate of return calculations"
---

## Overview
Few things in this life cause more headaches than performance metric calculations.  Ask 10 different financial firms how to calculate an *annualized yield* and you may get 10 different answers.  

That being said it is important to set your own standard so that you have an apples-to-apples comparison among different systems.

## TOC
- [Single-Period Return Metrics](#h1)
  - [Periodical Return](#h1.1)
  - [Annual Return](#h1.2)
- [Multi-Period Return Metrics](#h2)
  - [Total or Cumulative Return](#h2.1)
  - [Arithmetic Returns](#h2.2)
  - [Geometric Returns](#h2.3)
- [Multi-Asset, Single-Period Return Metrics](#h3)
- [Multi-Asset, Multi-Period Return Metrics](#h4)
- [Logarithmic Returns](#h5)


### Functions Mentioned


### Packages Mentioned
- [Performance Analytics](https://cran.r-project.org/web/packages/PerformanceAnalytics/index.html)

## <a name="h1"></a>Single-Period Return Metrics

### <a name="h1.1"></a>Periodical Return
The percentage change in the value of an asset or investment, including reinvestment of income, from the beginning to the end of a period, assuming no contributions or disbursements.

r = FV/PV - 1

FV = the future value of a sum of money
PV = the present value of the same amount
r = the periodical return, or the growth rate per period

For periodical returns calculated over more than one period:

r = (FV/PV)^(1/n) - 1

n = number of periods of growth

```
# - this functions takes in a starting and ending value and provides the return over n number of periods

fn.periodical.return <- function(FV, PV, n){
  a = ((FV/PV)^(1/n)) - 1
  return(a)
}
```
### <a name="h1.2"></a>Annual Return
"Annualized returns are useful for comparing two assets. To do so, you must scale your observations
to an annual scale by raising the compound return to the number of periods in a year, and taking the
root to the number of total observations." [Source:  Performance Analytics Manual](https://cran.r-project.org/web/packages/PerformanceAnalytics/PerformanceAnalytics.pdf)

Usually, you are transforming another type of return to an annualized return.  Converting daily, weekly, monthly or quarterly data to an annual return requires some assumptions.

In the financial world, it is (mostly) universally accepted that there are:
- 252 days in a year,
- 52 weeks in a year,
- 12 months in a year, and
- 4 quarters in a year.

Given a future value and a present value over a given scale that is *less than a year* (ie, daily, weekly etc.), then what is the annualized return?

```
fn.annualize.return <- function(FV, PV, scale = c('hourly','daily', 'weekly', 'monthly', 'quarterly')){
  a <- scale
  b <- c(6048, 252, 52, 12, 4)
  c <- c('hourly','daily', 'weekly', 'monthly', 'quarterly')
  d <- b[c == a]
  e <- (FV/PV-1)*d
  return(e)
}

# input a vector of values and returns annualized return
fn.annualize.return.values <- function(df, scale = c('hourly','daily', 'weekly', 'monthly', 'quarterly')){
  a <- scale
  b <- c(6048, 252, 52, 12, 4)
  c <- c('hourly','daily', 'weekly', 'monthly', 'quarterly')
  d <- b[c == a]
  df <- as.vector(df)
  e <- length(df)
  f <- df/lag(df, n = 1)-1
  f[1] <- 0
  g <- sum(f, na.rm = T)/e*d
  return(g)
}
```

## <a name="h2"></a>Returns over Multiple Holding Periods

Things get more complicated when comparing multiple returns with differing holding periods.

### <a name="h2.1"></a>Total or Cumulative Return
Is Stock A with a 7% annual returns over 8 years a better investment (in this case, a higher total return) than Stock B that returned 10% over 5 years?

You can revert back to time value of money basics for this question.

Total Return = (1 + r)^n - 1

r = the periodical return, or the growth rate per period
n = total number of periods *of equal length*

```
fn.total.return <- function(r, n){
  a = (1 + r)^n - 1
  return(a)
}

> fn.total.return(.07, 8)
[1] 0.7181862
> fn.total.return(.1, 5)
[1] 0.61051
```

### <a name="h2.2"></a>Arithmetic Returns
Arithmetic returns are the typical calculation of the mean.  Take the sum of all returns for each equal period and divide by the total number of periods.

Arithmetic returns assume *no* compounding or re-investment of capital.

For example, compare 2 stocks with the following returns (assume the length of each return period for each stock is equal):

```
stock_a <- c(.1,-.1,.2,.4,.5,-.2,.5,-.8)
stock_b <- c(.4,-.5,.6,-.75)
```

We can figure out the average of all of these returns fairly easily.

```
# accepts a numeric vector of percent returns
fn.arithmetic.return <- function(r, n){
  a <- sum(r, na.rm = T)
  b <- a/n
  return(b)
}

fn.arithmetic.return(stock_a,length(stock_a))
#[1] 0.075
fn.arithmetic.return(stock_b,length(stock_b))
#[1] -0.0625
```

But what does it *mean* that stock_a has an arithmetic return of .075?

Arithmetic returns assume that the same investment is made at the beginning of each period.  This means that arithmetic are *discrete* returns - they have no memory of prior periods.

If stock_a was worth $100 at the beginning of all periods, then the total value of the stock at the end can be found by (1) multiplying the vector of the returns times the starting value, (2) summing those returns, and (3) adding the sum to the beginning value of stock_a.

```
100*stock_a
#[1]  10 -10  20  40  50 -20  50 -80
sum(100*stock_a)
#[1] 60
sum(100*stock_a)+100
#[1] 160
```

So then the arithmetic return *means* that you could have invested $100 each period at .075, taken your returns off the table (ie, not reinvested them) and started again with the same $100 on the next period. At the end of 8 periods, you would have your original $100 plus the sum of each invested amount.  It can (clumsily) be shown by:

```
sum(100*(rep(.075,8)))+100
#[1] 160
```

### <a name="h2.3"></a>Geometric Returns
Geometric returns (ie, "compound returns") reflect that the increase or decrease in value is re-invested each period.

One thing to note about arithmetic returns is that they do not reflect volatility accurately.

Geometric returns, on the other hand,

## <a name="h3"></a>Multi-Asset, Single-Period Return Metrics
## <a name="h4"></a>Multi-Asset, Multi-Period Return Metrics
## <a name="h5"></a>Logarithmic Returns

## Notes & Research
* [How to Calculate Mortgage Payment Schedule in R – Become Great at R](https://masterr.org/r/calculate-mortgage-payment-schedule/)
* [https://faculty.ucr.edu/~tgirke/Documents/R_BioCond/My_R_Scripts/mortgage.R](https://faculty.ucr.edu/~tgirke/Documents/R_BioCond/My_R_Scripts/mortgage.R)
* [2020-gips-standards-asset-owners.ashx](https://www.cfainstitute.org/-/media/documents/code/gips/2020-gips-standards-asset-owners.ashx)
* [Introduction - GIPS - Finance Train](https://financetrain.com/lessons/introduction-gips/)
* [How to Calculate Annualized Returns - Finance Train](https://financetrain.com/how-to-calculate-annualized-returns/)
* [MBA503C02.pdf](https://www.scranton.edu/faculty/hussain/teaching/mba503c/MBA503C02.pdf)
* [R: Calculate Periodic Returns](https://www.quantmod.com/documentation/periodReturn.html)
* [The difference between arithmetic and geometric investment returns](https://www.firstlinks.com.au/why-you-should-know-the-difference-between-arithmetic-and-geometric-investment-returns)
* [Geometric Mean Return Formula (with Calculator)](https://financeformulas.net/Geometric_Mean_Return.html)
