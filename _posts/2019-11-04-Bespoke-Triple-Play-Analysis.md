---
layout: post
title:  "Backtesting Triple Miss Stocks in R"
by: "Jake Stoetzner"
categories: [r,backtesting]
tags: [Bespoke]
excerpt_separator: <!--more-->
---
Backtesting possible short positions by examining stocks that miss EPS and revenue estimates and lower forward guidance reveals that the behavior of these **Triple Miss** stocks offers a profitable opportunity over the short-term.

- [Goal](#goal)
- [Get the Data](#get-the-data)
  * [Extract the Stock Symbols and Dates of Each Triple Miss](#extract-the-stock-symbols-and-dates-of-each-triple-miss)
  * [Get Historical Stock Data](#get-historical-stock-data)
  * [Filter Out Data Prior to Triple Miss](#filter-out-data-prior-to-triple-miss)
  * [Examine the Aggregate Stock Data](#examine-the-aggregate-stock-data)
  * [Examine the Aggregate Stock Data 1, 5 and 10 Days After Triple Miss](#examine-the-aggregate-stock-data-1--5-and-10-days-after-triple-miss)
- [Conclusion](#conclusion)
- [Notes & Research](#notes---research)

## Goal
[Bespoke Investment Group](www.bespokepremium.com) offers a stock screen feature that "allows users to run in-depth earnings report screens and easily filter more than 150,000 individual quarterly reports for US stocks since 2001."

They also offer a report showing what they have termed an **Earnings Triple Play** - that is a stock that "beats consensus analyst EPS estimates, beats revenue estimates, and raises forward guidance."  The term has become so well known that [none other than Investopedia have given Bespoke credit for it.](https://www.investopedia.com/terms/t/triple-play.asp)  These stocks are the best candidates for long-term buy opportunities.

But is the opposite also true?  Does a stock that *misses* consensus analyst EPS estimates, *misses* revenue estimates and *lowers* forward guidance a terrible long-term buy opportunity and a possible short opportunity?  We can refer to these stocks in this category as a **Triple Miss**.

The goal is to quantify whether a Triple Miss makes a good short opportunity starting the day after the announcement.

## Get the Data
### Extract the Stock Symbols and Dates of Each Triple Miss
Using the aforementioned stock screener, I researched Triple Miss stocks in 2018, and downloaded a .csv file of the data.  Turns out there were 132 Triple Miss instances (with 104 different stocks - some companies were listed more than once!) in 2018.  On an aggregate basis, this was the return:

![Aggregate Stats of Triple Miss Stocks](/assets/img/2018.Triple.Miss.Stats.png)

After reading in the data in R, I extracted the symbols and dates of each Triple Miss.

```
#create a list of symbols that can be read in
symbols <- unlist(temp$ticker)
ticker <- as.character(unlist(temp$ticker))

#get list of dates of triple miss dates
date <- as.Date(unlist(temp$date))
```

Once this was created, I tried to use the quantmod() package to look-up the historical data.  quantmod() is wonderful for looking up individual stocks and analyzing them, but it is a challenge to deal with multiple stocks on varying dates of interest.

### Get Historical Stock Data

Enter the batchgetsymbols() package.

As the [creator of the package lamented](https://cran.r-project.org/web/packages/BatchGetSymbols/vignettes/BatchGetSymbols-vignette.html):
>In the past I have used function GetSymbols from the CRAN package quantmod in order to download end of day trade data for several stocks in the financial market. The problem in using GetSymbols is that it does not aggregate or clean the financial data for several tickers. In the usage of GetSymbols, each stock will have its own xts object with different column names and this makes it harder to store data from several tickers in a single dataframe.  Package BatchGetSymbols is my solution to this problem. Based on a list of tickers and a time period, BatchGetSymbols will download price data from yahoo finance and organize it so that you don’t need to worry about cleaning it yourself.

The batchgetsymbols() package worked beautifully to download all of the historical data for each stock going back to 2018 and return it in one giant, clean dataframe.

```
#<-----TRIPLE MISS ANALYSIS ----->
#Batch download symbols - change type.return to "log" for log
l.out   <-  BatchGetSymbols(tickers   = ticker,
                         first.date   = start.data,
                         last.date    = end.data,
                         type.return  = "arit",
                         freq.data    = freq.data)

#get a list of tickers that were downloaded
all.ticker <- l.out$df.control$ticker
```

### Filter Out Data Prior to Triple Miss
After getting the dataframe set-up, I used the dplyr() package to filter out data prior to the Triple Miss announcement.  This was required because I only wanted to look at the performance of the stock *after* the date of the Triple Miss. I then created a new data frame to hold this data using the bind_rows() function.

```
#filter out all data before entry date by ticker
b <- mapply(function(a,b)(l.out$df.tickers %>% filter(ref.date > a, ticker == b)),date,ticker,SIMPLIFY = F)

#create new df without the post entry date data
b1 <- bind_rows(b)
summary(b1$ret.closing.prices)
```

### Examine the Aggregate Stock Data
Interestingly, the average cumulative daily return for each stock measured from the date of the Triple Miss to the date of this post has been 4.28% - that means an average annual return of 1.36%.  The average daily return is .005%.  On an annualized basis, this is a Sharpe Ratio (return/volatility) of 0.59 - clearly a lot of volatility for not a whole lot of return!

![Individual Triple Miss Stocks Cumulative Return All Days](/assets/img/ch.All.png)

```
b2 <- b1 %>%
  select(ref.date,ticker,ret.closing.prices) %>%
    group_by(ticker) %>%
      summarise(Mean_Daily_Return = mean(ret.closing.prices,na.rm = TRUE)*100, Cum_Daily_Return = sum(ret.closing.prices,na.rm = TRUE)*100) %>%         arrange(Cum_Daily_Return)

> mean(b2$Cum_Daily_Return)
[1] 4.281108
> mean(b2$Mean_Daily_Return)
[1] 0.00539823
> mean(b2$Mean_Daily_Return)/sd(b2$Mean_Daily_Return)*sqrt(252)
[1] 0.5942328
```

### Examine the Aggregate Stock Data 1, 5 and 10 Days After Triple Miss
What if we shortened the window for analyzing the returns of all of the Triple Miss Stocks?  Do the effects of the Triple Miss "wear off" at a certain point or do they continue?

The Triple Miss stocks are very negative - below is a chart with the cumulative return from 1 to 10 days after each Triple Miss.

![All Triple Miss Stocks Cumulative Return Day 1 to 10](/assets/img/ch.10days.all.png)

On an individual basis, you can see the downward trend each stock exhibits post-Triple Miss.

![Individual Triple Miss Stocks Cumulative Return Day 1 to 10](/assets/img/ch.10days.png)

On an aggregate basis after 10 days:
- 71 out of the 113 stocks (approximately 63%) had a negative cumulative daily return 10 days after the Triple Miss,
- each stock had an average daily return of -0.58% and cumulatively lost 4.55% in value by day 10, and
- had an average Sharpe Ratio (average daily return divided by average standard deviation of the daily returns) of -5.57.

In short, the returns were **less** than spectacular.

Over 5 days, returns were even worse:
- 66 out of the 113 stocks (approximately 58%) had a negative cumulative daily return 5 days after the Triple Miss,
- each stock had an average daily return of -1.46% and cumulatively lost 5.26% in value by day 5, and
- had an average Sharpe Ratio of -6.50.

![All Triple Miss Stocks Cumulative Return Day 1 to 5](/assets/img/ch.5days.all.png)

And the day after a Triple miss?  95 of 113 (84%) were down day-over-day by an average of 6.86% with a Sharpe Ratio of -10.62.

## Conclusion
It seems clear that Triple Miss stocks would be a good place to start when looking for stocks to short over the near-term.  Obviously, this is not a recommendation to buy or sell; you would need to do your own research and make your own decision to determine that.  But the Triple Miss stocks might be a good filter to start looking for opportunities.

## Notes & Research
* [Analyzing Stocks Using R](https://towardsdatascience.com/analyzing-stocks-using-r-550be7f5f20d)
* [An Introduction to Stock Market Data Analysis with R (Part 1)](https://ntguardian.wordpress.com/2017/03/27/introduction-stock-market-data-r-1/)
* [getSymbols Extra | R-bloggers](https://www.r-bloggers.com/getsymbols-extra/)
* [quantmod: examples :: data](https://www.quantmod.com/examples/data/)
* [Different Ways to Obtain and Manipulate Stock Data In R Using quantmod – Programming For Finance](https://programmingforfinance.com/2017/10/different-ways-to-obtain-and-manipulate-stock-data-in-r-using-quantmod/)
* [Data Science: Theories, Models, Algorithms, and Analytics](https://srdas.github.io/MLBook/MoreDataHandling.html#using-the-apply-class-of-functions)
* [R Quantitative Analysis Package Integrations in tidyquant](https://cran.r-project.org/web/packages/tidyquant/vignettes/TQ02-quant-integrations-in-tidyquant.html#performanceanalytics-functionality)
* [Tidy Portfoliomanagement in R](https://bookdown.org/sstoeckl/Tidy_Portfoliomanagement_in_R/s-4portfolios.html)
* [Manipulating Time Series Data in R with xts & zoo](http://rstudio-pubs-static.s3.amazonaws.com/288218_117e183e74964557a5da4fc5902fc671.html#first-order-of-business---basic-manipulations)
* [chapter4.key](https://s3.amazonaws.com/assets.datacamp.com/production/course_1127/slides/chapter_4.pdf)
* [Processing and Analyzing Financial Data with R](https://www.msperlin.com/pafdR/Financial-data.html)
* [Working with Time Series Data in R](https://faculty.washington.edu/ezivot/econ424/Working%20with%20Time%20Series%20Data%20in%20R.pdf)
* [quantmod-vignette.pdf](http://statmath.wu.ac.at/~hornik/QFS1/quantmod-vignette.pdf)
* [r - Using lapply on quantmod, get straight to xts object? - Stack Overflow](https://stackoverflow.com/questions/26640795/using-lapply-on-quantmod-get-straight-to-xts-object)
* [Using BatchGetSymbols to download financial data for several tickers](https://cran.r-project.org/web/packages/BatchGetSymbols/vignettes/BatchGetSymbols-vignette.html)
* [Easy multi-panel plots in R using facet_wrap() and facet_grid() from ggplot2 | Technical Tidbits From Spatial Analysis & Data Science](http://zevross.com/blog/2019/04/02/easy-multi-panel-plots-in-r-using-facet_wrap-and-facet_grid-from-ggplot2/)
* [dplyr: How to Add Cumulative Sums by Groups Into a Data Frame? - Articles - STHDA](http://www.sthda.com/english/articles/17-tips-tricks/57-dplyr-how-to-add-cumulative-sums-by-groups-into-a-data-framee/)
* [Make Beautiful Tables with the Formattable Package | R-bloggers](https://www.r-bloggers.com/make-beautiful-tables-with-the-formattable-package/)
* [ggplot2 barplots : Quick start guide - R software and data visualization - Easy Guides - Wiki - STHDA](http://www.sthda.com/english/wiki/ggplot2-barplots-quick-start-guide-r-software-and-data-visualization)
* [Make Beautiful Tables with the Formattable Package | Displayr](https://www.displayr.com/formattable/)
