---
title: "An Analysis of Parlay Bets Using R"
layout: post
by: Jake Stoetzner
output:
  html_document:
    toc: true
    depth: 4
    theme: united
---

## Table of Contents
<hr/>

## 1. Introduction to Parlay Odds
<hr/>

Ask 10 different gamblers whether a parlay bet is a "wise" play an you will likely get 10 different answers.  A parlay is "a single bet that links together two or more individual wagers and is dependent on all of those wagers winning together". [Wikepedia - Parlay(gambling)](https://en.wikipedia.org/wiki/Parlay_(gambling)). At most betting houses (on and off-shore) you can combine different types of bets within a single parlay including: [moneyline](https://www.actionnetwork.com/education/moneyline), [over-under or game totals](https://www.actionnetwork.com/education/over-under-total) and [spreads](https://www.actionnetwork.com/education/point-spread).  In addition to combining multiple types of bets, you can include from 2 to 10 different games to link together.

For example, you might bet a 3-game parlay that takes: (1) the Falcons +7 in the Patriots vs Falcons game, (2) over 47.5 for the Patriots vs Falcons game total, and (3) the Falcons to Win at +245 on the moneyline.  If the final score of the game is Patriots 35 and Falcons 38, you would win all the bets.  But if the score was Patriots 42 Falcons 38, you win bet #1 and #2, but would lose bet #3 since the Falcons didn't win straight up.  Even though you won 2 of the 3 bets, you would lose the 3-game parlay.

If all three games hit and you had bet $100 on each leg individually, you would have won: (1) 91.91 at -110 odds on the spread bet, (2) 95.24 at -105 odds on the over bet and (3) 240 at -240 odds on the moneyline bet for a total won of 427.15 for risking 300.  That's a return-risk ratio 1.42.

Had you bet the same 300 on the 3-game parlay, you would have won 3501.82 for a return-risk ratio of 11.67. That's more than 8 times as much as the single game bet!  This means that if you could recreate this exact same scenario 8 times, and have the 3-game parlay hit at least one of those times, you would be better off betting the parlay.

So why the dissent among gamblers when it comes to parlays? Most of it comes from the fact sportsbooks don't pay out the "fair" or "true" amount based on the odds.

Assume for a second the outcome of any single bet is 50-50.  If we were offered "true" odds, then the book would payout 2-to-1 for each bet.  In American Odds terms, that's a bet of -100; for every 100 you bet, the payout is 100.  But you never see bets go off at -100; it is usually -110 meaning that you would need to bet 110 to win 100.  If the outcome of the bet is 50-50 and we wagered 100, then we lose 100 when the bet goes against us but only win  90.91 if we win. This extra 9 bucks goes back to the house; it's the "juice" or "vig" you pay so the book can make money.  And assuming they have equal action on both sides of the wager, they make a risk-free return.

### 1.1 Converting Odds

The table below shows some common conversions between different forms of odds and probabilities.  Odds are different than probabilities in the statistical world:

* The **probability** that an event will occur is the fraction of times you expect to see that event in many trials. Probabilities always range between 0 and 1.
* The **odds** are defined as the probability that the event will occur divided by the probability that the event will not occur. [See What is the difference between odds and probability?](https://www.graphpad.com/support/faq/probability-vs-odds/).

Before I get into how different odds are expressed, it is important to understand the difference between **total return** and **profit or winning**.
* **Total Return** is the amount you wagered plus the original bet.
* **Profit/Loss or Winning/Losses** represent the amount you won or lost separate from the original bet.
These terms are often used interchangably when you are researching or looking up information online,  It is important to keep them separate though, as they will affect your analysis.

Odds come in several varieties including:

* **American:** The default for most American sports books.  Expressed as a plus (+) or minus (-). A positive figure show the profit if you bet $100.  A negative figure show how much you would have to bet to profit \$100. Example: +300 means a \$100 bet would return a profit of \$300, and -450 means you would have to bet \$450 to profit or win \$100.
* **Decimal:** Most popular at international sports books.  Expressed as a whole number beginning with 1.  Decimal odds are slightly more intuitive than American odds since they express the **total return** for every \$1 wagered.  Example: 1.91 means that for every \$1 wagered your total return will be \$1.91 representing \$.91 in winnings or profit plus the return of your \$1 bet. Numbers over 2 are underdogs and decimals under 2 are favorites.
* **Fractional:** Less popular in most books, expressed as the amount you win over the amount you bet.  Underdogs have a higher numerator and lower denominator (ie 6/5).  Favorites have a lower numerator over a higher denominator (4/6).
* **Percentage or Implied Probability:** Rarely offered at most books, it is the percent chance that an event will occur.  Other odds formats are an interpretation of this probability. Example: 30% means the event should occur 3 out of 10 times.

#### Table: Converting Probabilities and Odds

![](/Users/jake_macbook_pro/Dropbox/jstoetz.github.io/jstoetz.github.io/assets/img/odds-conversion-formulas.png)

If you would like to view the table in more depth, you can [View the Google Sheet](https://docs.google.com/spreadsheets/d/1aGOGN8T8hT6N9lGrYD4ANQPAngYamPCGrR7AIXXqRfc/edit?usp=sharing) or [Copy the Google Sheet](https://docs.google.com/spreadsheets/d/1aGOGN8T8hT6N9lGrYD4ANQPAngYamPCGrR7AIXXqRfc/copy?usp=sharing).

### 1.2 Implied Probabilities and Parlays

[Implied probability](https://help.smarkets.com/hc/en-gb/articles/214058369-How-to-calculate-implied-probability-in-betting) is a conversion of betting odds into a percentage. It takes into account the bookmaker margin to express the expected probability of an outcome occurring. Edge comes when you identify a difference between the implied probability offered by the bookmaker and what you believe will occur.

Parlays involve calculating the probability of multiple events occurring. The probability of [two independent events occurring together](https://www.statisticshowto.com/probability-and-statistics/probability-main-index/how-to-find-the-probability-of-two-events-occurring-together/) is found by multiplying the probability of the first event times the probability of the second event.  So a bet with a 50 percent probability occurring independently twice can be solved by multiplying 0.5 x 0.5 = 0.25.  The "true" probability of a 2-game parlay is then 25%, meaning that the house should offer you 3 dollars for every 1 dollar you bet.

What if each event is offered at -110? A line of -110 means an implied probability of 110/(110+100) = 52.32%.  The "fair" probability of these two events occurring can be found by multiplying 0.5232 x 0.5232 = 27.43%.  The house would need to payout 3.78 dollars for every 1 dollar you bet.  Is that the case? I bet you know the answer already...

![](/Users/jake_macbook_pro/Dropbox/jstoetz.github.io/jstoetz.github.io/assets/img/parlay-odds-table.png)

The [table above](https://en.wikipedia.org/wiki/Parlay_(gambling)#Typical_payouts_for_up_to_10_team_parlay_bet) shows the typical payout for a 2-game to 10-game parlay based on -110 odds.

We can also calculate these implied probabilities using R and the custom function ```fn.parlay.tbl()```. Recall that a single -100 bet is considered a 50-50 proposition and that a -110 bet requires you to win north of 52% to break even.

#### Table: Implied Probability and True Odds for -100 Parlay

|Number of Games |Probability (%) |Odds      |
|:---------------|:---------------|:---------|
|2               |25.0000         |3 to 1    |
|3               |12.5000         |7 to 1    |
|4               |6.2500          |15 to 1   |
|5               |3.1250          |31 to 1   |
|6               |1.5625          |63 to 1   |
|7               |0.7812          |127 to 1  |
|8               |0.3906          |255 to 1  |
|9               |0.1953          |511 to 1  |
|10              |0.0977          |1023 to 1 |

#### Table: Implied Probability and True Odds for -110 Parlay

|Number of Games |Probability (%) |Odds        |
|:---------------|:---------------|:-----------|
|2               |27.4376         |2.64 to 1   |
|3               |14.3721         |5.96 to 1   |
|4               |7.5282          |12.28 to 1  |
|5               |3.9434          |24.36 to 1  |
|6               |2.0656          |47.41 to 1  |
|7               |1.0820          |91.42 to 1  |
|8               |0.5667          |175.45 to 1 |
|9               |0.2969          |335.85 to 1 |
|10              |0.1555          |642.08 to 1 |

#### Table: Implied Probability and True Odds for -120 Parlay

|Number of Games |Probability (%) |Odds        |
|:---------------|:---------------|:-----------|
|2               |29.7521         |2.36 to 1   |
|3               |16.2284         |5.16 to 1   |
|4               |8.8519          |10.3 to 1   |
|5               |4.8283          |19.71 to 1  |
|6               |2.6336          |36.97 to 1  |
|7               |1.4365          |68.61 to 1  |
|8               |0.7836          |126.62 to 1 |
|9               |0.4274          |232.98 to 1 |
|10              |0.2331          |427.96 to 1 |

### 1.3 Goals of the Post

The goal of this post is to analyze the following questions:

* Are parlays a "wise" play based on theoretical data?
* Are parlays a "wise" play based on actual historical data? This will involve downloading actual sports betting data and analyzing it with the **tidyverse** package.
* Is there a benefit to betting ["same-game" or single-game parlays](https://sportshandle.com/same-game-parlay/) (ie, a 2-game parlay with a moneyline and spread bet on the same game?

## 2. Parlays: A Wise Play?

### 2.1 Expected Values in Parlay Bets

“Expected value is a predicted value of a variable, calculated as the sum of all possible values each multiplied by the probability of its occurrence.” See [How to calculate expected value in betting](https://help.smarkets.com/hc/en-gb/articles/214554985-How-to-calculate-expected-value-in-betting).

A simple way of showing expected value (EV) is the probability of winning times the payout minus the probability of losing times the loss.

```
EV = win.prob * win.amount - (1-win.prob)*win.amount
```

Assume you are betting 2 single game moneyline bets at 100 dollars apiece.  One is -150 and one is +450. The implied probability and expected value is:

```
# - two single bets expected value
odds.game1 <- -150
odds.game2 <- 450
bet <- 100

# - convert american odds to probabilities
game1 <- abs(odds.game1)/(abs(odds.game1)+100)
game2 <- 100/(odds.game2+100)

# - calculate individual game EV
ev.game1 <- game1*bet - (1-game1)*bet
ev.game2 <- game2*bet - (1-game1)*bet

total.ev <- ev.game1 + ev.game2

# > total.ev
# [1] -1.818182
```

The ```total.ev``` value of -1.81 means that if you accept the implied probability offered by the book, your expected value is -1.81 for every 200 dollars bet.

What if you parlayed the games? What would the implied probability and expected value be then? Since we are now combining probabilities, you would need to multiply the probabilities of the games to determine the expected value. I will use the custom function I created to calculate the EV ```fn.parlay.ev()```. One thing to note is that if you want an "apples-to-apples" comparison between 2 single game bets of $100 and a 2-game parlay bet, you would need to input the bet amount in ```fn.parlay.ev()``` as $200.

```
# - function to return the expected value of a bet
fn.parlay.ev <- function(odds = c(-110,-110), bet = 100){
  ind.prob <- ifelse(odds<0, abs(odds)/(abs(odds)+100), 100/(odds+100))
  cum.prob <- prod(ind.prob)
  ev <- bet*cum.prob - bet*(1-cum.prob)
  return(ev)
}

> fn.parlay.ev(odds = c(-150,450),bet = 200)
[1] -156.3636
```

Yikes! A $200 2-game parlay bet would result in a loss of $156 on average.  That is why most gamblers will tell you that you are better off allocating your dough to single game bets.

### 2.2 Where's The Edge?

If parlays are such a big loser, why would anyone ever bet them? Recall that implied probability is the bookmaker's expression of the outcome of the bet. If you disagree with the bookmaker's evaluation, and find that the actual probability is skewed in your favor, you then have the ability to make money.  But how do you express this mathematically in R?

Assume you have done some [least squares regression](https://jstoetz.github.io/r/sports-betting/2019/10/24/Recreating-a-1976-Least-Squares-Betting-System-in-R/) or [created another sports betting system](http://www.topbettingreviews.com/regression-analysis-in-sports-betting-systems/) and found that you have an edge of 54.5% overall (across all games and teams) which is American odds of -120.  This equates to a $9.09 EV for each $100 bet.

The difference between your calculated probability (call it the "actual probability or actual expected value" or AV) and the implied probability is your edge.  Therefore, it would make sense that the difference in the calculated expected values between the AEV and the implied expected value (IEV) would be your edge represented on a per bet basis.

Consider a single game bet at -110/52.38%.  You believe that the actual outcome is -120/54.54%. Your EV won't be $9.09 for each $100 bet (since you aren't offered -120 odds); at -110 odds your EV is $4.76. The difference between the two bets is $9.09 - $4.76 = $4.32.

You can use the difference in the expected value formulas:

```
fn.parlay.ev(c(-120),100)-fn.parlay.ev(c(-110),100)
# [1] 4.329004
```

### 2.3 Downloading & Summarizing the Data

I used the **rvest** package to download the historical betting data from [Sports Books Reviews Online](https://www.sportsbookreviewsonline.com/). I won't review it here, but you can review the code in the repository.

Initially, I combined all of the data together for NFL, NBA, NCAAF, NCAAB and MLB. After reviewing the outcome, I noted that it would be better to analyze the data by sport rather than all together.  Additionally, Monte Carlo analysis is better suited to analyzing 

Below is a summary of the NBA data from 2007 to the present. The data was summarised assuming that you make a bet on either the home team or the visitor team with either a Spread bet, a ML bet or a 2-bet parlay on the same team (both Spread and Moneyline).

#### Table: NBA 2007 to Present - Summary of Home Team Data - Profit and Loss

|                            |           |
|:---------------------------|----------:|
|Home - Spread - Avg P/L     |      -3.97|
|Home - Spread - Max P/L     |     100.00|
|Home - Spread - Min P/L     |    -100.00|
|Home - Spread - Total P/L   | -71,645.45|
|Home - ML - Avg P/L         |      -3.72|
|Home - ML - Max P/L         |   1,900.00|
|Home - ML - Min P/L         |    -100.00|
|Home - ML - Total P/L       | -67,070.40|
|Home - 2-Parlay - Avg P/L   |      49.31|
|Home - 2-Parlay - Max P/L   |   3,718.18|
|Home - 2-Parlay - Min P/L   |    -100.00|
|Home - 2-Parlay - Total P/L | 889,746.94|

The way to read the table is as follows:
* Row 1-4: A single bet on the Spread for the Home team for every game during the time period,
* Row 5-8: A single bet on the MoneyLine for the Home team for every game during the time period,
* Row 9-12: A 2-bet parlay on the Spread & MoneyLine for the Home team for every game during the time period.

#### Table: NBA 2007 to Present - Summary of Visitor Team Data - Profit and Loss

|                               |             |
|:------------------------------|------------:|
|Visitor - Spread - Avg P/L     |        -1.39|
|Visitor - Spread - Max P/L     |       100.00|
|Visitor - Spread - Min P/L     |      -100.00|
|Visitor - Spread - Total P/L   |   -25,063.64|
|Visitor - ML - Avg P/L         |        -3.92|
|Visitor - ML - Max P/L         |     2,000.00|
|Visitor - ML - Min P/L         |      -100.00|
|Visitor - ML - Total P/L       |   -70,775.07|
|Visitor - 2-Parlay - Avg P/L   |        71.77|
|Visitor - 2-Parlay - Max P/L   |     3,909.09|
|Visitor - 2-Parlay - Min P/L   |      -100.00|
|Visitor - 2-Parlay - Total P/L | 1,294,960.01|

In addition to the profit and loss calculation, I have analysed the wins and loss data for each bet by home and visitor team.

#### Table: NBA 2007 to Present - Summary of Home Team Data - Wins and Losses

|                                             |          |
|:--------------------------------------------|---------:|
|Home - Spread - Win Total                    |  8,739.00|
|Home - Spread - Loss Total                   |  8,983.00|
|Home - Spread - Total Games                  | 17,722.00|
|Home - Spread - Average Winning Percentage   |     49.31|
|Home - ML - Win Total                        | 10,623.00|
|Home - ML - Loss Total                       |  7,421.00|
|Home - ML - Total Games                      | 18,044.00|
|Home - ML - Average Winning Percentage       |     58.87|
|Home - 2-Parlay - Win Total                  |  8,055.00|
|Home - 2-Parlay - Loss Total                 |  9,989.00|
|Home - 2-Parlay - Total Games                | 18,044.00|
|Home - 2-Parlay - Average Winning Percentage |     44.64|

#### Table: NBA 2007 to Present - Summary of Visitor Team Data - Wins and Losses

|                                                |          |
|:-----------------------------------------------|---------:|
|Visitor - Spread - Win Total                    |  8,983.00|
|Visitor - Spread - Loss Total                   |  8,739.00|
|Visitor - Spread - Total Games                  | 17,722.00|
|Visitor - Spread - Average Winning Percentage   |     50.69|
|Visitor - ML - Win Total                        |  7,421.00|
|Visitor - ML - Loss Total                       | 10,623.00|
|Visitor - ML - Total Games                      | 18,044.00|
|Visitor - ML - Average Winning Percentage       |     41.13|
|Visitor - 2-Parlay - Win Total                  |  6,632.00|
|Visitor - 2-Parlay - Loss Total                 | 11,412.00|
|Visitor - 2-Parlay - Total Games                | 18,044.00|
|Visitor - 2-Parlay - Average Winning Percentage |     36.75|

Note: all tables for NFL, NBA, NCAAF and NCAAB are included in the Appendix section of this post.

## 3 Monte Carlo Analysis of 2-Game Parlay Bets on NBA Game Data

Assume a rich uncle gave you $10,000 and said you had to bet it on 2-game parlays on NBA games involving the ML and the Spread (your uncle is quite the betting sharp). You can only bet on ML plays with odds between -13000 and +6500.  For spread bets, you can only take the favorite at -110 or the underdog at -110. 

A quick summary of the data:

* The average ML favorite offers odds of -413.
* The average ML underdog offers odds of 299.
* The average Spread is +/- 6 points.
* The bet per parlay is $100.
* You can only bet 100 parlays.

### 3.1 Monte Carlo Run #1 - Random 2-Game Parlays

Your Uncle is crazy rich and he will allow you to make 100 bets 100,000 times.  You get to keep any profit but don't have to pay back any losses.  Profits don't carryover; each iteration of 100 parlay bets either gains or loses money separate and apart from other iterations. Because you don't know anything about the games, you must make these bets blind without knowing anything about the teams or the matchups (or even the year the game was played). Let's see what happens when we model this (really) crazy challenge in R using [Monte Carlo Analysis](https://www.countbayesie.com/blog/2015/3/3/6-amazing-trick-with-monte-carlo-simulations):

```
# times to run the simulation
runs <- 100000

# - export real data
mc.data <- data.frame(Home.ML = sport.All.Data3$Home.ML, Visitor.ML = sport.All.Data3$Visitor.ML,H.Spread = sport.All.Data3$H.Spread.Open,V.Spread = sport.All.Data3$V.Spread.Open, Visitor.Final = sport.All.Data3$Visitor.Final, Home.Final = sport.All.Data3$Home.Final)

# add in winner and loser odds
mc.data2 <- mc.data %>% mutate(
  ML.Winner = ifelse(mc.data$Home.Final > mc.data$Visitor.Final, "H", "V"),
  Diff = mc.data$Home.Final - mc.data$Visitor.Final,
  Spread.Winner = ifelse((H.Spread + Diff) > 0, "H", "V"),
  ML.Winner.Odds = ifelse(ML.Winner == "H", Home.ML, Visitor.ML),
  ML.Loser.Odds = ifelse(ML.Winner == "H", Visitor.ML, Home.ML)
)

# function to return the profit and loss from the bet
parlay.bet <- function(){
  # - randomly choose H or V for ML Bet
  mc.ML.team <- sample(c("H","V"), 100, replace = TRUE)
  
  # - randomly choose H or V for Spread Bet
  mc.Spread.team <- sample(c("H","V"), 100, replace = TRUE)
  
  # - randomly choose game row for ML and Spread
  mc.ML.game <- sample(1:length(mc.data2$ML.Winner), 100, replace = FALSE)
  mc.Spread.game <- sample(1:length(mc.data2$Spread.Winner), 100, replace = FALSE)
  
  # - parlay WL
  mc.parlay.WL <- ifelse((mc.data2[mc.ML.game,]$ML.Winner == mc.ML.team & mc.data2[mc.Spread.game,]$Spread.Winner == mc.Spread.team),"W","L")
  
  # - parlay odds - ML winner and loser odds
  mc.parlay.Odds <- ifelse((mc.data2[mc.ML.game,]$ML.Winner == mc.ML.team & mc.data2[mc.Spread.game,]$Spread.Winner == mc.Spread.team), mc.data2[mc.ML.game,]$ML.Winner.Odds, mc.data2[mc.ML.game,]$ML.Loser.Odds)
  
  # - ML bet
  mc.parlay.PL = unlist(mapply(function(x,y)(ifelse(x == "W", fn.parlay.profit(c(y,-110),100), -100)),mc.parlay.WL,mc.parlay.Odds,SIMPLIFY = F))
  return(sum(mc.parlay.PL))
}

mc1.results <- replicate(runs, parlay.bet())
```

The results from our 100,000 simulations are, not surprisingly, not so great.  On average, we lost $820 of our uncle's money after each iteration of 100 2-game parlay bets.

```
> summary(mc1.results)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
-7407.1 -2174.9  -937.7  -820.4   409.2 14145.4 
```

![](/Users/jake_macbook_pro/Dropbox/jstoetz.github.io/jstoetz.github.io/assets/img/mc1results-plot-parlay.png)

### 3.2 Monte Carlo Run #2 - Random 2-Game Parlays With Same Game

Since the results of Run #1 weren't great, you ask your rich uncle for another shot.  Only this time, you will impose the following restrictions on your bets:

* Both parlay bets will be in the same game. For example, if you bet the spread on an individual game, you will also pick the ML winner in that same game.

I just changed the code on the ```parlay.bet()``` function so that the same game row is chosen for each bet.  The new function is ```parlay.bet2()```, and it runs the random sampling of all games 100 times and returns the total profit or loss from each iteration of 100 bets.

```
# run the simulation 10,000 times
mc2.results <- replicate(runs, parlay.bet2())

# return a summary of the results
summary(mc2.results)

 Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
-7969.9 -2187.9  -935.1  -821.2   405.4 10743.2 
```

![](/Users/jake_macbook_pro/Dropbox/jstoetz.github.io/jstoetz.github.io/assets/img/mc2results-plot-parlay.png)

The results were much the same.  Most of the results from the first and second Monte Carlo run were basically the same with an average loss after each iteration of 100 bets of around $820.

### 3.3 Monte Carlo Run #3 - Random 2-Game Parlays - Same Game and Take Only Favorites

Since you have wasted enough of your Uncle's money (I know, how long can I keep up this awful analogy?) you go back to him one more time and ask for *one more shot* because you have an amazing, thouroughly researched idea.

This time you will have all of the same limitations as Run #1 and Run #2 but with the following rules:
* Only bet the favorite to cover the spread and win the ML bet.

The new function is codified in ```parlay.bet3()```.

```
> # run the simulation 10,000 times
> mc3.results <- replicate(runs, parlay.bet3())
> # return a summary of the results
> summary(mc3.results)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  -2443    1948    2884    2886    3823    8534 
```

Jackpot! You finally have found a system that, on average, makes money.

![](/Users/jake_macbook_pro/Dropbox/jstoetz.github.io/jstoetz.github.io/assets/img/mc3results-plot-parlay.png)

Buoyed by your run of success, you try 2 more systems.

### 3.4 Monte Carlo Run #4 - Random 2-Game Parlays - Same Game and Take Only Underdogs

Same limitations as Run #1 and Run #2 but with the following rules:
* Only bet the underdog to cover the spread AND win the ML bet.

This is less intuitive than taking the favorite to cover and win outright because generally the spread favorite is also the ML favorite (the common thought being that the team that is giving points will be more likely to win the game). The hypothesis with Monte Carlo run #4 is that even though you might not have as high a winning percentage for a ML bet on the underdog, the better odds on the ML will give you a better overall return.

Turns out, the hypothesis was correct:

```
> summary(mc4.results)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  -4206    5173    7241    7373    9414   23604 
```

![](/Users/jake_macbook_pro/Dropbox/jstoetz.github.io/jstoetz.github.io/assets/img/mc4results-plot-parlay.png)

### 3.5 Monte Carlo Run #5 - Random 2-Game Parlays - Same Game and Home Team for Both Spread and ML

Same limitations as Run #1 and Run #2 but with the following rules:
* Only bet the home team, regardless of favorite or underdog status

```
> summary(mc5.results)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  -2325    3573    4856    4928    6203   14855 
```

![](/Users/jake_macbook_pro/Dropbox/jstoetz.github.io/jstoetz.github.io/assets/img/mc5results-plot-parlay.png)

### 3.6 Monte Carlo Run #6 - Random 2-Game Parlays - Same Game and Visitor Team for Both Spread and ML

Same limitations as Run #1 and Run #2 but with the following rules:
* Only bet the visitor team, regardless of favorite or underdog status

```
> summary(mc6.results)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  -2738    5362    7224    7351    9192   23241
```

![](/Users/jake_macbook_pro/Dropbox/jstoetz.github.io/jstoetz.github.io/assets/img/mc6results-plot-parlay.png)

### 3.7 Summary of Monte Carlo Runs #1-6

#### Table: All Monte Carlo Results

|      name| total runs| no. profitable runs| no. losing runs| percent profitable runs| avg profit or loss per run| avg win per run| avg loss per run|
|---------:|----------:|-------------------:|---------------:|-----------------------:|--------------------------:|---------------:|----------------:|
| Run No. 1|      1e+05|              31,722|          68,278|                   31.72|                    -820.39|        1,411.57|        -1,857.36|
| Run No. 2|      1e+05|              31,682|          68,318|                   31.68|                    -821.18|        1,412.32|        -1,856.95|
| Run No. 3|      1e+05|              98,293|           1,707|                   98.29|                   2,886.17|        2,944.96|          -499.59|
| Run No. 4|      1e+05|              99,504|             496|                   99.50|                   7,372.52|        7,413.20|          -788.35|
| Run No. 5|      1e+05|              99,664|             336|                   99.66|                   4,927.97|        4,946.44|          -550.17|
| Run No. 6|      1e+05|              99,820|             180|                   99.82|                   7,350.55|        7,364.89|          -601.17|

To summarize the Monte Carlo runs:

* Each iteration is a series of 100 bets on 2-game parlays (moneyline and spread) at $100 total per parlay bet
* Each run consisted of replicating each 100-bet iteration 100,000 times
* Each run had different limitations:
  + Run #1: no limits on the game or which team to bet on for each individual component of the 2-game parlay
  + Run #2: each parlay bet must be from the same game (ie, ML and Spread must be on the same game)
  + Run #3: same game and only favorites for both ML and Spread
  + Run #4: same game and only underdogs for both ML and Spread
  + Run #5: same game and only home teams for both ML and Spread
  + Run #6: same game and only visitor teams for both ML and Spread

My analysis regarding the results:

* **Same Game Effect Seems Insignificant.** I would have guessed that betting the same game (both the ML and Spread from the same game) would have had a bigger effect on the outcome.  Run No. 1 reflects betting from any game for the spread and the ML, and Run No. 2 reflects choosing the same game for each bet. The results were virtually the same.  This tells me that there is some indication that there is no advantage **solely** from picking each part of the 2-game parlay bet from the same game (but there might be).

* **Home/Visitor and Favorite/Underdog is Meaningful.** I wasn't anticipating that the favorite/underdog and the home/visitor choice would have such a large effect on the outcome. My take is that the favorite/underdog designation is more important to the outcome than the home/visitor status of the winning team.  Based on the data, the Home team is favored on the ML 66.4% of the time and on the Spread 66.9% of the time. The Home team is also an almost 3 point favorite over the visitor. By chosing the Home team, you are implicitly also choosing the favorite on the ML and the Spread the majority of the time.

#### Table: Summary of Opening Odds for 2-Game Parlay

|      Description|      Home|  Visitor|
|----------------:|---------:|--------:|
|     ML Fav (No.)| 11,983.00| 6,061.00|
| Spread Fav (No.)| 12,074.00| 5,970.00|
|     ML Fav (Pct)|     66.41|    33.59|
| Spread Fav (Pct)|     66.91|    33.09|
|       Avg Spread|     -2.89|     2.89|
|           Avg ML|   -264.14|   128.90|

* **The Odds Favor the Underdog/Visitor.** It is clear that the underdog (and by extension the visitor in this dataset) is the better bet.  In other words, the historical performance of the underdog does not match what the book offers.  A few facts from the data (check the table below if you want to skip these points):
  + The Home team is favored on the ML 66.4% of the time, but it only wins the ML bet 58.8% of the time. In American odds, the Home Team is generally offered at -198 but it should be offered at -143. In other words, you have to bet more ($198 to win $100) than you should have if the books offered fair odds ($143 to win $100).  You must risk $56 more per bet on average than you should *assuming the book offered fair odds*.
  + The Visiting team is favored on the ML 33.6% of the time (American Odds of 197.6), but it wins the ML bet 41.1% of the time which converts to American Odds of +143. In other words, on a single bet basis, you win $54.60 more for each bet than fair odds would imply.

```
Home.EV <- .588*100 -(1-.588)*264
Vis.EV <- .411*128.9-(1-.411)*100
```
  
  + The Home team is offered on the ML at an average of -264 which implies a winning probability of 72.5%.
  + The Visitor team is offered on the ML at an average of +128 which implies a winning probability of 43.68%.
  + However, when the Home team wins, the average ML odds are -432.7 which implies a winning probability of 81.22%.
  + When the Visitor team wins, the average ML odds are -54.8 which implies a winning probability of 35.4%.
  
#### Table: Summary of Winning Outcome for 2-Game Parlay

|                     Description|    Home| Visitor|
|-------------------------------:|-------:|-------:|
|            Avg No - Winner - ML|    0.59|    0.41|
| Avg ML - Winner - American Odds| -432.70|  -54.83|
|  Avg ML - Winner - Implied Odds|   81.23|   35.41|

## 4 Additional Monte Carlo Testing

Based on my analysis of the first 6 Monte Carlo runs, adding certain limitations to the parlay bets for re-testing should be instructive.

More specifically:

* Pick home team ML when -140 or better on ML and home team spread
* Pick visitor team ML when +140 or better on ML and visitor team spread
* re-run Run #3 and Run #4 without the same game limitation to see "how much" the same game matters
* run where you can pick 2 ML bets and/or 2 Spread bets

### 4.1 Monte Carlo Run #7 - Same Game and Home Team Only When Odds Greater than -140 and Home Spread

```
  Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
 -354.6  6315.0  8031.9  8117.8  9834.1 18697.7 
```

![](/Users/jake_macbook_pro/Dropbox/jstoetz.github.io/jstoetz.github.io/assets/img/mc7results-plot-parlay.png)

### 4.2 Monte Carlo Run #8 - Same Game and Visitor Team Only When Odds Greater than +140 and Visitor Spread

```
summary(mc8.results)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  -4196    5616    7955    8116   10446   27685 
```

![](/Users/jake_macbook_pro/Dropbox/jstoetz.github.io/jstoetz.github.io/assets/img/mc8results-plot-parlay.png)

### 4.3 Monte Carlo Run #9 - Different Games - Home Team Only When Odds Greater than -142 - Both ML Games - No Spread Bet

```
> summary(mc9.results)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
-8457.6 -2509.1  -861.0  -649.6   996.6 22219.8 
```

![](/Users/jake_macbook_pro/Dropbox/jstoetz.github.io/jstoetz.github.io/assets/img/mc9results-plot-parlay.png)

### 4.4 Monte Carlo Run #10 - Different Games - Visitor Team Only When Odds Greater than +140 - Both ML Games - No Spread Bet

```
> summary(mc10.results)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
 -10000   -4099   -1490    -980    1575   23774 
```

![](/Users/jake_macbook_pro/Dropbox/jstoetz.github.io/jstoetz.github.io/assets/img/mc10results-plot-parlay.png)

### 4.5 Monte Carlo Run #11 - 4 bet parlay - Visitor Only

```
> summary(mc11.results)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  -4645   12806   18931   20242   26203   96376 
```

![](/Users/jake_macbook_pro/Dropbox/jstoetz.github.io/jstoetz.github.io/assets/img/mc11.results-plot-parlay.png)

### 4.6 Monte Carlo Run #12 - 4 bet parlay - Home Only

```
> summary(mc12.results)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  -6139    8311   11715   12206   15610   56056  
```

![](/Users/jake_macbook_pro/Dropbox/jstoetz.github.io/jstoetz.github.io/assets/img/mc12.results-plot-parlay.png)

### 4.7 Monte Carlo Run #13 - 2 Bet - Home or Visitor - +200 or better ML

One way to look at the distribution of odds is to determine the best ML underdogs. These are the bets that payoff the highest amounts.  In the span of the betting data, the top quartile of ML underdogs are offered at +200 or greater.  American odds of +200 imply a probability of approximately 33%.

```
> summary(c(mc.data3$Home.ML,mc.data3$Visitor.ML))
     Min.   1st Qu.    Median      Mean   3rd Qu.      Max. 
-13000.00   -240.00   -110.00    -67.62    200.00   6500.00 
```

For this run, I put together a list of:

* Home and Visitor underdogs that have a ML greater than 200 (ie the top quartile of ML offered)
* 2-bet parlay
* Bet both home and visitor
* Spread bet is the ML dog

```
> summary(mc13.results)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  -4349    5414    7979    8122   10637   27332
```

![](/Users/jake_macbook_pro/Dropbox/jstoetz.github.io/jstoetz.github.io/assets/img/mc13.results-plot-parlay.png)

### 4.8 Monte Carlo Run #14 - 4 Bet - Home or Visitor - +200 or better ML

* Home and Visitor underdogs that have a ML greater than 200 (ie the top quartile of ML offered)
* 4-bet parlay
* Bet both home and visitor
* Spread bet is the ML dog

```
> summary(mc14.results)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
 -10000    9197   20133   22754   32875  169971
 ```

![](/Users/jake_macbook_pro/Dropbox/jstoetz.github.io/jstoetz.github.io/assets/img/mc14.results-plot-parlay.png)

### 4.9 Monte Carlo Run #15 - 8-Bet - Visitor Only

* Visitor underdogs
* 8-bet parlay
* ML and Spread bet in same game
* Spread bet is the ML dog

```
> summary(mc15.results)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
 -10000    9718   46257   81140  108291 6421301 
```

![](/Users/jake_macbook_pro/Dropbox/jstoetz.github.io/jstoetz.github.io/assets/img/mc15.results-plot-parlay.png)

### 4.10 Summary of Monte Carlo Runs 1 - 15

Below is a summary of all of the Monte Carlo runs for the NBA data for 2007 to the present.

* Overview of each run:
  + Run #1: no limits on the game or which team to bet on for each individual component of the 2-game parlay
  + Run #2: each parlay bet must be from the same game (ie, ML and Spread portion of the parlay bet must be on the same game)
  + Run #3: same game and only favorites for both ML and Spread
  + Run #4: same game and only underdogs for both ML and Spread
  + Run #5: same game and only home teams for both ML and Spread
  + Run #6: same game and only visitor teams for both ML and Spread
  + Run #7: Same Game and Home Team Only When Odds Greater than -142 and Home Spread
  + Run #8: Same Game and Visitor Team Only When Odds Greater than +140 and Visitor Spread
  + Run #9: Different Games - Home Team Only When Odds Greater than -142 - Both ML Games - No Spread Bet
  + Run #10: Different Games - Visitor Team Only When Odds Greater than +140 - Both ML Games - No Spread Bet
  + Run #11: Same Game - 4-bet parlay - Visitor Only
  + Run #12: Same Game - 4-bet parlay - Home Only
  + Run #13: Same Game - 2-Bet parlay - Home or Visitor - +200 or better ML
  + Run #14: Same Game - 4-Bet parlay - Home or Visitor - +200 or better ML
  + Run #15: Same Game - 8-Bet parlay - Visitor Only

#### Table: All Monte Carlo Results Runs 1 - 15

|       name| total runs| no. profitable runs| no. losing runs| percent profitable runs| avg profit or loss per run| avg win per run| avg loss per run|
|----------:|----------:|-------------------:|---------------:|-----------------------:|--------------------------:|---------------:|----------------:|
|  Run No. 1|      1e+05|              31,722|          68,278|                   31.72|                    -820.39|        1,411.57|        -1,857.36|
|  Run No. 2|      1e+05|              31,682|          68,318|                   31.68|                    -821.18|        1,412.32|        -1,856.95|
|  Run No. 3|      1e+05|              98,293|           1,707|                   98.29|                   2,886.17|        2,944.96|          -499.59|
|  Run No. 4|      1e+05|              99,504|             496|                   99.50|                   7,372.52|        7,413.20|          -788.35|
|  Run No. 5|      1e+05|              99,664|             336|                   99.66|                   4,927.97|        4,946.44|          -550.17|
|  Run No. 6|      1e+05|              99,820|             180|                   99.82|                   7,350.55|        7,364.89|          -601.17|
|  Run No. 7|      1e+04|               9,999|               1|                   99.99|                   8,117.82|        8,118.67|          -354.56|
|  Run No. 8|      1e+05|              99,350|             650|                   99.35|                   8,115.70|        8,174.83|          -921.68|
|  Run No. 9|      1e+04|               3,773|           6,227|                   37.73|                    -649.64|        2,061.12|        -2,292.12|
| Run No. 10|      1e+04|               3,614|           6,386|                   36.14|                    -979.97|        3,594.20|        -3,568.61|
| Run No. 11|      1e+04|               9,951|              49|                   99.51|                  20,241.72|       20,349.35|        -1,614.84|
| Run No. 12|      1e+04|               9,945|              55|                   99.45|                  12,206.48|       12,281.72|        -1,397.68|
| Run No. 13|      1e+04|               9,873|             127|                   98.73|                   8,122.37|        8,239.67|          -996.29|
| Run No. 14|      1e+04|               9,216|             784|                   92.16|                  22,753.62|       25,042.22|        -4,149.10|
| Run No. 15|      5e+04|              41,459|           8,541|                   82.92|                  81,139.91|       99,635.94|        -8,641.98|

## 5 Final Observations and Future Opportunities

### 5.1 Final Observations

The results from these basic tests gave me a solid understanding of how parlay bets in sports betting work. Although much of the analysis presented here is from NBA games, the overall trend stayed the same when I ran the NFL, NCAAF and NCAAB data through the system. Note that the outcome for MC Runs 1-15 for the NFL, NCAAF and NCAAB are included below.  The MC analysis showed that an underdog ML bet and an underdog spread bet can be profitable over a wide range of 100-bet iterations.  A 2-game or 4-game parlay on the Visitor and/or underdog seems like the best route with a large percentage of 100-bet iterations that are profitable.  Single game plays, without a significant edge (likely greater than 55%), are not as profitable over the same random sample size.

* **For NBA Data, the Visitor Underdog is the Best Bet** As noted above, the Visiting team is favored on the ML 33.6% of the time (American Odds of 197.6), but it wins the ML bet 41.1% of the time. In plain terms, a Visitor that is a ML dog offers better value since they are more likely to win that what the odds would imply.  The corollary to betting the ML is that the spread bet for that team would also cover since the ML dog is (almost) always getting points.  For example, Team A (Home) vs Team B (Visitor) where Team A is -200 ML and -5 (-110) on the Spread and Team B is +240 ML and +5 (-110) on the Spread.  If Team B wins the game, the spread doesn't matter since they don't need the extra points to win the bet.  A 2-game parlay on Team B at +240 ML and +5 (-110) on the Spread would net $550 in profit.  

* **Random Sampling Was Useful.** Using Monte Carlo analysis and picking the games at random helped to expose some of the available opportunities inherent in the bookmaker's odds.  Any in-season bias (from either knowing the teams or catching a "hot"" team) was avoided since the runs were executed across multiple seasons.

* **Money Management Could Help** No money management system was used.  Each bet amount was the same flat amount, regardless of the perceived edge or the profit or loss of the system.

* **Better Data.** All Spread bets were assumed to be -110.  This is a function of the betting data I used. In reality, the odds could range (likely from 100 to -115) and the spread could give better or worse implied probabilities. I have sourced a few other providers (see Notes & Research section below) so 

* **Closing Line Data.** All bets (both the Spread and the ML) were taken at the Opening line. The data is available, this was just a function of me not having enough time to re-run the analysis.  In reality, you would be hard pressed to always get a bet at the opening line.

* **Number of Runs.** There is likely a more "scientific" way of determining the number of iterations for a given run.  I initially started with 100,000.  However, this became very burdensome from a compute standpoint and I noticed that the outcome (when averaged) was not significantly different.  Thus the reduction in runs to 10,000 for later Monte Carlo results.

### 5.2 Future ideas and Additional Research:

* **The Closing Line Issue.** Re-run Monte Carlo Runs 1-15 at Closing Lines (note after initially running the Opening lines: the Appendix below shows a summary for NFL data for Closing Lines - it appears that the line movement has little effect from an overall standpoint on the outcome of the "system"),
* **Horizontal Analysis of Game Line Difference.** Analyze the profitability in 2-game, 4-game and 8-game parlays based on the difference between the ML odds of the favorite and the underdog between the two bets.
  + For instance, consider the following scenarios for a 4-bet parlay:
    + Bet A: Game 1 - Visitor at +140 ML and +4 (-110). Game 2 - Visitor at +140 ML and +4 (-110)
    + Bet B: Game 1 - Visitor at +110 ML and +2 (-110). Game 2 - Visitor at +170 ML and +8 (-110)
    + Bet A profit: $1999. Bet B profit: $1966.
    + The difference in the Game 1 and Game 2 ML for Bet A is 0 and for Bet B is +60.
    + Is there any advantage in Game 1 versus Game 2, considering the win amount is the same?
* **Our Friend Linear Regression.** Add in Multiple Variate Regression analysis to identify a systematic approach to choosing games and teams to bet on,
* **Multiple Sports Parlays.** Possibly add in multiple sports parlays (ie, a 2-bet parlay in the NFL with a 2-bet parlay in the NBA) if profitable trends are identified.
* **Money Management.** Add in Money Management components - betting bigger or smaller based on the situation or on the probability of a given outcome.

## 6 NFL Monte Carlo Runs Summary
<hr/>

#### Table: NFL - Summary of Opening Odds for 2-Game Parlay

|      Description|     Home|  Visitor|
|----------------:|--------:|--------:|
|     ML Fav (No.)| 3,199.00| 1,580.00|
| Spread Fav (No.)| 3,269.00| 1,510.00|
|     ML Fav (Pct)|    66.94|    33.06|
| Spread Fav (Pct)|    68.40|    31.60|
|       Avg Spread|    -2.39|     2.39|
|           Avg ML|  -140.81|    78.04|

#### Table: NFL - Summary of Winning Outcome for 2-Game Parlay

|                     Description|    Home| Visitor|
|-------------------------------:|-------:|-------:|
|            Avg No - Winner - ML|    0.56|    0.44|
| Avg ML - Winner - American Odds| -234.38|  -24.90|
|  Avg ML - Winner - Implied Odds|   70.09|   19.94|

#### Table: NFL - All Monte Carlo Results

|       name| total runs| no. profitable runs| no. losing runs| percent profitable runs| avg profit or loss per run| avg win per run| avg loss per run|
|----------:|----------:|-------------------:|---------------:|-----------------------:|--------------------------:|---------------:|----------------:|
|  Run No. 1|     10,000|               3,193|           6,807|                   31.93|                    -804.41|        1,198.13|        -1,743.75|
|  Run No. 2|     10,000|               3,136|           6,864|                   31.36|                    -820.40|        1,239.42|        -1,761.48|
|  Run No. 3|     10,000|               9,366|             634|                   93.66|                   2,131.05|        2,314.68|          -581.66|
|  Run No. 4|     10,000|               9,972|              28|                   99.72|                   7,017.33|        7,038.84|          -646.62|
|  Run No. 5|     10,000|               9,949|              51|                   99.49|                   4,644.18|        4,670.60|          -510.37|
|  Run No. 6|     10,000|               9,995|               5|                   99.95|                   7,792.99|        7,797.06|          -333.17|
|  Run No. 7|     10,000|               9,992|               8|                   99.92|                   7,252.28|        7,258.45|          -449.06|
|  Run No. 8|     10,000|               9,984|              16|                   99.84|                   8,146.81|        8,160.80|          -583.17|
|  Run No. 9|     10,000|               3,131|           6,869|                   31.31|                  -1,072.18|        1,641.03|        -2,308.90|
| Run No. 10|     10,000|               3,689|           6,311|                   36.89|                    -815.49|        2,721.69|        -2,883.09|
| Run No. 11|     10,000|               9,991|               9|                   99.91|                  21,868.44|       21,888.85|          -785.71|
| Run No. 12|     10,000|               9,941|              59|                   99.41|                  11,450.07|       11,526.84|        -1,485.26|
| Run No. 13|     10,000|               9,878|             122|                   98.78|                   7,120.17|        7,220.46|          -999.87|
| Run No. 14|     10,000|               9,341|             659|                   93.41|                  19,459.09|       21,085.12|        -3,589.13|
| Run No. 15|     10,000|               9,254|             746|                   92.54|                  90,942.53|       98,965.81|        -8,584.92|

## 7 NCAAB Monte Carlo Runs Summary
<hr/>

#### Table: NCAAB - Summary of Opening Odds for 2-Game Parlay

|      Description|      Home|  Visitor|
|----------------:|---------:|--------:|
|     ML Fav (No.)|  8,070.00| 5,468.00|
| Spread Fav (No.)|  7,383.00| 6,155.00|
|     ML Fav (Pct)|     59.61|    40.39|
| Spread Fav (Pct)|     54.54|    45.46|
|       Avg Spread|     -4.21|     4.21|
|           Avg ML| -3,353.78| 1,049.07|

#### Table: NCAAB - Summary of Winning Outcome for 2-Game Parlay

|                     Description|      Home| Visitor|
|-------------------------------:|---------:|-------:|
|            Avg No - Winner - ML|      0.61|    0.39|
| Avg ML - Winner - American Odds| -5,606.88| -572.85|
|  Avg ML - Winner - Implied Odds|     98.25|   85.14|

#### Table: NCAAB - All Monte Carlo Results

|       name| total runs| no. profitable runs| no. losing runs| percent profitable runs| avg profit or loss per run| avg win per run| avg loss per run|
|----------:|----------:|-------------------:|---------------:|-----------------------:|--------------------------:|---------------:|----------------:|
|  Run No. 1|     10,000|               2,406|           7,594|                   24.06|                  -1,156.59|        2,381.13|        -2,277.45|
|  Run No. 2|     10,000|               2,433|           7,567|                   24.33|                  -1,110.07|        2,528.77|        -2,280.06|
|  Run No. 3|     10,000|               6,988|           3,012|                   69.88|                     667.90|        1,296.86|          -791.32|
|  Run No. 4|     10,000|               9,494|             506|                   94.94|                   5,700.05|        6,064.53|        -1,138.79|
|  Run No. 5|     10,000|               9,884|             116|                   98.84|                   4,604.60|        4,665.67|          -599.31|
|  Run No. 6|     10,000|               9,647|             353|                   96.47|                   5,882.13|        6,129.69|          -883.34|
|  Run No. 7|     10,000|               9,938|              62|                   99.38|                   6,781.59|        6,828.42|          -723.77|
|  Run No. 8|     10,000|               9,529|             471|                   95.29|                   7,823.57|        8,270.69|        -1,222.24|
|  Run No. 9|     10,000|               2,489|           7,511|                   24.89|                  -1,588.46|        3,069.56|        -3,132.03|
| Run No. 10|     10,000|               2,635|           7,365|                   26.35|                  -1,184.19|        9,793.87|        -5,111.84|
| Run No. 11|     10,000|               9,120|             880|                   91.20|                  14,782.22|       16,425.96|        -2,252.91|
| Run No. 12|     10,000|               9,887|             113|                   98.87|                  11,243.45|       11,385.37|        -1,173.96|
| Run No. 13|     10,000|               8,777|           1,223|                   87.77|                   5,414.94|        6,392.62|        -1,601.51|
| Run No. 14|     10,000|               7,076|           2,924|                   70.76|                  13,889.26|       21,900.51|        -5,497.72|
| Run No. 15|     10,000|               5,805|           4,195|                   58.05|                  43,336.03|       80,769.80|        -8,464.46|

## 8 NCAAF Monte Carlo Runs Summary
<hr/>

#### 

## Notes & Research

### Odds and Odds Converters
* American Odds (-110).  See [How to Read American Odds in Sports Betting](https://www.actionnetwork.com/education/american-odds).
* Decimal Odds (1.91).  See [Decimal Odds in Sports Betting, Explained](https://www.actionnetwork.com/education/decimal-odds).
* Fractional Odds (3/2). See [How to Read Fractional Odds](http://www.betsmart.co/how-to-read-fractional-odds/)
* Implied Probability. See [How to calculate implied probability in betting](https://help.smarkets.com/hc/en-gb/articles/214058369-How-to-calculate-implied-probability-in-betting)
* [What is the difference between odds and probability?](https://www.graphpad.com/support/faq/probability-vs-odds/)
* [What are the different betting odds formats?](https://help.smarkets.com/hc/en-gb/articles/214071849-What-are-the-different-betting-odds-formats-)
* [Sports Betting 101](https://www.williamhill.us/how-to-bet/sports-betting-101/)
* [How to Convert Betting Odds](https://help.smarkets.com/hc/en-gb/articles/214062929-How-to-convert-betting-odds)
* [How to Convert Odds](https://www.bettingexpert.com/academy/betting-fundamentals/betting-odds-explained)
* [Odds Converter on AceOdds](https://www.aceodds.com/bet-calculator/odds-converter.html)
* [Odds COnversion Formulas on BetSmart.co](http://www.betsmart.co/odds-conversion-formulas/#fractionaltoamerican)
* [Calculating Payouts From Moneyline Odds](https://www.gamblingsites.org/sports-betting/beginners-guide/odds/moneyline/)
* [The Math Behind Betting Odds and Gambling](https://www.investopedia.com/articles/dictionary/042215/understand-math-behind-betting-odds-gambling.asp)
* [Betting Odds Calculator](https://www.actionnetwork.com/betting-calculators/betting-odds-calculator)

### Betting & Odds Calculators
* [Smarket - Odds Converter](https://help.smarkets.com/hc/en-gb/articles/214062929-How-to-convert-betting-odds). Convert decimal, fractional, percentage and American odds.
* [Oddshark - Parlay Calculator](https://www.oddsshark.com/tools/parlay-calculator). Multiple game parlay calculator using American, decimal or fractional odds.
* [Calculate Stuff - Parlay Calculator](https://www.calculatestuff.com/miscellaneous/parlay-calculator). Calculate the payout for up to an 8-game parlay using American odds. Article below the calculator also does a good job discussing parlay math for moneyline bets.

### Historical Sports Data


## Appendix

### All NFL Charts & Tables - Opening Line

#### Table: NFL 2007 to Present - Summary of Home Team Data - Profit and Loss - Opening Line

|                            |           |
|:---------------------------|----------:|
|Home - Spread - Avg P/L     |      -4.91|
|Home - Spread - Max P/L     |     100.00|
|Home - Spread - Min P/L     |    -100.00|
|Home - Spread - Total P/L   | -23,481.82|
|Home - ML - Avg P/L         |      -5.45|
|Home - ML - Max P/L         |     800.00|
|Home - ML - Min P/L         |    -100.00|
|Home - ML - Total P/L       | -26,025.79|
|Home - 2-Parlay - Avg P/L   |      46.26|
|Home - 2-Parlay - Max P/L   |   1,618.18|
|Home - 2-Parlay - Min P/L   |    -100.00|
|Home - 2-Parlay - Total P/L | 221,085.78|

#### Table: NFL 2007 to Present - Summary of Visitor Team Data - Profit and Loss - Opening Line

|                               |           |
|:------------------------------|----------:|
|Visitor - Spread - Avg P/L     |       2.52|
|Visitor - Spread - Max P/L     |     100.00|
|Visitor - Spread - Min P/L     |    -100.00|
|Visitor - Spread - Total P/L   |  12,027.27|
|Visitor - ML - Avg P/L         |      -1.52|
|Visitor - ML - Max P/L         |   1,200.00|
|Visitor - ML - Min P/L         |    -100.00|
|Visitor - ML - Total P/L       |  -7,249.48|
|Visitor - 2-Parlay - Avg P/L   |      75.01|
|Visitor - 2-Parlay - Max P/L   |   2,381.82|
|Visitor - 2-Parlay - Min P/L   |    -100.00|
|Visitor - 2-Parlay - Total P/L | 358,470.88|

#### Table: NFL 2007 to Present - Summary of Home Team Data - Wins and Losses - Opening Line

|                                             |         |
|:--------------------------------------------|--------:|
|Home - Spread - Win Total                    | 2,220.00|
|Home - Spread - Loss Total                   | 2,406.00|
|Home - Spread - Total Games                  | 4,626.00|
|Home - Spread - Average Winning Percentage   |    47.99|
|Home - ML - Win Total                        | 2,658.00|
|Home - ML - Loss Total                       | 2,121.00|
|Home - ML - Total Games                      | 4,779.00|
|Home - ML - Average Winning Percentage       |    55.62|
|Home - 2-Parlay - Win Total                  | 2,048.00|
|Home - 2-Parlay - Loss Total                 | 2,731.00|
|Home - 2-Parlay - Total Games                | 4,779.00|
|Home - 2-Parlay - Average Winning Percentage |    42.85|

#### Table: NFL 2007 to Present - Summary of Visitor Team Data - Wins and Losses - Opening Line

|                                                |         |
|:-----------------------------------------------|--------:|
|Visitor - Spread - Win Total                    | 2,406.00|
|Visitor - Spread - Loss Total                   | 2,220.00|
|Visitor - Spread - Total Games                  | 4,626.00|
|Visitor - Spread - Average Winning Percentage   |    52.01|
|Visitor - ML - Win Total                        | 2,121.00|
|Visitor - ML - Loss Total                       | 2,658.00|
|Visitor - ML - Total Games                      | 4,779.00|
|Visitor - ML - Average Winning Percentage       |    44.38|
|Visitor - 2-Parlay - Win Total                  | 1,899.00|
|Visitor - 2-Parlay - Loss Total                 | 2,880.00|
|Visitor - 2-Parlay - Total Games                | 4,779.00|
|Visitor - 2-Parlay - Average Winning Percentage |    39.74|

#### Table: NFL 2007 to Present - Home Favorites - Summary by Spread Amount - Opening Line

| Home - Spread - Open| Home - Spread - PL - Mean| Home - Spread - PL - Max| Home - Spread - PL - Sum| Home - ML - PL - Mean| Home - ML - PL - Max| Home - ML - PL - Sum| Home - 2-Parlay - PL - Mean| Home - 2-Parlay - PL - Max| Home - 2-Parlay - PL - Sum|
|--------------------:|-------------------------:|------------------------:|------------------------:|---------------------:|--------------------:|--------------------:|---------------------------:|--------------------------:|--------------------------:|
|                 -0.5|                     -4.55|                    90.91|                    -9.09|                  2.50|               105.00|                 5.00|                       95.68|                     291.36|                     191.36|
|                 -1.0|                    -11.92|                   100.00|                -2,181.82|                -13.90|               175.00|            -2,543.21|                       48.05|                     425.00|                   8,794.06|
|                 -1.5|                    -14.55|                    90.91|                -1,527.27|                -10.87|               250.00|            -1,141.17|                       63.23|                     568.18|                   6,638.84|
|                 -2.0|                      1.25|                   100.00|                   127.27|                 -1.61|               125.00|              -164.36|                       67.76|                     329.55|                   6,911.13|
|                 -2.5|                    -18.07|                    90.91|                -4,463.64|                -11.97|               265.00|            -2,956.17|                       43.86|                     596.82|                  10,834.21|
|                 -3.0|                     -3.03|                   100.00|                -1,954.55|                 -8.00|               245.00|            -5,157.45|                       32.28|                     558.64|                  20,822.48|
|                 -3.5|                    -15.63|                    90.91|                -4,172.73|                -10.53|               220.00|            -2,810.95|                       31.91|                     510.91|                   8,519.63|
|                 -4.0|                      3.50|                   100.00|                   654.55|                  7.26|               350.00|             1,358.39|                       46.54|                     759.09|                   8,702.87|
|                 -4.5|                    -15.64|                    90.91|                -2,018.18|                -12.62|               200.00|            -1,628.31|                       23.42|                     222.57|                   3,021.40|
|                 -5.0|                      4.76|                   100.00|                   409.09|                 -8.43|                83.33|              -725.33|                       39.71|                     250.00|                   3,414.95|
|                 -5.5|                    -17.15|                    90.91|                -1,818.18|                 -6.80|               300.00|              -720.67|                       24.04|                     663.64|                   2,548.68|
|                 -6.0|                    -19.02|                   100.00|                -2,472.73|                 -4.00|               420.00|              -520.65|                        6.58|                     218.18|                     854.92|
|                 -6.5|                     15.86|                    90.91|                 2,300.00|                  0.10|                55.56|                15.20|                       57.01|                     196.97|                   8,266.94|
|                 -7.0|                      2.53|                   100.00|                   563.64|                 -0.84|                71.43|              -188.39|                       16.19|                     196.97|                   3,610.50|
|                 -7.5|                     -8.00|                    90.91|                  -663.64|                 -1.01|                41.67|               -83.89|                       17.49|                     170.45|                   1,451.27|
|                 -8.0|                    -14.10|                   100.00|                  -690.91|                  4.61|               360.00|               225.80|                       17.59|                     778.18|                     861.93|
|                 -8.5|                     -7.62|                    90.91|                  -236.36|                  9.56|                46.51|               296.49|                       15.02|                     150.57|                     465.49|
|                 -9.0|                      6.60|                   100.00|                   627.27|                  4.61|                58.82|               437.84|                       32.35|                     203.21|                   3,072.95|
|                 -9.5|                    -29.09|                    90.91|                -2,036.36|                 -8.32|                34.48|              -582.31|                      -13.03|                     152.49|                    -912.20|
|                -10.0|                    -12.25|                   100.00|                  -881.82|                 -4.04|                31.25|              -290.82|                       -2.32|                     145.45|                    -166.92|
|                -10.5|                     -4.55|                    90.91|                  -181.82|                  4.60|                33.33|               184.16|                       12.58|                     152.49|                     503.27|
|                -11.0|                     29.14|                    90.91|                   990.91|                  4.19|                27.03|               142.39|                       52.93|                     142.51|                   1,799.70|
|                -11.5|                     18.18|                    90.91|                   381.82|                 -1.28|                29.41|               -26.83|                       35.43|                     132.41|                     744.10|
|                -12.0|                    -16.60|                   100.00|                  -381.82|                -14.08|                21.98|              -323.84|                      -12.87|                     132.87|                    -296.05|
|                -12.5|                    -20.45|                    90.91|                  -490.91|                  7.87|                55.56|               188.98|                       -8.48|                     129.09|                    -203.49|
|                -13.0|                      5.23|                   100.00|                   209.09|                  1.86|                21.05|                74.42|                       12.69|                     129.09|                     507.68|
|                -13.5|                    -26.10|                    90.91|                  -809.09|                -12.81|                20.00|              -396.96|                      -16.52|                     122.73|                    -512.10|
|                -14.0|                      6.06|                    90.91|                   218.18|                 -3.37|                46.51|              -121.34|                       17.78|                     129.09|                     640.16|
|                -14.5|                    -10.16|                    90.91|                  -172.73|                  9.39|                17.54|               159.70|                       -4.17|                     110.00|                     -70.85|
|                -15.0|                    -23.64|                    90.91|                  -354.55|                 -5.03|                14.29|               -75.50|                      -16.50|                     118.18|                    -247.47|
|                -16.0|                    -36.36|                    90.91|                  -327.27|                -27.24|                18.18|              -245.18|                      -30.53|                     125.62|                    -274.80|
|                -16.5|                     19.32|                    90.91|                   154.55|                 -7.87|                 6.67|               -62.98|                       25.37|                     103.64|                     202.94|
|                -17.0|                    -36.36|                    90.91|                  -218.18|                  6.32|                12.50|                37.89|                      -34.14|                     100.45|                    -204.82|
|                -18.0|                   -100.00|                  -100.00|                  -100.00|                  9.09|                 9.09|                 9.09|                     -100.00|                    -100.00|                    -100.00|
|                -20.5|                   -100.00|                  -100.00|                  -100.00|                  4.17|                 4.17|                 4.17|                     -100.00|                    -100.00|                    -100.00|
|                -21.0|                     90.91|                    90.91|                    90.91|                  2.00|                 2.00|                 2.00|                       94.73|                      94.73|                      94.73|
|                -21.5|                   -100.00|                  -100.00|                  -100.00|                  2.50|                 2.50|                 2.50|                     -100.00|                    -100.00|                    -100.00|
|                -22.0|                   -100.00|                  -100.00|                  -100.00|                  3.33|                 3.33|                 3.33|                     -100.00|                    -100.00|                    -100.00|
|                -23.0|                   -100.00|                  -100.00|                  -100.00|                  4.00|                 4.00|                 4.00|                     -100.00|                    -100.00|                    -100.00|
|                -24.0|                   -100.00|                  -100.00|                  -100.00|                  2.00|                 2.00|                 2.00|                     -100.00|                    -100.00|                    -100.00|
|                -24.5|                   -100.00|                  -100.00|                  -100.00|                  7.14|                 7.14|                 7.14|                     -100.00|                    -100.00|                    -100.00|

#### Table: NFL 2007 to Present - Visitor Favorites - Summary by Spread Amount - Opening Line

| Visitor - Spread - Open| Visitor - Spread - PL - Mean| Visitor - Spread - PL - Max| Visitor - Spread - PL - Sum| Visitor - ML - PL - Mean| Visitor - ML - PL - Max| Visitor - ML - PL - Sum| Visitor - 2-Parlay - PL - Mean| Visitor - 2-Parlay - PL - Max| Visitor - 2-Parlay - PL - Sum|
|-----------------------:|----------------------------:|---------------------------:|---------------------------:|------------------------:|-----------------------:|-----------------------:|------------------------------:|-----------------------------:|-----------------------------:|
|                    -0.5|                        90.91|                       90.91|                       90.91|                   330.00|                  330.00|                  330.00|                         720.91|                        720.91|                        720.91|
|                    -1.0|                         4.15|                      100.00|                      481.82|                     4.10|                  200.00|                  475.37|                          80.64|                        472.73|                      9,354.70|
|                    -1.5|                         9.54|                       90.91|                      581.82|                     6.34|                  135.00|                  386.94|                          97.39|                        348.64|                      5,940.51|
|                    -2.0|                        27.12|                      100.00|                    1,600.00|                    21.04|                  195.00|                1,241.29|                          85.83|                        463.18|                      5,064.22|
|                    -2.5|                         7.39|                       90.91|                      827.27|                     0.56|                  275.00|                   62.23|                          84.14|                        615.91|                      9,423.54|
|                    -3.0|                         4.14|                      100.00|                    1,218.18|                    -4.06|                  285.00|               -1,193.99|                          38.16|                        530.00|                     11,218.21|
|                    -3.5|                        -4.55|                       90.91|                     -572.73|                    -2.05|                  190.00|                 -258.37|                          44.85|                        453.64|                      5,651.22|
|                    -4.0|                         5.70|                      100.00|                      381.82|                     3.54|                  200.00|                  237.38|                          56.03|                        472.73|                      3,754.12|
|                    -4.5|                         5.68|                       90.91|                      318.18|                     9.09|                  175.00|                  509.12|                          53.36|                        320.00|                      2,988.29|
|                    -5.0|                       -14.90|                      100.00|                     -536.36|                   -14.27|                   58.82|                 -513.66|                          11.62|                        203.21|                        418.41|
|                    -5.5|                        -7.27|                       90.91|                     -254.55|                     8.10|                   66.67|                  283.65|                          29.99|                        218.18|                      1,049.51|
|                    -6.0|                       -10.39|                       90.91|                     -509.09|                    -7.69|                   55.56|                 -376.58|                          22.80|                        186.36|                      1,117.22|
|                    -6.5|                       -23.64|                       90.91|                   -1,300.00|                    -6.54|                   62.50|                 -359.76|                           4.07|                        210.23|                        223.65|
|                    -7.0|                        -1.35|                      100.00|                     -100.00|                     4.14|                  260.00|                  306.11|                          18.36|                        587.27|                      1,359.00|
|                    -7.5|                         6.68|                       90.91|                      227.27|                    -4.45|                   50.00|                 -151.24|                          38.18|                        186.36|                      1,298.05|
|                    -8.0|                       -18.18|                       90.91|                     -254.55|                   -27.50|                   32.26|                 -385.01|                           4.48|                        152.49|                         62.70|
|                    -8.5|                       -18.18|                       90.91|                     -127.27|                    -9.06|                   36.36|                  -63.41|                           5.22|                        160.33|                         36.52|
|                    -9.0|                       -11.89|                       90.91|                     -309.09|                    14.45|                  210.00|                  375.80|                          10.99|                        164.34|                        285.62|
|                    -9.5|                       -29.67|                       90.91|                     -563.64|                    -3.69|                   30.30|                  -70.20|                         -12.96|                        148.76|                       -246.19|
|                   -10.0|                        -7.84|                       90.91|                     -227.27|                    -0.54|                   33.33|                  -15.60|                          10.46|                        142.51|                        303.31|
|                   -10.5|                       -49.09|                       90.91|                     -736.36|                   -14.37|                   23.26|                 -215.62|                         -40.76|                        127.27|                       -611.38|
|                   -11.0|                         4.13|                       90.91|                       45.45|                     7.63|                   27.78|                   83.94|                          23.57|                        143.94|                        259.22|
|                   -11.5|                      -100.00|                     -100.00|                     -300.00|                   -23.97|                   16.67|                  -71.90|                        -100.00|                       -100.00|                       -300.00|
|                   -12.0|                       -61.82|                       90.91|                     -309.09|                   -31.92|                   15.38|                 -159.62|                         -57.05|                        114.77|                       -285.23|
|                   -12.5|                        -4.55|                       90.91|                       -9.09|                   -40.91|                   18.18|                  -81.82|                          12.81|                        125.62|                         25.62|
|                   -13.0|                        27.27|                       90.91|                      163.64|                    -8.20|                   16.00|                  -49.19|                          38.35|                        111.00|                        230.10|
|                   -13.5|                       -23.64|                       90.91|                     -118.18|                   -11.49|                   20.00|                  -57.45|                         -18.21|                        106.82|                        -91.04|
|                   -14.0|                        27.27|                       90.91|                      163.64|                    -9.05|                   11.76|                  -54.29|                          38.07|                        112.12|                        228.44|
|                   -14.5|                        -4.55|                       90.91|                       -9.09|                     9.92|                   10.75|                   19.84|                           4.13|                        108.26|                          8.26|
|                   -15.0|                        -4.55|                       90.91|                       -9.09|                     6.55|                    9.09|                   13.09|                          -0.73|                         98.55|                         -1.45|
|                   -16.5|                        90.91|                       90.91|                       90.91|                    11.11|                   11.11|                   11.11|                         112.12|                        112.12|                        112.12|
|                   -20.0|                      -100.00|                     -100.00|                     -100.00|                     6.64|                    6.64|                    6.64|                        -100.00|                       -100.00|                       -100.00|

#### Table: NFL 2007 to Present - Home Underdogs - Summary by Spread Amount - Opening Line

| Home - Spread - Open| Home - Spread - PL - Mean| Home - Spread - PL - Max| Home - Spread - PL - Sum| Home - ML - PL - Mean| Home - ML - PL - Max| Home - ML - PL - Sum| Home - 2-Parlay - PL - Mean| Home - 2-Parlay - PL - Max| Home - 2-Parlay - PL - Sum|
|--------------------:|-------------------------:|------------------------:|------------------------:|---------------------:|--------------------:|--------------------:|---------------------------:|--------------------------:|--------------------------:|
|                  0.5|                   -100.00|                  -100.00|                  -100.00|               -100.00|                 -100|              -100.00|                     -100.00|                    -100.00|                    -100.00|
|                  1.0|                     -2.43|                   100.00|                  -281.82|                 -3.71|                  200|              -430.76|                       83.82|                     472.73|                   9,723.09|
|                  1.5|                    -18.63|                    90.91|                -1,136.36|                -15.67|                  170|              -956.09|                       60.99|                     415.45|                   3,720.18|
|                  2.0|                    -14.95|                   100.00|                  -881.82|                -29.66|                  275|            -1,750.11|                       34.28|                     615.91|                   2,022.52|
|                  2.5|                    -16.48|                    90.91|                -1,845.45|                -12.07|                  210|            -1,352.04|                       67.86|                     491.82|                   7,600.65|
|                  3.0|                      7.39|                   100.00|                 2,172.73|                  1.07|                  355|               315.10|                       92.96|                     768.64|                  27,328.82|
|                  3.5|                     -4.55|                    90.91|                  -572.73|                 -6.99|                  425|              -880.96|                       77.56|                     902.27|                   9,772.71|
|                  4.0|                     -8.55|                   100.00|                  -572.73|                 -8.98|                  290|              -601.74|                       73.76|                     644.55|                   4,942.13|
|                  4.5|                    -14.77|                    90.91|                  -827.27|                -26.79|                  245|            -1,500.00|                       39.77|                     558.64|                   2,227.27|
|                  5.0|                     11.62|                   100.00|                   418.18|                  9.01|                  235|               324.26|                      108.10|                     539.55|                   3,891.77|
|                  5.5|                     -1.82|                    90.91|                   -63.64|                -31.71|                  260|            -1,110.00|                       30.36|                     587.27|                   1,062.73|
|                  6.0|                      1.30|                    90.91|                    63.64|                 -3.67|                  285|              -179.76|                       83.91|                     635.00|                   4,111.36|
|                  6.5|                     14.55|                    90.91|                   800.00|                  2.44|                  295|               134.00|                       95.56|                     654.09|                   5,255.82|
|                  7.0|                      6.39|                   100.00|                   472.73|                -16.62|                  340|            -1,230.00|                       59.18|                     740.00|                   4,379.09|
|                  7.5|                    -15.78|                    90.91|                  -536.36|                  0.88|                  325|                30.00|                       92.59|                     711.36|                   3,148.18|
|                  8.0|                      9.09|                    90.91|                   127.27|                 61.79|                  325|               865.00|                      208.86|                     711.36|                   2,924.09|
|                  8.5|                      9.09|                    90.91|                    63.64|                 25.00|                  355|               175.00|                      138.64|                     768.64|                     970.45|
|                  9.0|                      2.80|                    90.91|                    72.73|                -12.50|                  450|              -325.00|                       67.05|                     950.00|                   1,743.18|
|                  9.5|                     20.57|                    90.91|                   390.91|                -18.54|                  500|              -352.22|                       55.52|                   1,045.45|                   1,054.85|
|                 10.0|                     -1.25|                    90.91|                   -36.36|                -25.64|                  458|              -743.61|                       41.96|                     965.27|                   1,216.75|
|                 10.5|                     40.00|                    90.91|                   600.00|                 24.25|                  800|               363.81|                      137.21|                   1,618.18|                   2,058.18|
|                 11.0|                    -13.22|                    90.91|                  -145.45|                -60.00|                  340|              -660.00|                      -23.64|                     740.00|                    -260.00|
|                 11.5|                     90.91|                    90.91|                   272.73|                183.33|                  750|               550.00|                      440.91|                   1,522.73|                   1,322.73|
|                 12.0|                     52.73|                    90.91|                   263.64|                 85.00|                  425|               425.00|                      253.18|                     902.27|                   1,265.91|
|                 12.5|                     -4.55|                    90.91|                    -9.09|                125.00|                  350|               250.00|                      329.55|                     759.09|                     659.09|
|                 13.0|                    -36.36|                    90.91|                  -218.18|                 29.17|                  675|               175.00|                      146.59|                   1,379.55|                     879.55|
|                 13.5|                     14.55|                    90.91|                    72.73|                 55.00|                  675|               275.00|                      195.91|                   1,379.55|                     979.55|
|                 14.0|                    -36.36|                    90.91|                  -218.18|                 -1.67|                  490|               -10.00|                       87.73|                   1,026.36|                     526.36|
|                 14.5|                     -4.55|                    90.91|                    -9.09|               -100.00|                 -100|              -200.00|                     -100.00|                    -100.00|                    -200.00|
|                 15.0|                     -4.55|                    90.91|                    -9.09|               -100.00|                 -100|              -200.00|                     -100.00|                    -100.00|                    -200.00|
|                 16.5|                   -100.00|                  -100.00|                  -100.00|               -100.00|                 -100|              -100.00|                     -100.00|                    -100.00|                    -100.00|
|                 20.0|                     90.91|                    90.91|                    90.91|               -100.00|                 -100|              -100.00|                     -100.00|                    -100.00|                    -100.00|

#### Table: NFL 2007 to Present - Visitor Underdogs - Summary by Spread Amount - Opening Line

| Visitor - Spread - Open| Visitor - Spread - PL - Mean| Visitor - Spread - PL - Max| Visitor - Spread - PL - Sum| Visitor - ML - PL - Mean| Visitor - ML - PL - Max| Visitor - ML - PL - Sum| Visitor - 2-Parlay - PL - Mean| Visitor - 2-Parlay - PL - Max| Visitor - 2-Parlay - PL - Sum|
|-----------------------:|----------------------------:|---------------------------:|---------------------------:|------------------------:|-----------------------:|-----------------------:|------------------------------:|-----------------------------:|-----------------------------:|
|                     0.5|                        -4.55|                       90.91|                       -9.09|                   -11.54|                   76.92|                  -23.08|                          68.88|                        237.76|                        137.76|
|                     1.0|                        13.11|                      100.00|                    2,400.00|                     8.24|                  200.00|                1,507.19|                         106.63|                        472.73|                     19,513.73|
|                     1.5|                         5.45|                       90.91|                      572.73|                     6.70|                  160.00|                  703.31|                         103.70|                        396.36|                     10,888.14|
|                     2.0|                        -6.24|                      100.00|                     -636.36|                    -8.11|                  220.00|                 -827.05|                          75.43|                        510.91|                      7,693.82|
|                     2.5|                         8.98|                       90.91|                    2,218.18|                     9.77|                  215.00|                2,413.49|                         109.56|                        501.36|                     27,062.11|
|                     3.0|                        11.77|                      100.00|                    7,590.91|                     4.52|                  280.00|                2,917.88|                          99.55|                        625.45|                     64,206.86|
|                     3.5|                         6.54|                       90.91|                    1,745.45|                     7.97|                  240.00|                2,126.93|                         106.12|                        549.09|                     28,333.22|
|                     4.0|                        -3.65|                      100.00|                     -681.82|                   -17.56|                  290.00|               -3,283.66|                          57.39|                        644.55|                     10,731.20|
|                     4.5|                         6.55|                       90.91|                      845.45|                    10.23|                  250.00|                1,319.34|                         110.43|                        568.18|                     14,246.01|
|                     5.0|                        -4.12|                      100.00|                     -354.55|                     8.85|                  340.00|                  761.00|                         107.80|                        740.00|                      9,271.00|
|                     5.5|                         8.06|                       90.91|                      854.55|                     7.03|                  305.00|                  745.00|                         104.33|                        673.18|                     11,058.64|
|                     6.0|                        14.76|                      100.00|                    1,918.18|                     2.46|                  260.00|                  319.50|                          95.60|                        587.27|                     12,428.14|
|                     6.5|                       -24.95|                       90.91|                   -3,618.18|                   -14.76|                  300.00|               -2,140.00|                          62.73|                        663.64|                      9,096.36|
|                     7.0|                         3.38|                      100.00|                      754.55|                   -10.67|                  400.00|               -2,380.00|                          70.53|                        854.55|                     15,729.09|
|                     7.5|                        -1.10|                       90.91|                      -90.91|                    -9.52|                  460.00|                 -790.00|                          72.74|                        969.09|                      6,037.27|
|                     8.0|                         9.28|                      100.00|                      454.55|                    -0.51|                  450.00|                  -25.00|                          89.94|                        950.00|                      4,406.82|
|                     8.5|                        -1.47|                       90.91|                      -45.45|                   -51.94|                  285.00|               -1,610.00|                          -8.24|                        635.00|                       -255.45|
|                     9.0|                       -13.49|                      100.00|                   -1,281.82|                   -28.68|                  450.00|               -2,725.00|                          36.15|                        950.00|                      3,434.09|
|                     9.5|                        20.00|                       90.91|                    1,400.00|                    20.54|                  600.00|                1,438.00|                         130.13|                      1,236.36|                      9,108.91|
|                    10.0|                         8.96|                      100.00|                      645.45|                    -8.72|                  664.00|                 -627.79|                          74.26|                      1,358.55|                      5,346.94|
|                    10.5|                        -4.55|                       90.91|                     -181.82|                   -37.38|                  450.00|               -1,495.00|                          19.56|                        950.00|                        782.27|
|                    11.0|                       -38.24|                       90.91|                   -1,300.00|                   -50.00|                  380.00|               -1,700.00|                          -4.55|                        816.36|                       -154.55|
|                    11.5|                       -27.27|                       90.91|                     -572.73|                   -22.38|                  635.00|                 -470.00|                          48.18|                      1,303.18|                      1,011.82|
|                    12.0|                        16.60|                      100.00|                      381.82|                    42.39|                  500.00|                  975.00|                         171.84|                      1,045.45|                      3,952.27|
|                    12.5|                        11.36|                       90.91|                      272.73|                   -38.54|                  700.00|                 -925.00|                          17.33|                      1,427.27|                        415.91|
|                    13.0|                        -9.09|                      100.00|                     -363.64|                   -53.00|                  600.00|               -2,120.00|                         -10.27|                      1,236.36|                       -410.91|
|                    13.5|                        17.01|                       90.91|                      527.27|                    46.21|                1,200.00|                1,432.50|                         179.13|                      2,381.82|                      5,552.95|
|                    14.0|                       -15.15|                       90.91|                     -545.45|                   -14.58|                  600.00|                 -525.00|                          63.07|                      1,236.36|                      2,270.45|
|                    14.5|                         1.07|                       90.91|                       18.18|                  -100.00|                 -100.00|               -1,700.00|                        -100.00|                       -100.00|                     -1,700.00|
|                    15.0|                        14.55|                       90.91|                      218.18|                    -6.67|                  800.00|                 -100.00|                          78.18|                      1,618.18|                      1,172.73|
|                    16.0|                        27.27|                       90.91|                      245.45|                   155.33|                  948.00|                1,398.00|                         387.45|                      1,900.73|                      3,487.09|
|                    16.5|                       -28.41|                       90.91|                     -227.27|                    25.00|                  900.00|                  200.00|                         138.64|                      1,809.09|                      1,109.09|
|                    17.0|                        27.27|                       90.91|                      163.64|                  -100.00|                 -100.00|                 -600.00|                        -100.00|                       -100.00|                       -600.00|
|                    18.0|                        90.91|                       90.91|                       90.91|                  -100.00|                 -100.00|                 -100.00|                        -100.00|                       -100.00|                       -100.00|
|                    20.5|                        90.91|                       90.91|                       90.91|                  -100.00|                 -100.00|                 -100.00|                        -100.00|                       -100.00|                       -100.00|
|                    21.0|                      -100.00|                     -100.00|                     -100.00|                  -100.00|                 -100.00|                 -100.00|                        -100.00|                       -100.00|                       -100.00|
|                    21.5|                        90.91|                       90.91|                       90.91|                  -100.00|                 -100.00|                 -100.00|                        -100.00|                       -100.00|                       -100.00|
|                    22.0|                        90.91|                       90.91|                       90.91|                  -100.00|                 -100.00|                 -100.00|                        -100.00|                       -100.00|                       -100.00|
|                    23.0|                        90.91|                       90.91|                       90.91|                  -100.00|                 -100.00|                 -100.00|                        -100.00|                       -100.00|                       -100.00|
|                    24.0|                        90.91|                       90.91|                       90.91|                  -100.00|                 -100.00|                 -100.00|                        -100.00|                       -100.00|                       -100.00|
|                    24.5|                        90.91|                       90.91|                       90.91|                  -100.00|                 -100.00|                 -100.00|                        -100.00|                       -100.00|                       -100.00|

### All NFL Charts and Tables - Closing Line

#### Table: NFL - Summary of Home Team Data - Profit and Loss - Closing Line

|                            |           |
|:---------------------------|----------:|
|Home - Spread - Avg P/L     |      -4.89|
|Home - Spread - Max P/L     |     100.00|
|Home - Spread - Min P/L     |    -100.00|
|Home - Spread - Total P/L   | -23,363.64|
|Home - ML - Avg P/L         |      -5.45|
|Home - ML - Max P/L         |     800.00|
|Home - ML - Min P/L         |    -100.00|
|Home - ML - Total P/L       | -26,025.79|
|Home - 2-Parlay - Avg P/L   |      45.91|
|Home - 2-Parlay - Max P/L   |   1,618.18|
|Home - 2-Parlay - Min P/L   |    -100.00|
|Home - 2-Parlay - Total P/L | 219,389.56|

#### Table: NFL - Summary of Visitor Team Data - Profit and Loss - Closing Line

|                               |           |
|:------------------------------|----------:|
|Visitor - Spread - Avg P/L     |       2.14|
|Visitor - Spread - Max P/L     |     100.00|
|Visitor - Spread - Min P/L     |    -100.00|
|Visitor - Spread - Total P/L   |  10,236.36|
|Visitor - ML - Avg P/L         |      -1.52|
|Visitor - ML - Max P/L         |   1,200.00|
|Visitor - ML - Min P/L         |    -100.00|
|Visitor - ML - Total P/L       |  -7,249.48|
|Visitor - 2-Parlay - Avg P/L   |      74.00|
|Visitor - 2-Parlay - Max P/L   |   2,381.82|
|Visitor - 2-Parlay - Min P/L   |    -100.00|
|Visitor - 2-Parlay - Total P/L | 353,647.21|

#### Table: NFL - Summary of Home Team Data - Wins and Losses - Closing Line

|                                             |         |
|:--------------------------------------------|--------:|
|Home - Spread - Win Total                    | 2,229.00|
|Home - Spread - Loss Total                   | 2,405.00|
|Home - Spread - Total Games                  | 4,634.00|
|Home - Spread - Average Winning Percentage   |    48.10|
|Home - ML - Win Total                        | 2,658.00|
|Home - ML - Loss Total                       | 2,121.00|
|Home - ML - Total Games                      | 4,779.00|
|Home - ML - Average Winning Percentage       |    55.62|
|Home - 2-Parlay - Win Total                  | 2,034.00|
|Home - 2-Parlay - Loss Total                 | 2,745.00|
|Home - 2-Parlay - Total Games                | 4,779.00|
|Home - 2-Parlay - Average Winning Percentage |    42.56|

#### Table: NFL Summary of Visitor Team Data - Wins and Losses - Closing Line

|                                                |         |
|:-----------------------------------------------|--------:|
|Visitor - Spread - Win Total                    | 2,405.00|
|Visitor - Spread - Loss Total                   | 2,229.00|
|Visitor - Spread - Total Games                  | 4,634.00|
|Visitor - Spread - Average Winning Percentage   |    51.90|
|Visitor - ML - Win Total                        | 2,121.00|
|Visitor - ML - Loss Total                       | 2,658.00|
|Visitor - ML - Total Games                      | 4,779.00|
|Visitor - ML - Average Winning Percentage       |    44.38|
|Visitor - 2-Parlay - Win Total                  | 1,879.00|
|Visitor - 2-Parlay - Loss Total                 | 2,900.00|
|Visitor - 2-Parlay - Total Games                | 4,779.00|
|Visitor - 2-Parlay - Average Winning Percentage |    39.32|

#### Table: NFL - Home Favorites - Summary by Spread Amount - Closing Line

| Home - Spread - Open| Home - Spread - PL - Mean| Home - Spread - PL - Max| Home - Spread - PL - Sum| Home - ML - PL - Mean| Home - ML - PL - Max| Home - ML - PL - Sum| Home - 2-Parlay - PL - Mean| Home - 2-Parlay - PL - Max| Home - 2-Parlay - PL - Sum|
|--------------------:|-------------------------:|------------------------:|------------------------:|---------------------:|--------------------:|--------------------:|---------------------------:|--------------------------:|--------------------------:|
|                 -1.0|                     -2.11|                   100.00|                  -272.73|                 -5.42|               135.00|              -698.76|                       72.42|                     348.64|                   9,342.73|
|                 -1.5|                    -14.74|                    90.91|                -1,518.18|                -12.49|               110.00|            -1,286.41|                       53.77|                     300.91|                   5,538.60|
|                 -2.0|                     -8.53|                   100.00|                  -963.64|                -10.71|               140.00|            -1,209.72|                       52.74|                     358.18|                   5,959.99|
|                 -2.5|                    -14.98|                    90.91|                -3,700.00|                -12.51|               155.00|            -3,090.53|                       45.97|                     386.82|                  11,354.59|
|                 -3.0|                     -6.25|                   100.00|                -3,872.73|                -10.16|               165.00|            -6,300.57|                       24.63|                     367.73|                  15,270.65|
|                 -3.5|                     -3.40|                    90.91|                  -854.55|                 -4.60|               185.00|            -1,154.47|                       49.41|                     444.09|                  12,401.16|
|                 -4.0|                      1.75|                   100.00|                   300.00|                  1.41|               185.00|               240.69|                       47.68|                     444.09|                   8,152.43|
|                 -4.5|                    -12.43|                    90.91|                -1,354.55|                 -3.64|               200.00|              -396.50|                       27.28|                     200.00|                   2,973.28|
|                 -5.0|                     -2.14|                   100.00|                  -227.27|                 -0.18|                48.78|               -19.33|                       21.01|                     184.04|                   2,227.09|
|                 -5.5|                      0.28|                    90.91|                    27.27|                  7.12|               265.00|               704.66|                       49.45|                     596.82|                   4,895.24|
|                 -6.0|                    -17.72|                   100.00|                -2,763.64|                -10.92|               245.00|            -1,704.26|                        6.89|                     549.09|                   1,075.37|
|                 -6.5|                     -5.99|                    90.91|                  -790.91|                 -0.81|                40.00|              -106.43|                       26.97|                     167.27|                   3,559.38|
|                 -7.0|                      3.95|                   100.00|                   818.18|                  0.54|                38.91|               111.36|                       16.89|                     165.19|                   3,496.10|
|                 -7.5|                     -2.01|                    90.91|                  -227.27|                  6.37|               350.00|               719.59|                       31.29|                     759.09|                   3,535.81|
|                 -8.0|                    -11.20|                   100.00|                  -627.27|                -11.97|                30.77|              -670.58|                        7.63|                     149.65|                     427.03|
|                 -8.5|                     -1.65|                    90.91|                   -54.55|                 -4.32|                33.90|              -142.49|                       23.61|                     154.55|                     778.99|
|                 -9.0|                     -6.94|                   100.00|                  -527.27|                 -5.54|                31.25|              -421.30|                       12.26|                     150.57|                     932.14|
|                 -9.5|                     -2.98|                    90.91|                  -181.82|                  9.85|               420.00|               600.68|                       17.83|                     141.82|                   1,087.64|
|                -10.0|                     -7.31|                   100.00|                  -636.36|                 -3.28|                26.32|              -285.52|                       -2.35|                     141.15|                    -204.09|
|                -10.5|                    -13.64|                    90.91|                  -572.73|                -15.34|                22.22|              -644.24|                        2.09|                     133.33|                      87.93|
|                -11.0|                     17.91|                    90.91|                   609.09|                  6.71|                20.00|               228.11|                       37.82|                     129.09|                   1,285.77|
|                -11.5|                    -16.00|                    90.91|                  -400.00|                 12.16|                23.81|               304.10|                       -2.56|                     125.62|                     -63.93|
|                -12.0|                     23.53|                    90.91|                   400.00|                  7.91|                16.67|               134.44|                       40.85|                     122.73|                     694.53|
|                -12.5|                    -33.18|                    90.91|                  -663.64|                -14.12|                16.67|              -282.42|                      -23.52|                     122.73|                    -470.32|
|                -13.0|                     14.81|                   100.00|                   518.18|                  6.47|                16.67|               226.40|                       23.20|                     121.21|                     811.98|
|                -13.5|                    -32.62|                    90.91|                -1,109.09|                -14.62|                14.81|              -497.05|                      -24.37|                     119.19|                    -828.45|
|                -14.0|                     18.92|                   100.00|                   700.00|                  7.56|                14.29|               279.86|                       25.41|                     118.18|                     940.00|
|                -14.5|                    -26.57|                    90.91|                  -345.45|                  1.48|                12.50|                19.26|                      -19.10|                     114.77|                    -248.34|
|                -15.0|                    -23.64|                    90.91|                  -118.18|                -12.28|                12.50|               -61.39|                      -17.91|                     106.82|                     -89.55|
|                -15.5|                    -18.18|                    90.91|                  -127.27|                  6.68|                 8.33|                46.77|                      -12.50|                     106.82|                     -87.50|
|                -16.0|                    -26.57|                    90.91|                  -345.45|                  7.63|                12.50|                99.14|                      -20.92|                     114.77|                    -271.99|
|                -16.5|                     -4.55|                    90.91|                   -36.36|                  6.36|                 7.41|                50.88|                        1.20|                     105.05|                       9.64|
|                -17.0|                     12.12|                   100.00|                   145.45|                -20.52|                 7.14|              -246.19|                        0.79|                     103.64|                       9.45|
|                -18.5|                     90.91|                    90.91|                    90.91|                  5.00|                 5.00|                 5.00|                      100.45|                     100.45|                     100.45|
|                -19.5|                     90.91|                    90.91|                    90.91|                  2.00|                 2.00|                 2.00|                       94.73|                      94.73|                      94.73|
|                -20.0|                     -4.55|                    90.91|                    -9.09|                  5.24|                 7.14|                10.48|                       -1.36|                      97.27|                      -2.73|
|                -20.5|                     -4.55|                    90.91|                    -9.09|                  3.53|                 4.17|                 7.05|                       -1.79|                      96.42|                      -3.58|
|                -21.0|                   -100.00|                  -100.00|                  -100.00|                  2.50|                 2.50|                 2.50|                     -100.00|                    -100.00|                    -100.00|
|                -22.5|                     -4.55|                    90.91|                    -9.09|                  3.00|                 4.00|                 6.00|                       -2.64|                      94.73|                      -5.27|
|                -23.5|                   -100.00|                  -100.00|                  -100.00|                  3.33|                 3.33|                 3.33|                     -100.00|                    -100.00|                    -100.00|
|                -26.5|                   -100.00|                  -100.00|                  -100.00|                  2.00|                 2.00|                 2.00|                     -100.00|                    -100.00|                    -100.00|

#### Table: NFL - Visitor Favorites - Summary by Spread Amount - Closing Line

| Visitor - Spread - Open| Visitor - Spread - PL - Mean| Visitor - Spread - PL - Max| Visitor - Spread - PL - Sum| Visitor - ML - PL - Mean| Visitor - ML - PL - Max| Visitor - ML - PL - Sum| Visitor - 2-Parlay - PL - Mean| Visitor - 2-Parlay - PL - Max| Visitor - 2-Parlay - PL - Sum|
|-----------------------:|----------------------------:|---------------------------:|---------------------------:|------------------------:|-----------------------:|-----------------------:|------------------------------:|-----------------------------:|-----------------------------:|
|                    -0.5|                        90.91|                       90.91|                      181.82|                   120.00|                  125.00|                  240.00|                         320.00|                        329.55|                        640.00|
|                    -1.0|                         4.55|                      100.00|                      500.00|                     0.69|                  105.00|                   75.62|                          76.42|                        291.36|                      8,406.53|
|                    -1.5|                         4.13|                       90.91|                      318.18|                     3.27|                  120.00|                  251.83|                          88.23|                        320.00|                      6,793.49|
|                    -2.0|                        -1.17|                      100.00|                      -81.82|                    -4.15|                  285.00|                 -290.40|                          48.76|                        247.39|                      3,413.33|
|                    -2.5|                         4.55|                       90.91|                      572.73|                     4.02|                  145.00|                  506.14|                          80.66|                        367.73|                     10,163.73|
|                    -3.0|                         1.87|                      100.00|                      645.45|                    -4.82|                  160.00|               -1,661.26|                          44.15|                        377.27|                     15,233.01|
|                    -3.5|                         5.71|                       90.91|                      690.91|                     1.37|                  190.00|                  165.57|                          67.49|                        453.64|                      8,166.81|
|                    -4.0|                        -6.46|                      100.00|                     -490.91|                    -9.76|                  230.00|                 -741.46|                          23.82|                        530.00|                      1,810.64|
|                    -4.5|                        17.79|                       90.91|                      836.36|                    21.49|                  200.00|                1,009.91|                          90.78|                        472.73|                      4,266.51|
|                    -5.0|                        -4.55|                       90.91|                     -154.55|                    -7.62|                   46.51|                 -259.18|                          36.35|                        179.70|                      1,235.97|
|                    -5.5|                       -14.70|                       90.91|                     -690.91|                    -7.12|                   45.45|                 -334.85|                          20.50|                        177.69|                        963.64|
|                    -6.0|                        33.77|                      100.00|                    2,127.27|                    31.15|                  260.00|                1,962.28|                          78.15|                        587.27|                      4,923.47|
|                    -6.5|                       -29.18|                       90.91|                   -1,809.09|                     0.94|                  280.00|                   58.42|                          -4.91|                        164.34|                       -304.30|
|                    -7.0|                         0.54|                      100.00|                       54.55|                    -6.41|                   41.67|                 -647.23|                          21.51|                        164.34|                      2,172.22|
|                    -7.5|                       -23.64|                       90.91|                     -945.45|                     3.11|                   34.48|                  124.58|                          -1.41|                        154.55|                        -56.53|
|                    -8.0|                        27.88|                      100.00|                      418.18|                     0.79|                   33.33|                   11.81|                          44.09|                        143.21|                        661.39|
|                    -8.5|                       -32.62|                       90.91|                     -554.55|                   -11.88|                   28.57|                 -201.95|                         -15.77|                        145.45|                       -268.13|
|                    -9.0|                       -12.50|                       90.91|                     -300.00|                    -1.19|                   28.57|                  -28.58|                           9.40|                        145.45|                        225.53|
|                    -9.5|                        11.36|                       90.91|                      136.36|                     2.08|                   24.69|                   24.98|                          36.55|                        138.05|                        438.59|
|                   -10.0|                       -21.39|                       90.91|                     -727.27|                    -4.75|                   23.26|                 -161.34|                          -6.01|                        133.81|                       -204.24|
|                   -10.5|                       -36.36|                       90.91|                     -436.36|                   -11.24|                   23.26|                 -134.83|                         -25.19|                        129.09|                       -302.28|
|                   -11.0|                       -20.45|                       90.91|                     -245.45|                    -2.57|                   20.00|                  -30.79|                          -6.52|                        129.09|                        -78.28|
|                   -11.5|                      -100.00|                     -100.00|                     -300.00|                    16.74|                   18.18|                   50.23|                        -100.00|                       -100.00|                       -300.00|
|                   -12.0|                        -4.55|                       90.91|                       -9.09|                   -42.31|                   15.38|                  -84.62|                          10.14|                        120.28|                         20.28|
|                   -12.5|                       -23.64|                       90.91|                     -118.18|                    -8.90|                   15.38|                  -44.50|                         -12.99|                        120.28|                        -64.95|
|                   -13.0|                       -36.36|                       90.91|                     -109.09|                    13.53|                   14.29|                   40.58|                         -27.59|                        117.24|                        -82.76|
|                   -13.5|                        27.27|                       90.91|                       81.82|                    12.51|                   14.29|                   37.52|                          44.08|                        118.18|                        132.23|
|                   -14.0|                       -11.19|                      100.00|                     -145.45|                     2.12|                   12.50|                   27.50|                         -18.58|                        114.77|                       -241.52|
|                   -14.5|                        14.55|                       90.91|                       72.73|                   -12.01|                   11.11|                  -60.03|                          26.63|                        112.12|                        133.13|
|                   -15.5|                        90.91|                       90.91|                      363.64|                     7.67|                    9.09|                   30.67|                         105.54|                        108.26|                        422.18|
|                   -16.0|                      -100.00|                     -100.00|                     -200.00|                  -100.00|                 -100.00|                 -200.00|                        -100.00|                       -100.00|                       -200.00|
|                   -16.5|                        27.27|                       90.91|                       81.82|                     6.85|                    7.14|                   20.54|                          35.80|                        104.55|                        107.39|
|                   -18.5|                        90.91|                       90.91|                       90.91|                     4.00|                    4.00|                    4.00|                          98.55|                         98.55|                         98.55|
|                   -19.5|                      -100.00|                     -100.00|                     -100.00|                     6.64|                    6.64|                    6.64|                        -100.00|                       -100.00|                       -100.00|

#### Table: NFL - Home Underdogs - Summary by Spread Amount - Closing Line

| Home - Spread - Open| Home - Spread - PL - Mean| Home - Spread - PL - Max| Home - Spread - PL - Sum| Home - ML - PL - Mean| Home - ML - PL - Max| Home - ML - PL - Sum| Home - 2-Parlay - PL - Mean| Home - 2-Parlay - PL - Max| Home - 2-Parlay - PL - Sum|
|--------------------:|-------------------------:|------------------------:|------------------------:|---------------------:|--------------------:|--------------------:|---------------------------:|--------------------------:|--------------------------:|
|                  0.5|                   -100.00|                  -100.00|                  -200.00|               -100.00|                 -100|              -200.00|                     -100.00|                    -100.00|                    -200.00|
|                  1.0|                     -4.13|                   100.00|                  -454.55|                 -9.42|                  115|            -1,036.31|                       72.92|                     310.45|                   8,021.59|
|                  1.5|                    -13.22|                    90.91|                -1,018.18|                -11.71|                  120|              -901.89|                       68.55|                     320.00|                   5,278.21|
|                  2.0|                      7.01|                   100.00|                   490.91|                 -0.51|                  125|               -36.00|                       89.93|                     329.55|                   6,294.91|
|                  2.5|                    -13.64|                    90.91|                -1,718.18|                -12.28|                  140|            -1,547.00|                       67.47|                     358.18|                   8,501.18|
|                  3.0|                      2.98|                   100.00|                 1,027.27|                  0.03|                  170|                11.00|                       90.97|                     415.45|                  31,384.64|
|                  3.5|                    -14.80|                    90.91|                -1,790.91|                 -5.12|                  190|              -620.00|                       81.13|                     453.64|                   9,816.36|
|                  4.0|                     11.12|                   100.00|                   845.45|                 11.58|                  190|               880.00|                      113.01|                     453.64|                   8,589.09|
|                  4.5|                    -26.89|                    90.91|                -1,263.64|                -33.57|                  210|            -1,578.00|                       26.81|                     491.82|                   1,260.18|
|                  5.0|                     -4.55|                    90.91|                  -154.55|                  4.06|                  220|               138.00|                       98.66|                     510.91|                   3,354.36|
|                  5.5|                      5.61|                    90.91|                   263.64|                  3.30|                  235|               155.00|                       97.21|                     539.55|                   4,568.64|
|                  6.0|                    -32.90|                   100.00|                -2,072.73|                -70.24|                  255|            -4,425.00|                      -43.18|                     577.73|                  -2,720.45|
|                  6.5|                     20.09|                    90.91|                 1,245.45|                  1.95|                  250|               121.00|                       94.63|                     568.18|                   5,867.36|
|                  7.0|                     -1.35|                   100.00|                  -136.36|                  4.05|                  295|               409.00|                       98.64|                     654.09|                   9,962.64|
|                  7.5|                     14.55|                    90.91|                   581.82|                -22.38|                  315|              -895.00|                       48.19|                     692.27|                   1,927.73|
|                  8.0|                    -23.03|                   100.00|                  -345.45|                -19.33|                  325|              -290.00|                       54.00|                     711.36|                     810.00|
|                  8.5|                     23.53|                    90.91|                   400.00|                 24.71|                  350|               420.00|                      138.07|                     759.09|                   2,347.27|
|                  9.0|                      3.41|                    90.91|                    81.82|                 -8.12|                  355|              -195.00|                       75.40|                     768.64|                   1,809.55|
|                  9.5|                    -20.45|                    90.91|                  -245.45|                -23.33|                  400|              -280.00|                       46.36|                     854.55|                     556.36|
|                 10.0|                     12.30|                    90.91|                   418.18|                  0.65|                  435|                22.00|                       92.14|                     921.36|                   3,132.91|
|                 10.5|                     27.27|                    90.91|                   327.27|                 27.92|                  450|               335.00|                      144.20|                     950.00|                   1,730.45|
|                 11.0|                     11.36|                    90.91|                   136.36|                 -3.50|                  500|               -42.00|                       84.23|                   1,045.45|                   1,010.73|
|                 11.5|                     90.91|                    90.91|                   272.73|               -100.00|                 -100|              -300.00|                     -100.00|                    -100.00|                    -300.00|
|                 12.0|                     -4.55|                    90.91|                    -9.09|                195.00|                  490|               390.00|                      463.18|                   1,026.36|                     926.36|
|                 12.5|                     14.55|                    90.91|                    72.73|                  5.00|                  425|                25.00|                      100.45|                     902.27|                     502.27|
|                 13.0|                     27.27|                    90.91|                    81.82|               -100.00|                 -100|              -300.00|                     -100.00|                    -100.00|                    -300.00|
|                 13.5|                    -36.36|                    90.91|                  -109.09|               -100.00|                 -100|              -300.00|                     -100.00|                    -100.00|                    -300.00|
|                 14.0|                     18.18|                   100.00|                   236.36|                -40.38|                  675|              -525.00|                       13.81|                   1,379.55|                     179.55|
|                 14.5|                    -23.64|                    90.91|                  -118.18|                 55.00|                  675|               275.00|                      195.91|                   1,379.55|                     979.55|
|                 15.5|                   -100.00|                  -100.00|                  -400.00|               -100.00|                 -100|              -400.00|                     -100.00|                    -100.00|                    -400.00|
|                 16.0|                     90.91|                    90.91|                   181.82|                775.00|                  800|             1,550.00|                    1,570.45|                   1,618.18|                   3,140.91|
|                 16.5|                    -36.36|                    90.91|                  -109.09|               -100.00|                 -100|              -300.00|                     -100.00|                    -100.00|                    -300.00|
|                 18.5|                   -100.00|                  -100.00|                  -100.00|               -100.00|                 -100|              -100.00|                     -100.00|                    -100.00|                    -100.00|
|                 19.5|                     90.91|                    90.91|                    90.91|               -100.00|                 -100|              -100.00|                     -100.00|                    -100.00|                    -100.00|

#### Table: NFL - Visitor Underdogs - Summary by Spread Amount - Closing Line

| Visitor - Spread - Open| Visitor - Spread - PL - Mean| Visitor - Spread - PL - Max| Visitor - Spread - PL - Sum| Visitor - ML - PL - Mean| Visitor - ML - PL - Max| Visitor - ML - PL - Sum| Visitor - 2-Parlay - PL - Mean| Visitor - 2-Parlay - PL - Max| Visitor - 2-Parlay - PL - Sum|
|-----------------------:|----------------------------:|---------------------------:|---------------------------:|------------------------:|-----------------------:|-----------------------:|------------------------------:|-----------------------------:|-----------------------------:|
|                     1.0|                        -2.11|                      100.00|                     -272.73|                    -1.83|                     125|                 -235.78|                          87.42|                        329.55|                     11,277.14|
|                     1.5|                         5.65|                       90.91|                      581.82|                     7.20|                     130|                  742.00|                         104.66|                        339.09|                     10,780.18|
|                     2.0|                         4.99|                      100.00|                      563.64|                     6.46|                     220|                  730.00|                         103.24|                        510.91|                     11,666.36|
|                     2.5|                         5.89|                       90.91|                    1,454.55|                     9.32|                     175|                2,303.00|                         108.71|                        425.00|                     26,851.18|
|                     3.0|                        14.69|                      100.00|                    9,109.09|                     7.47|                     170|                4,634.00|                         105.18|                        415.45|                     65,210.36|
|                     3.5|                        -5.69|                       90.91|                   -1,427.27|                     0.23|                     180|                   58.00|                          91.35|                        434.55|                     22,928.91|
|                     4.0|                        -7.18|                      100.00|                   -1,227.27|                   -10.97|                     200|               -1,876.22|                          69.96|                        472.73|                     11,963.58|
|                     4.5|                         3.34|                       90.91|                      363.64|                     0.24|                     200|                   26.00|                          91.36|                        472.73|                      9,958.73|
|                     5.0|                         6.86|                      100.00|                      727.27|                   -11.08|                     215|               -1,174.00|                          69.77|                        501.36|                      7,395.09|
|                     5.5|                        -9.37|                       90.91|                     -927.27|                   -13.89|                     220|               -1,375.00|                          64.39|                        510.91|                      6,375.00|
|                     6.0|                        15.33|                      100.00|                    2,390.91|                    18.04|                     250|                2,815.00|                         125.36|                        568.18|                     19,555.91|
|                     6.5|                        -3.10|                       90.91|                     -409.09|                   -10.35|                     255|               -1,366.00|                          71.15|                        577.73|                      9,392.18|
|                     7.0|                         2.11|                      100.00|                      436.36|                   -16.26|                     300|               -3,365.00|                          59.87|                        663.64|                     12,394.09|
|                     7.5|                        -7.08|                       90.91|                     -800.00|                   -18.81|                     330|               -2,125.00|                          55.01|                        720.91|                      6,215.91|
|                     8.0|                         5.84|                      100.00|                      327.27|                    25.57|                     375|                1,432.00|                         139.73|                        806.82|                      7,824.73|
|                     8.5|                        -7.44|                       90.91|                     -245.45|                     3.94|                     350|                  130.00|                          98.43|                        759.09|                      3,248.18|
|                     9.0|                         0.60|                      100.00|                       45.45|                     3.18|                     380|                  242.00|                          96.99|                        816.36|                      7,371.09|
|                     9.5|                        -6.11|                       90.91|                     -372.73|                   -30.49|                     400|               -1,860.00|                          32.70|                        854.55|                      1,994.55|
|                    10.0|                        10.24|                      100.00|                      890.91|                    -3.33|                     450|                 -290.00|                          84.55|                        950.00|                      7,355.45|
|                    10.5|                         4.55|                       90.91|                      190.91|                    50.95|                     480|                2,140.00|                         188.18|                      1,007.27|                      7,903.64|
|                    11.0|                       -27.01|                       90.91|                     -918.18|                   -52.21|                     475|               -1,775.00|                          -8.76|                        997.73|                       -297.73|
|                    11.5|                         6.91|                       90.91|                      172.73|                   -77.20|                     470|               -1,930.00|                         -56.47|                        988.18|                     -1,411.82|
|                    12.0|                       -32.62|                       90.91|                     -554.55|                   -70.59|                     400|               -1,200.00|                         -43.85|                        854.55|                       -745.45|
|                    12.5|                        24.09|                       90.91|                      481.82|                    53.25|                     700|                1,065.00|                         192.57|                      1,427.27|                      3,851.36|
|                    13.0|                       -17.92|                      100.00|                     -627.27|                   -61.86|                     575|               -2,165.00|                         -27.18|                      1,188.64|                       -951.36|
|                    13.5|                        23.53|                       90.91|                      800.00|                    62.79|                     635|                2,135.00|                         210.79|                      1,303.18|                      7,166.82|
|                    14.0|                       -22.36|                      100.00|                     -827.27|                   -79.35|                     664|               -2,936.00|                         -60.58|                      1,358.55|                     -2,241.45|
|                    14.5|                        17.48|                       90.91|                      227.27|                   -39.62|                     685|                 -515.00|                          15.28|                      1,398.64|                        198.64|
|                    15.0|                        14.55|                       90.91|                       72.73|                    80.00|                     800|                  400.00|                         243.64|                      1,618.18|                      1,218.18|
|                    15.5|                         9.09|                       90.91|                       63.64|                  -100.00|                    -100|                 -700.00|                        -100.00|                       -100.00|                       -700.00|
|                    16.0|                        17.48|                       90.91|                      227.27|                  -100.00|                    -100|               -1,300.00|                        -100.00|                       -100.00|                     -1,300.00|
|                    16.5|                        -4.55|                       90.91|                      -36.36|                  -100.00|                    -100|                 -800.00|                        -100.00|                       -100.00|                       -800.00|
|                    17.0|                        -3.79|                      100.00|                      -45.45|                   179.00|                   1,200|                2,148.00|                         432.64|                      2,381.82|                      5,191.64|
|                    18.5|                      -100.00|                     -100.00|                     -100.00|                  -100.00|                    -100|                 -100.00|                        -100.00|                       -100.00|                       -100.00|
|                    19.5|                      -100.00|                     -100.00|                     -100.00|                  -100.00|                    -100|                 -100.00|                        -100.00|                       -100.00|                       -100.00|
|                    20.0|                        -4.55|                       90.91|                       -9.09|                  -100.00|                    -100|                 -200.00|                        -100.00|                       -100.00|                       -200.00|
|                    20.5|                        -4.55|                       90.91|                       -9.09|                  -100.00|                    -100|                 -200.00|                        -100.00|                       -100.00|                       -200.00|
|                    21.0|                        90.91|                       90.91|                       90.91|                  -100.00|                    -100|                 -100.00|                        -100.00|                       -100.00|                       -100.00|
|                    22.5|                        -4.55|                       90.91|                       -9.09|                  -100.00|                    -100|                 -200.00|                        -100.00|                       -100.00|                       -200.00|
|                    23.5|                        90.91|                       90.91|                       90.91|                  -100.00|                    -100|                 -100.00|                        -100.00|                       -100.00|                       -100.00|
|                    26.5|                        90.91|                       90.91|                       90.91|                  -100.00|                    -100|                 -100.00|                        -100.00|                       -100.00|                       -100.00|


### All NBA Charts

#### Table: NBA 2007 to Present - Summary of Home Team Data - Profit and Loss

|                            |         |
|:---------------------------|----------:|
|Home - Spread - Avg P/L     |      -3.97|
|Home - Spread - Max P/L     |     100.00|
|Home - Spread - Min P/L     |    -100.00|
|Home - Spread - Total P/L   | -71,645.45|
|Home - ML - Avg P/L         |      -3.72|
|Home - ML - Max P/L         |   1,900.00|
|Home - ML - Min P/L         |    -100.00|
|Home - ML - Total P/L       | -67,070.40|
|Home - 2-Parlay - Avg P/L   |      49.31|
|Home - 2-Parlay - Max P/L   |   3,718.18|
|Home - 2-Parlay - Min P/L   |    -100.00|
|Home - 2-Parlay - Total P/L | 889,746.94|

#### Table: NBA 2007 to Present - Summary of Visitor Team Data - Profit and Loss

|                               |           V1|
|:------------------------------|------------:|
|Visitor - Spread - Avg P/L     |        -1.39|
|Visitor - Spread - Max P/L     |       100.00|
|Visitor - Spread - Min P/L     |      -100.00|
|Visitor - Spread - Total P/L   |   -25,063.64|
|Visitor - ML - Avg P/L         |        -3.92|
|Visitor - ML - Max P/L         |     2,000.00|
|Visitor - ML - Min P/L         |      -100.00|
|Visitor - ML - Total P/L       |   -70,775.07|
|Visitor - 2-Parlay - Avg P/L   |        71.77|
|Visitor - 2-Parlay - Max P/L   |     3,909.09|
|Visitor - 2-Parlay - Min P/L   |      -100.00|
|Visitor - 2-Parlay - Total P/L | 1,294,960.01|

#### Table: NBA 2007 to Present - Summary of Home Team Data - Wins and Losses

|                                             |        V1|
|:--------------------------------------------|---------:|
|Home - Spread - Win Total                    |  8,739.00|
|Home - Spread - Loss Total                   |  8,983.00|
|Home - Spread - Total Games                  | 17,722.00|
|Home - Spread - Average Winning Percentage   |     49.31|
|Home - ML - Win Total                        | 10,623.00|
|Home - ML - Loss Total                       |  7,421.00|
|Home - ML - Total Games                      | 18,044.00|
|Home - ML - Average Winning Percentage       |     58.87|
|Home - 2-Parlay - Win Total                  |  8,055.00|
|Home - 2-Parlay - Loss Total                 |  9,989.00|
|Home - 2-Parlay - Total Games                | 18,044.00|
|Home - 2-Parlay - Average Winning Percentage |     44.64|

#### Table: NBA 2007 to Present - Summary of Visitor Team Data - Wins and Losses

|                                                |        V1|
|:-----------------------------------------------|---------:|
|Visitor - Spread - Win Total                    |  8,983.00|
|Visitor - Spread - Loss Total                   |  8,739.00|
|Visitor - Spread - Total Games                  | 17,722.00|
|Visitor - Spread - Average Winning Percentage   |     50.69|
|Visitor - ML - Win Total                        |  7,421.00|
|Visitor - ML - Loss Total                       | 10,623.00|
|Visitor - ML - Total Games                      | 18,044.00|
|Visitor - ML - Average Winning Percentage       |     41.13|
|Visitor - 2-Parlay - Win Total                  |  6,632.00|
|Visitor - 2-Parlay - Loss Total                 | 11,412.00|
|Visitor - 2-Parlay - Total Games                | 18,044.00|
|Visitor - 2-Parlay - Average Winning Percentage |     36.75|

#### Table: NBA 2007 to Present - Home Favorites - Summary by Spread Amount

| Home - Spread - Open| Home - Spread - PL - Mean| Home - Spread - PL - Max| Home - Spread - PL - Sum| Home - ML - PL - Mean| Home - ML - PL - Max| Home - ML - PL - Sum| Home - 2-Parlay - PL - Mean| Home - 2-Parlay - PL - Max| Home - 2-Parlay - PL - Sum|
|--------------------:|-------------------------:|------------------------:|------------------------:|---------------------:|--------------------:|--------------------:|---------------------------:|--------------------------:|--------------------------:|
|                -0.50|                     -4.55|                    90.91|                    -9.09|                 -2.38|                95.24|                -4.76|                       86.36|                     272.73|                     172.73|
|                -1.00|                     -3.62|                   100.00|                -1,909.09|                 -5.24|               175.00|            -2,768.81|                       72.44|                     396.36|                  38,250.90|
|                -1.50|                     -5.99|                    90.91|                -2,763.64|                 -5.81|               150.00|            -2,676.85|                       72.11|                     377.27|                  33,244.97|
|                -2.00|                     -0.40|                   100.00|                  -218.18|                 -6.46|               150.00|            -3,568.45|                       59.05|                     325.73|                  32,593.21|
|                -2.50|                      2.06|                    90.91|                 1,072.73|                 -0.42|               135.00|              -217.35|                       73.17|                     348.64|                  38,047.28|
|                -2.51|                   -100.00|                  -100.00|                  -100.00|               -100.00|              -100.00|              -100.00|                     -100.00|                    -100.00|                    -100.00|
|                -3.00|                      1.59|                   100.00|                   945.45|                 -5.06|               160.00|            -3,005.80|                       53.06|                     396.36|                  31,520.24|
|                -3.50|                      1.25|                    90.91|                   763.64|                 -1.31|               125.00|              -800.70|                       60.86|                     329.55|                  37,061.90|
|                -4.00|                     -0.57|                   100.00|                  -336.36|                 -4.66|               115.00|            -2,753.32|                       44.41|                     310.45|                  26,248.37|
|                -4.50|                     -3.19|                    90.91|                -2,027.27|                 -4.41|               110.00|            -2,801.93|                       45.93|                     300.91|                  29,163.17|
|                -5.00|                      1.53|                   100.00|                   909.09|                 -6.71|               240.00|            -3,981.12|                       33.49|                     549.09|                  19,858.52|
|                -5.50|                     -1.96|                    90.91|                -1,154.55|                 -3.26|               200.00|            -1,923.09|                       39.56|                     472.73|                  23,338.63|
|                -6.00|                     -1.12|                   100.00|                  -663.64|                 -4.26|                74.07|            -2,529.24|                       30.42|                     232.32|                  18,070.18|
|                -6.50|                     -9.53|                    90.91|                -5,109.09|                 -6.45|               105.00|            -3,457.96|                       22.08|                     291.36|                  11,834.22|
|                -7.00|                     -2.97|                   100.00|                -1,654.55|                 -3.86|               140.00|            -2,152.41|                       17.59|                     358.18|                   9,800.10|
|                -7.50|                     -4.15|                    90.91|                -2,027.27|                 -5.17|                64.52|            -2,522.89|                       23.49|                     181.82|                  11,460.93|
|                -8.00|                     -1.24|                   100.00|                  -600.00|                 -3.66|               110.00|            -1,767.15|                       15.32|                     214.08|                   7,398.03|
|                -8.50|                     -6.30|                    90.91|                -2,736.36|                 -3.39|                54.05|            -1,473.16|                       15.98|                     172.15|                   6,934.46|
|                -9.00|                      1.02|                   100.00|                   454.55|                 -0.65|                41.67|              -292.66|                       16.24|                     170.45|                   7,260.62|
|                -9.50|                     -5.83|                    90.91|                -2,163.64|                 -4.13|                52.63|            -1,533.69|                       11.85|                     191.39|                   4,396.71|
|               -10.00|                     -1.91|                   100.00|                  -772.73|                 -4.28|                37.04|            -1,727.62|                        7.29|                     161.62|                   2,946.29|
|               -10.50|                     -5.84|                    90.91|                -1,718.18|                 -1.23|                38.46|              -362.77|                        8.36|                     164.34|                   2,458.09|
|               -11.00|                     -1.58|                   100.00|                  -454.55|                 -1.31|                37.04|              -376.54|                        3.43|                     148.76|                     987.46|
|               -11.50|                    -17.85|                    90.91|                -4,354.55|                 -0.90|                45.45|              -218.62|                       -8.70|                     138.64|                  -2,122.80|
|               -12.00|                     -9.13|                   100.00|                -2,290.91|                 -4.68|                23.81|            -1,174.38|                       -8.51|                     129.09|                  -2,135.39|
|               -12.50|                     -1.87|                    90.91|                  -400.00|                 -1.66|                23.81|              -356.06|                        6.82|                     122.73|                   1,459.07|
|               -13.00|                     -4.03|                   100.00|                  -636.36|                 -7.27|                20.00|            -1,149.15|                       -9.00|                     129.09|                  -1,422.54|
|               -13.50|                    -12.87|                    90.91|                -1,918.18|                 -4.93|                20.00|              -734.24|                       -7.15|                     129.09|                  -1,064.75|
|               -14.00|                    -19.04|                   100.00|                -2,209.09|                 -5.66|                22.22|              -656.19|                      -21.05|                     133.33|                  -2,441.27|
|               -14.50|                     18.41|                    90.91|                 1,454.55|                  1.23|                16.67|                97.31|                       24.26|                     108.26|                   1,916.84|
|               -15.00|                      8.43|                   100.00|                   818.18|                  0.12|                12.50|                11.86|                        8.87|                     112.12|                     860.65|
|               -15.50|                    -17.65|                    90.91|                  -900.00|                 -0.30|                 8.33|               -15.43|                      -14.65|                     103.64|                    -747.12|
|               -16.00|                    -14.10|                   100.00|                  -690.91|                 -2.81|                12.50|              -137.89|                      -15.23|                     114.77|                    -746.26|
|               -16.50|                    -18.18|                    90.91|                  -509.09|                 -1.05|                 5.00|               -29.49|                      -16.15|                     100.45|                    -452.10|
|               -17.00|                    -14.09|                    90.91|                  -281.82|                 -7.53|                 5.00|              -150.61|                      -11.86|                      99.21|                    -237.28|
|               -17.50|                      2.80|                    90.91|                    36.36|                  2.86|                 6.25|                37.18|                        5.89|                     102.84|                      76.63|
|               -18.00|                    -60.91|                   100.00|                  -609.09|                  1.68|                 3.33|                16.79|                      -80.70|                      93.01|                    -806.99|
|               -18.50|                    -23.64|                    90.91|                  -118.18|                  2.45|                 4.55|                12.27|                      -21.42|                      99.59|                    -107.12|
|               -19.00|                    -36.36|                    90.91|                  -218.18|                -14.72|                 3.91|               -88.30|                      -34.39|                      98.38|                    -206.37|
|               -19.50|                     27.27|                    90.91|                    81.82|                  2.80|                 4.55|                 8.39|                       31.90|                      99.59|                      95.70|
|               -20.00|                    -36.36|                    90.91|                  -109.09|                  1.26|                 2.00|                 3.77|                      -35.87|                      92.38|                    -107.62|
|               -20.50|                     90.91|                    90.91|                    90.91|                  2.01|                 2.01|                 2.01|                       94.76|                      94.76|                      94.76|
|               -21.00|                   -100.00|                  -100.00|                  -100.00|                  1.18|                 1.18|                 1.18|                     -100.00|                    -100.00|                    -100.00|
|               -21.50|                     -4.55|                    90.91|                    -9.09|                  1.37|                 1.91|                 2.74|                       -2.73|                      94.55|                      -5.45|
|               -22.00|                     -4.55|                    90.91|                    -9.09|                  1.14|                 2.27|                 2.27|                       -4.55|                      90.91|                      -9.09|

#### Table: NBA 2007 to Present - Visitor Favorites - Summary by Spread Amount

| Visitor - Spread - Open| Visitor - Spread - PL - Mean| Visitor - Spread - PL - Max| Visitor - Spread - PL - Sum| Visitor - ML - PL - Mean| Visitor - ML - PL - Max| Visitor - ML - PL - Sum| Visitor - 2-Parlay - PL - Mean| Visitor - 2-Parlay - PL - Max| Visitor - 2-Parlay - PL - Sum|
|-----------------------:|----------------------------:|---------------------------:|---------------------------:|------------------------:|-----------------------:|-----------------------:|------------------------------:|-----------------------------:|-----------------------------:|
|                    -0.5|                        90.91|                       90.91|                      181.82|                    40.34|                   62.50|                   80.68|                         167.92|                        210.23|                        335.85|
|                    -1.0|                         4.00|                      100.00|                    1,900.00|                     0.90|                  300.00|                  429.29|                          80.60|                        663.64|                     38,283.96|
|                    -1.5|                        -2.95|                       90.91|                   -1,063.64|                    -1.19|                  180.00|                 -429.80|                          74.55|                        405.91|                     26,837.59|
|                    -2.0|                         8.69|                      100.00|                    3,318.18|                     2.91|                  135.00|                1,112.99|                          77.14|                        348.64|                     29,465.65|
|                    -2.5|                        -8.67|                       90.91|                   -3,009.09|                   -11.93|                  190.00|               -4,140.94|                          54.71|                        453.64|                     18,982.87|
|                    -3.0|                        -2.30|                      100.00|                     -972.73|                    -8.98|                  175.00|               -3,796.81|                          52.85|                        425.00|                     22,353.83|
|                    -3.5|                        -5.02|                       90.91|                   -2,036.36|                    -7.71|                  125.00|               -3,129.87|                          50.01|                        329.55|                     20,302.35|
|                    -4.0|                        -4.32|                      100.00|                   -1,545.45|                   -11.57|                  140.00|               -4,140.85|                          37.60|                        358.18|                     13,461.04|
|                    -4.5|                         7.69|                       90.91|                    2,400.00|                     4.53|                  115.00|                1,413.38|                          59.98|                        281.82|                     18,715.02|
|                    -5.0|                         2.29|                      100.00|                      809.09|                    -3.75|                  120.00|               -1,328.71|                          37.21|                        320.00|                     13,173.96|
|                    -5.5|                        -3.07|                       90.91|                     -990.91|                    -5.94|                  155.00|               -1,917.29|                          37.61|                        232.32|                     12,146.73|
|                    -6.0|                        16.36|                      100.00|                    4,581.82|                     1.38|                   83.33|                  387.68|                          46.67|                        250.00|                     13,066.74|
|                    -6.5|                        -2.33|                       90.91|                     -600.00|                    -0.87|                   71.43|                 -225.31|                          30.36|                        227.27|                      7,832.39|
|                    -7.0|                        11.54|                      100.00|                    2,400.00|                     1.54|                   58.82|                  319.99|                          34.33|                        203.21|                      7,140.13|
|                    -7.5|                        -3.52|                       90.91|                     -654.55|                    -4.52|                   52.63|                 -840.67|                          22.72|                        191.39|                      4,225.07|
|                    -8.0|                         5.64|                      100.00|                      963.64|                    -5.80|                   50.00|                 -992.01|                          12.55|                        186.36|                      2,145.28|
|                    -8.5|                       -11.41|                       90.91|                   -1,745.45|                    -2.12|                   76.92|                 -324.71|                           9.77|                        237.76|                      1,495.54|
|                    -9.0|                        10.96|                      100.00|                    1,436.36|                     0.23|                   76.92|                   30.65|                          28.13|                        170.45|                      3,684.43|
|                    -9.5|                        13.77|                       90.91|                    1,363.64|                     0.63|                  350.00|                   62.19|                          36.31|                        181.82|                      3,595.18|
|                   -10.0|                        -9.46|                      100.00|                     -700.00|                    -5.45|                   37.04|                 -402.99|                          -0.65|                        161.62|                        -48.42|
|                   -10.5|                        18.87|                       90.91|                    1,000.00|                    -5.65|                   22.22|                 -299.68|                          34.68|                        129.09|                      1,838.18|
|                   -11.0|                        -8.00|                      100.00|                     -400.00|                    -9.93|                   76.92|                 -496.55|                          -4.21|                        133.33|                       -210.31|
|                   -11.5|                       -19.62|                       90.91|                     -745.45|                    -7.75|                   32.26|                 -294.39|                         -10.37|                        136.36|                       -394.24|
|                   -12.0|                        10.49|                      100.00|                      272.73|                    -2.48|                   28.57|                  -64.57|                          14.17|                        145.45|                        368.39|
|                   -12.5|                        24.09|                       90.91|                      481.82|                    -2.78|                   14.29|                  -55.54|                          33.76|                        118.18|                        675.14|
|                   -13.0|                       -21.39|                       90.91|                     -363.64|                    -5.20|                   12.50|                  -88.37|                         -16.30|                        108.26|                       -277.04|
|                   -13.5|                       -25.76|                       90.91|                     -463.64|                    -9.88|                   16.67|                 -177.84|                         -18.77|                        122.73|                       -337.93|
|                   -14.0|                         4.13|                       90.91|                       45.45|                   -13.03|                   14.29|                 -143.36|                          10.54|                        108.26|                        115.94|
|                   -14.5|                        52.73|                       90.91|                      263.64|                     5.80|                   11.76|                   28.98|                          59.30|                        103.64|                        296.51|
|                   -15.0|                        90.91|                       90.91|                      181.82|                     3.93|                    5.00|                    7.86|                          98.41|                        100.45|                        196.82|
|                   -15.5|                      -100.00|                     -100.00|                     -400.00|                     2.40|                    3.33|                    9.62|                        -100.00|                       -100.00|                       -400.00|
|                   -16.0|                       -52.27|                       90.91|                     -209.09|                   -22.28|                    4.02|                  -89.12|                         -50.91|                         96.36|                       -203.64|
|                   -16.5|                      -100.00|                     -100.00|                     -100.00|                     3.23|                    3.23|                    3.23|                        -100.00|                       -100.00|                       -100.00|
|                   -17.0|                      -100.00|                     -100.00|                     -100.00|                     1.67|                    1.67|                    1.67|                        -100.00|                       -100.00|                       -100.00|
|                   -17.5|                      -100.00|                     -100.00|                     -100.00|                     1.01|                    1.01|                    1.01|                        -100.00|                       -100.00|                       -100.00|
|                  -212.5|                      -100.00|                     -100.00|                     -100.00|                   320.00|                  320.00|                  320.00|                        -100.00|                       -100.00|                       -100.00|
|                  -216.0|                      -100.00|                     -100.00|                     -100.00|                   130.00|                  130.00|                  130.00|                        -100.00|                       -100.00|                       -100.00|

#### Table: NBA 2007 to Present - Home Underdogs - Summary by Spread Amount

| Home - Spread - Open| Home - Spread - PL - Mean| Home - Spread - PL - Max| Home - Spread - PL - Sum| Home - ML - PL - Mean| Home - ML - PL - Max| Home - ML - PL - Sum| Home - 2-Parlay - PL - Mean| Home - 2-Parlay - PL - Max| Home - 2-Parlay - PL - Sum|
|--------------------:|-------------------------:|------------------------:|------------------------:|---------------------:|--------------------:|--------------------:|---------------------------:|--------------------------:|--------------------------:|
|                  0.5|                   -100.00|                  -100.00|                  -200.00|               -100.00|                 -100|              -200.00|                     -100.00|                    -100.00|                    -200.00|
|                  1.0|                     -6.05|                   100.00|                -2,872.73|                 -6.48|                  175|            -3,079.71|                       78.53|                     425.00|                  37,302.37|
|                  1.5|                     -6.14|                    90.91|                -2,209.09|                 -6.81|                  195|            -2,452.95|                       77.90|                     463.18|                  28,044.38|
|                  2.0|                    -12.30|                   100.00|                -4,700.00|                -14.63|                  240|            -5,587.21|                       62.99|                     549.09|                  24,060.77|
|                  2.5|                     -0.42|                    90.91|                  -145.45|                  9.66|                  260|             3,353.28|                      109.36|                     587.27|                  37,947.18|
|                  3.0|                     -1.85|                   100.00|                  -781.82|                  5.05|                  300|             2,137.45|                      100.56|                     663.64|                  42,535.13|
|                  3.5|                     -4.08|                    90.91|                -1,654.55|                  3.94|                  450|             1,600.96|                       98.44|                     950.00|                  39,965.47|
|                  4.0|                      0.48|                   100.00|                   172.73|                  9.86|                  290|             3,529.82|                      109.73|                     644.55|                  39,284.20|
|                  4.5|                    -16.78|                    90.91|                -5,236.36|                -21.54|                  230|            -6,719.11|                       49.80|                     530.00|                  15,536.25|
|                  5.0|                     -3.11|                   100.00|                -1,100.00|                 -2.66|                  350|              -942.90|                       85.82|                     759.09|                  30,381.73|
|                  5.5|                     -6.02|                    90.91|                -1,945.45|                  2.25|                  320|               725.24|                       95.20|                     701.82|                  30,748.18|
|                  6.0|                    -15.00|                   100.00|                -4,200.00|                -13.11|                  425|            -3,671.00|                       65.88|                     902.27|                  18,446.27|
|                  6.5|                     -6.77|                    90.91|                -1,745.45|                -12.63|                  350|            -3,257.93|                       66.80|                     759.09|                  17,234.87|
|                  7.0|                     -9.57|                   100.00|                -1,990.91|                -13.10|                  430|            -2,725.00|                       65.90|                     911.82|                  13,706.82|
|                  7.5|                     -5.57|                    90.91|                -1,036.36|                 -7.57|                  450|            -1,408.74|                       76.45|                     950.00|                  14,219.67|
|                  8.0|                      1.17|                   100.00|                   200.00|                  2.06|                  550|               352.69|                       94.85|                   1,140.91|                  16,218.78|
|                  8.5|                      2.32|                    90.91|                   354.55|                -10.58|                  475|            -1,618.00|                       70.72|                     997.73|                  10,820.18|
|                  9.0|                    -15.27|                   100.00|                -2,000.00|                -17.29|                  525|            -2,265.00|                       57.90|                   1,093.18|                   7,585.00|
|                  9.5|                    -22.87|                    90.91|                -2,263.64|                 -3.50|                  650|              -346.67|                       84.22|                   1,331.82|                   8,338.18|
|                 10.0|                      6.02|                   100.00|                   445.45|                 -5.47|                  650|              -405.00|                       80.46|                   1,331.82|                   5,954.09|
|                 10.5|                    -27.96|                    90.91|                -1,481.82|                  6.60|                  800|               350.00|                      103.52|                   1,618.18|                   5,486.36|
|                 11.0|                      7.27|                   100.00|                   363.64|                 35.34|                  750|             1,767.00|                      158.38|                   1,522.73|                   7,918.82|
|                 11.5|                     10.53|                    90.91|                   400.00|                 28.42|                  730|             1,080.00|                      145.17|                   1,484.55|                   5,516.36|
|                 12.0|                    -11.54|                   100.00|                  -300.00|                -31.73|                  700|              -825.00|                       30.33|                   1,427.27|                     788.64|
|                 12.5|                    -33.18|                    90.91|                  -663.64|                  0.00|                1,000|                 0.00|                       90.91|                   2,000.00|                   1,818.18|
|                 13.0|                     12.30|                    90.91|                   209.09|                 26.47|                1,300|               450.00|                      141.44|                   2,572.73|                   2,404.55|
|                 13.5|                     16.67|                    90.91|                   300.00|                 83.06|                1,195|             1,495.00|                      249.47|                   2,372.27|                   4,490.45|
|                 14.0|                    -13.22|                    90.91|                  -145.45|                 52.27|                1,000|               575.00|                      190.70|                   2,000.00|                   2,097.73|
|                 14.5|                    -61.82|                    90.91|                  -309.09|               -100.00|                 -100|              -500.00|                     -100.00|                    -100.00|                    -500.00|
|                 15.0|                   -100.00|                  -100.00|                  -200.00|               -100.00|                 -100|              -200.00|                     -100.00|                    -100.00|                    -200.00|
|                 15.5|                     90.91|                    90.91|                   363.64|               -100.00|                 -100|              -400.00|                     -100.00|                    -100.00|                    -400.00|
|                 16.0|                     43.18|                    90.91|                   172.73|                400.00|                1,900|             1,600.00|                      854.55|                   3,718.18|                   3,418.18|
|                 16.5|                     90.91|                    90.91|                    90.91|               -100.00|                 -100|              -100.00|                     -100.00|                    -100.00|                    -100.00|
|                 17.0|                     90.91|                    90.91|                    90.91|               -100.00|                 -100|              -100.00|                     -100.00|                    -100.00|                    -100.00|
|                 17.5|                     90.91|                    90.91|                    90.91|               -100.00|                 -100|              -100.00|                     -100.00|                    -100.00|                    -100.00|
|                212.5|                     90.91|                    90.91|                    90.91|               -100.00|                 -100|              -100.00|                     -100.00|                    -100.00|                    -100.00|
|                216.0|                     90.91|                    90.91|                    90.91|               -100.00|                 -100|              -100.00|                     -100.00|                    -100.00|                    -100.00|

#### Table: NBA 2007 to Present - Visitor Underdogs - Summary by Spread Amount

| Visitor - Spread - Open| Visitor - Spread - PL - Mean| Visitor - Spread - PL - Max| Visitor - Spread - PL - Sum| Visitor - ML - PL - Mean| Visitor - ML - PL - Max| Visitor - ML - PL - Sum| Visitor - 2-Parlay - PL - Mean| Visitor - 2-Parlay - PL - Max| Visitor - 2-Parlay - PL - Sum|
|-----------------------:|----------------------------:|---------------------------:|---------------------------:|------------------------:|-----------------------:|-----------------------:|------------------------------:|-----------------------------:|-----------------------------:|
|                    0.50|                        -4.55|                       90.91|                       -9.09|                     5.00|                     110|                   10.00|                         100.45|                        300.91|                        200.91|
|                    1.00|                        -0.72|                      100.00|                     -381.82|                    -1.79|                     270|                 -945.90|                          87.49|                        606.36|                     46,194.20|
|                    1.50|                        -3.10|                       90.91|                   -1,427.27|                     0.85|                     400|                  390.97|                          92.53|                        854.55|                     42,655.48|
|                    2.00|                        -0.74|                      100.00|                     -409.09|                    -1.86|                     270|               -1,026.36|                          87.36|                        606.36|                     48,222.40|
|                    2.50|                       -11.15|                       90.91|                   -5,800.00|                    -7.75|                     310|               -4,031.05|                          76.11|                        682.73|                     39,577.08|
|                    2.51|                        90.91|                       90.91|                       90.91|                   105.00|                     105|                  105.00|                         291.36|                        291.36|                        291.36|
|                    3.00|                        -2.59|                      100.00|                   -1,536.36|                    -1.32|                     350|                 -786.43|                          88.38|                        759.09|                     52,498.63|
|                    3.50|                       -10.34|                       90.91|                   -6,300.00|                    -7.90|                     250|               -4,808.55|                          75.84|                        568.18|                     46,183.67|
|                    4.00|                        -2.51|                      100.00|                   -1,481.82|                    -1.87|                     250|               -1,106.29|                          87.34|                        568.18|                     51,615.26|
|                    4.50|                        -5.90|                       90.91|                   -3,745.45|                    -2.30|                     309|               -1,458.11|                          86.53|                        680.82|                     54,943.61|
|                    5.00|                        -0.40|                      100.00|                     -236.36|                     1.98|                     450|                1,176.92|                          94.70|                        950.00|                     56,155.94|
|                    5.50|                        -7.13|                       90.91|                   -4,209.09|                    -1.83|                   1,453|               -1,078.00|                          87.42|                      2,864.82|                     51,578.36|
|                    6.00|                        -3.05|                      100.00|                   -1,809.09|                    -1.35|                     525|                 -799.08|                          88.34|                      1,093.18|                     52,474.49|
|                    6.50|                         0.44|                       90.91|                      236.36|                     2.15|                     428|                1,149.96|                          95.00|                        908.00|                     50,922.64|
|                    7.00|                         2.51|                      100.00|                    1,400.00|                    -3.28|                     495|               -1,827.00|                          84.65|                      1,035.91|                     47,148.45|
|                    7.50|                        -4.94|                       90.91|                   -2,409.09|                     1.76|                     465|                  857.00|                          94.26|                        978.64|                     45,999.73|
|                    8.00|                        -0.06|                      100.00|                      -27.27|                    -3.55|                     500|               -1,716.50|                          84.12|                      1,045.45|                     40,632.14|
|                    8.50|                        -2.79|                       90.91|                   -1,209.09|                    -1.03|                     775|                 -449.00|                          88.93|                      1,570.45|                     38,597.36|
|                    9.00|                        -4.96|                      100.00|                   -2,218.18|                   -14.90|                     900|               -6,661.57|                          62.46|                      1,809.09|                     27,918.82|
|                    9.50|                        -3.26|                       90.91|                   -1,209.09|                    -0.07|                     700|                  -25.93|                          90.78|                      1,427.27|                     33,677.78|
|                   10.00|                        -0.97|                      100.00|                     -390.91|                    -0.15|                   1,400|                  -60.00|                          90.63|                      2,763.64|                     36,612.73|
|                   10.50|                        -3.25|                       90.91|                     -954.55|                   -15.68|                     900|               -4,610.00|                          60.97|                      1,809.09|                     17,926.36|
|                   11.00|                        -0.25|                      100.00|                      -72.73|                   -12.06|                   2,000|               -3,473.00|                          67.89|                      3,909.09|                     19,551.55|
|                   11.50|                         8.76|                       90.91|                    2,136.36|                   -22.64|                   1,200|               -5,525.00|                          47.68|                      2,381.82|                     11,634.09|
|                   12.00|                         8.37|                      100.00|                    2,100.00|                     0.63|                     850|                  158.00|                          92.11|                      1,713.64|                     23,119.82|
|                   12.50|                        -7.22|                       90.91|                   -1,545.45|                    -9.86|                   1,490|               -2,110.00|                          72.09|                      2,935.45|                     15,426.36|
|                   13.00|                         6.85|                      100.00|                    1,081.82|                    18.30|                   1,100|                2,892.00|                         125.85|                      2,190.91|                     19,884.73|
|                   13.50|                         3.78|                       90.91|                      563.64|                     4.38|                   1,500|                  652.00|                          99.26|                      2,954.55|                     14,790.18|
|                   14.00|                        17.16|                      100.00|                    1,990.91|                    19.93|                   1,400|                2,312.00|                         128.96|                      2,763.64|                     14,959.27|
|                   14.50|                       -27.50|                       90.91|                   -2,172.73|                   -62.78|                     940|               -4,960.00|                         -28.95|                      1,885.45|                     -2,287.27|
|                   15.00|                       -13.21|                      100.00|                   -1,281.82|                   -53.97|                   1,600|               -5,235.00|                         -12.12|                      3,145.45|                     -1,175.91|
|                   15.50|                         8.56|                       90.91|                      436.36|                   -41.08|                   1,605|               -2,095.00|                          12.49|                      3,155.00|                        636.82|
|                   16.00|                         9.28|                      100.00|                      454.55|                   -36.92|                   1,280|               -1,809.00|                          20.43|                      2,534.55|                      1,001.00|
|                   16.50|                         9.09|                       90.91|                      254.55|                   -75.00|                     600|               -2,100.00|                         -52.27|                      1,236.36|                     -1,463.64|
|                   17.00|                         5.00|                       90.91|                      100.00|                    55.00|                   1,500|                1,100.00|                         195.91|                      2,954.55|                      3,918.18|
|                   17.50|                       -11.89|                       90.91|                     -154.55|                  -100.00|                    -100|               -1,300.00|                        -100.00|                       -100.00|                     -1,300.00|
|                   18.00|                        72.73|                      100.00|                      727.27|                  -100.00|                    -100|               -1,000.00|                        -100.00|                       -100.00|                     -1,000.00|
|                   18.50|                        14.55|                       90.91|                       72.73|                  -100.00|                    -100|                 -500.00|                        -100.00|                       -100.00|                       -500.00|
|                   19.00|                        27.27|                       90.91|                      163.64|                   166.67|                   1,500|                1,000.00|                         409.09|                      2,954.55|                      2,454.55|
|                   19.50|                       -36.36|                       90.91|                     -109.09|                  -100.00|                    -100|                 -300.00|                        -100.00|                       -100.00|                       -300.00|
|                   20.00|                        27.27|                       90.91|                       81.82|                  -100.00|                    -100|                 -300.00|                        -100.00|                       -100.00|                       -300.00|
|                   20.50|                      -100.00|                     -100.00|                     -100.00|                  -100.00|                    -100|                 -100.00|                        -100.00|                       -100.00|                       -100.00|
|                   21.00|                        90.91|                       90.91|                       90.91|                  -100.00|                    -100|                 -100.00|                        -100.00|                       -100.00|                       -100.00|
|                   21.50|                        -4.55|                       90.91|                       -9.09|                  -100.00|                    -100|                 -200.00|                        -100.00|                       -100.00|                       -200.00|
|                   22.00|                        -4.55|                       90.91|                       -9.09|                  -100.00|                    -100|                 -200.00|                        -100.00|                       -100.00|                       -200.00|

### All NCAAF Charts - Opening Line

#### Table: NCAAF 2007 to Present - Summary of Home Team Data - Profit and Loss - Opening Line

|                            |           |
|:---------------------------|----------:|
|Home - Spread - Avg P/L     |       2.92|
|Home - Spread - Max P/L     |     100.00|
|Home - Spread - Min P/L     |    -100.00|
|Home - Spread - Total P/L   |  39,572.73|
|Home - ML - Avg P/L         |      -5.67|
|Home - ML - Max P/L         |   5,000.00|
|Home - ML - Min P/L         |    -100.00|
|Home - ML - Total P/L       | -76,792.35|
|Home - 2-Parlay - Avg P/L   |      45.70|
|Home - 2-Parlay - Max P/L   |   9,636.36|
|Home - 2-Parlay - Min P/L   |    -100.00|
|Home - 2-Parlay - Total P/L | 618,659.09|

#### Table: NCAAF 2007 to Present - Summary of Visitor Team Data - Profit and Loss - Opening Line

|                               |            |
|:------------------------------|-----------:|
|Visitor - Spread - Avg P/L     |       -8.88|
|Visitor - Spread - Max P/L     |      100.00|
|Visitor - Spread - Min P/L     |     -100.00|
|Visitor - Spread - Total P/L   | -120,218.18|
|Visitor - ML - Avg P/L         |       -9.31|
|Visitor - ML - Max P/L         |   14,950.00|
|Visitor - ML - Min P/L         |     -100.00|
|Visitor - ML - Total P/L       | -126,027.40|
|Visitor - 2-Parlay - Avg P/L   |       56.51|
|Visitor - 2-Parlay - Max P/L   |   28,631.82|
|Visitor - 2-Parlay - Min P/L   |     -100.00|
|Visitor - 2-Parlay - Total P/L |  765,047.52|

#### Table: NCAAF 2007 to Present - Summary of Home Team Data - Wins and Losses - Opening Line

|                                             |          |
|:--------------------------------------------|---------:|
|Home - Spread - Win Total                    |  7,087.00|
|Home - Spread - Loss Total                   |  6,249.00|
|Home - Spread - Total Games                  | 13,336.00|
|Home - Spread - Average Winning Percentage   |     53.14|
|Home - ML - Win Total                        |  8,273.00|
|Home - ML - Loss Total                       |  5,265.00|
|Home - ML - Total Games                      | 13,538.00|
|Home - ML - Average Winning Percentage       |     61.11|
|Home - 2-Parlay - Win Total                  |  6,240.00|
|Home - 2-Parlay - Loss Total                 |  7,298.00|
|Home - 2-Parlay - Total Games                | 13,538.00|
|Home - 2-Parlay - Average Winning Percentage |     46.09|

#### Table: NCAAF 2007 to Present - Summary of Visitor Team Data - Wins and Losses - Opening Line

|                                                |          |
|:-----------------------------------------------|---------:|
|Visitor - Spread - Win Total                    |  6,250.00|
|Visitor - Spread - Loss Total                   |  7,086.00|
|Visitor - Spread - Total Games                  | 13,336.00|
|Visitor - Spread - Average Winning Percentage   |     46.87|
|Visitor - ML - Win Total                        |  5,265.00|
|Visitor - ML - Loss Total                       |  8,273.00|
|Visitor - ML - Total Games                      | 13,538.00|
|Visitor - ML - Average Winning Percentage       |     38.89|
|Visitor - 2-Parlay - Win Total                  |  4,343.00|
|Visitor - 2-Parlay - Loss Total                 |  9,195.00|
|Visitor - 2-Parlay - Total Games                | 13,538.00|
|Visitor - 2-Parlay - Average Winning Percentage |     32.08|

#### Table: NCAAF 2007 to Present - Home Favorites - Summary by Spread Amount - Opening Line

| Home - Spread - Open| Home - Spread - PL - Mean| Home - Spread - PL - Max| Home - Spread - PL - Sum| Home - ML - PL - Mean| Home - ML - PL - Max| Home - ML - PL - Sum| Home - 2-Parlay - PL - Mean| Home - 2-Parlay - PL - Max| Home - 2-Parlay - PL - Sum|
|--------------------:|-------------------------:|------------------------:|------------------------:|---------------------:|--------------------:|--------------------:|---------------------------:|--------------------------:|--------------------------:|
|                 -0.5|                    -36.36|                    90.91|                  -109.09|                -13.33|               160.00|               -40.00|                       65.45|                     396.36|                     196.36|
|                 -1.0|                     -4.38|                   100.00|                  -709.09|                 -7.57|               165.00|            -1,226.56|                       69.47|                     405.91|                  11,253.82|
|                 -1.5|                    -12.24|                    90.91|                -1,518.18|                -13.36|               135.00|            -1,656.94|                       57.03|                     348.64|                   7,071.72|
|                 -2.0|                     -8.54|                   100.00|                -1,127.27|                -12.50|               130.00|            -1,650.36|                       50.78|                     339.09|                   6,702.35|
|                 -2.5|                     -7.58|                    90.91|                -1,909.09|                 -8.14|               130.00|            -2,050.07|                       57.60|                     339.09|                  14,515.67|
|                 -3.0|                      3.38|                   100.00|                 1,181.82|                 -1.95|               290.00|              -683.98|                       40.94|                     358.18|                  14,328.23|
|                 -3.5|                      0.23|                    90.91|                    54.55|                 -1.85|               110.00|              -444.48|                       58.42|                     300.91|                  14,021.38|
|                 -4.0|                    -14.94|                   100.00|                -3,063.64|                 -8.56|               120.00|            -1,755.00|                       21.42|                     300.91|                   4,390.79|
|                 -4.5|                     -8.70|                    90.91|                -1,200.00|                -10.27|                90.91|            -1,416.60|                       37.39|                     264.46|                   5,159.52|
|                 -5.0|                     -5.80|                   100.00|                  -800.00|                 -5.72|                83.33|              -789.13|                       35.28|                     250.00|                   4,868.23|
|                 -5.5|                    -16.25|                    90.91|                -2,518.18|                -10.17|                80.00|            -1,576.71|                       21.33|                     243.64|                   3,306.37|
|                 -6.0|                     -2.50|                   100.00|                  -500.00|                -12.93|                74.07|            -2,585.19|                       35.55|                     232.32|                   7,109.26|
|                 -6.5|                     -4.94|                    90.91|                -1,200.00|                  0.53|                74.07|               129.38|                       35.18|                     232.32|                   8,548.88|
|                 -7.0|                     -3.39|                   100.00|                  -936.36|                 -8.73|                83.33|            -2,410.63|                       19.32|                     250.00|                   5,333.53|
|                 -7.5|                    -16.83|                    90.91|                -3,400.00|                 -6.53|                80.00|            -1,319.22|                       10.40|                     203.21|                   2,101.20|
|                 -8.0|                    -15.67|                   100.00|                -2,272.73|                -14.22|                66.67|            -2,061.82|                        9.37|                     218.18|                   1,358.80|
|                 -8.5|                     -2.64|                    90.91|                  -263.64|                 -2.87|                62.50|              -287.12|                       27.77|                     210.23|                   2,777.15|
|                 -9.0|                      1.26|                    90.91|                   145.45|                 -2.56|                58.82|              -294.19|                       31.04|                     191.39|                   3,569.15|
|                 -9.5|                     -7.15|                    90.91|                -1,309.09|                 -8.10|                55.56|            -1,482.92|                       18.81|                     196.97|                   3,441.58|
|                -10.0|                      6.32|                   100.00|                 1,309.09|                  1.39|                51.28|               288.53|                       26.54|                     188.81|                   5,493.21|
|                -10.5|                    -14.15|                    90.91|                -2,390.91|                 -3.86|                40.00|              -652.82|                        6.07|                     167.27|                   1,026.62|
|                -11.0|                     -8.75|                   100.00|                -1,154.55|                 -3.49|                37.04|              -461.22|                        8.59|                     161.62|                   1,134.26|
|                -11.5|                    -11.56|                    90.91|                -1,572.73|                 -7.03|                38.31|              -956.46|                        8.29|                     159.09|                   1,127.75|
|                -12.0|                    -13.79|                   100.00|                -1,254.55|                 -1.39|                35.71|              -126.20|                       -0.92|                     159.09|                     -83.34|
|                -12.5|                    -18.18|                    90.91|                -2,163.64|                -12.88|                36.36|            -1,532.29|                       -2.00|                     156.74|                    -237.51|
|                -13.0|                     -8.52|                    90.91|                -1,227.27|                 -8.67|                32.26|            -1,247.99|                        9.37|                     145.45|                   1,349.98|
|                -13.5|                      3.64|                    90.91|                   509.09|                  1.38|                35.71|               193.02|                       24.31|                     159.09|                   3,403.81|
|                -14.0|                      8.13|                   100.00|                 1,772.73|                 -3.91|                38.46|              -852.27|                       15.21|                     164.34|                   3,315.17|
|                -14.5|                     -2.29|                    90.91|                  -290.91|                 -3.89|                33.33|              -494.45|                       12.38|                     147.06|                   1,572.49|
|                -15.0|                     -8.69|                   100.00|                  -781.82|                 -2.53|                23.81|              -227.86|                        2.27|                     136.36|                     204.18|
|                -15.5|                     -5.89|                    90.91|                  -418.18|                  3.80|                26.32|               269.74|                        8.40|                     141.15|                     596.58|
|                -16.0|                     -8.68|                   100.00|                  -581.82|                 -2.76|                25.00|              -184.98|                       -1.34|                     126.93|                     -89.88|
|                -16.5|                     -9.89|                    90.91|                -1,236.36|                 -5.47|                29.41|              -683.78|                        1.84|                     147.06|                     229.90|
|                -17.0|                      5.32|                   100.00|                   909.09|                  4.37|               600.00|               746.65|                        7.94|                     129.09|                   1,357.93|
|                -17.5|                     -4.55|                    90.91|                  -463.64|                  3.54|                50.00|               361.51|                        5.34|                     133.81|                     544.20|
|                -18.0|                     25.17|                   100.00|                 1,963.64|                 -1.65|                16.67|              -128.62|                       28.28|                     122.73|                   2,205.61|
|                -18.5|                    -12.97|                    90.91|                  -881.82|                  1.18|                20.00|                80.40|                       -5.99|                     119.19|                    -407.58|
|                -19.0|                    -13.78|                    90.91|                  -854.55|                 -1.37|                90.91|               -84.71|                       -5.00|                     214.08|                    -309.86|
|                -19.5|                      5.83|                    90.91|                   536.36|                  0.88|                30.77|                81.40|                       13.52|                     118.18|                   1,243.55|
|                -20.0|                      7.50|                   100.00|                   600.00|                  3.71|                18.18|               296.53|                       12.78|                     118.18|                   1,022.44|
|                -20.5|                      3.41|                    90.91|                   245.45|                  0.00|                55.56|                -0.25|                        9.68|                     112.00|                     696.87|
|                -21.0|                     13.38|                   100.00|                 1,445.45|                 29.36|             2,875.00|             3,170.75|                       65.62|                   5,579.55|                   7,086.74|
|                -21.5|                     -3.37|                    90.91|                  -272.73|                 -2.95|                10.53|              -238.87|                        1.17|                     111.00|                      94.53|
|                -22.0|                      9.31|                   100.00|                   390.91|                 -7.25|                10.00|              -304.56|                       10.20|                     110.00|                     428.47|
|                -22.5|                     -8.36|                    90.91|                  -418.18|                  0.94|                13.99|                46.85|                       -4.11|                     106.82|                    -205.52|
|                -23.0|                      2.69|                   100.00|                   145.45|                  2.64|                11.70|               142.29|                        3.21|                     106.18|                     173.32|
|                -23.5|                      5.73|                    90.91|                   372.73|                 -7.08|                10.00|              -459.99|                       10.43|                     110.00|                     678.00|
|                -24.0|                     16.16|                   100.00|                 1,309.09|                  4.93|               500.00|               399.38|                        5.15|                     109.09|                     416.87|
|                -24.5|                     18.31|                    90.91|                 1,300.00|                  0.30|                 7.41|                21.48|                       22.16|                     105.05|                   1,573.67|
|                -25.0|                     -8.66|                   100.00|                  -363.64|                 -4.06|                 9.30|              -170.62|                      -15.42|                     108.67|                    -647.67|
|                -25.5|                      6.06|                    90.91|                   218.18|                 -2.60|                 9.09|               -93.58|                        9.14|                     102.14|                     329.10|
|                -26.0|                     28.86|                    90.91|                 1,154.55|                 -0.31|                 5.56|               -12.29|                       31.79|                     100.45|                   1,271.55|
|                -26.5|                     13.51|                    90.91|                   500.00|                 -2.79|                 7.69|              -103.37|                       16.03|                     100.45|                     593.27|
|                -27.0|                     -6.58|                    90.91|                  -309.09|                  1.95|                 6.67|                91.65|                       -5.04|                      98.44|                    -236.81|
|                -27.5|                     -6.77|                    90.91|                  -290.91|                 -0.46|                 5.26|               -19.64|                       -5.31|                     100.45|                    -228.20|
|                -28.0|                      7.22|                   100.00|                   454.55|                 -3.05|                 4.00|              -192.39|                      -16.79|                      97.73|                  -1,057.89|
|                -28.5|                      9.77|                    90.91|                   390.91|                 -1.24|                 4.55|               -49.56|                       11.25|                      99.59|                     449.97|
|                -29.0|                      3.41|                    90.91|                    81.82|                 -2.89|                 2.70|               -69.43|                        4.60|                      95.68|                     110.38|
|                -29.5|                      4.13|                    90.91|                    90.91|                 -3.44|                 3.70|               -75.57|                        5.26|                      97.98|                     115.67|
|                -30.0|                     10.49|                   100.00|                   272.73|                 -2.68|                 2.70|               -69.56|                        3.99|                      96.07|                     103.71|
|                -30.5|                     -8.08|                    90.91|                  -218.18|                 -2.94|                 2.16|               -79.39|                       -7.36|                      95.04|                    -198.67|
|                -31.0|                    -13.49|                   100.00|                  -418.18|                  0.91|                 2.63|                28.28|                      -19.06|                      95.93|                    -590.84|
|                -31.5|                    -14.59|                    90.91|                  -554.55|                 -1.84|                 3.33|               -69.85|                      -14.01|                      94.65|                    -532.42|
|                -32.0|                     -4.55|                    90.91|                   -63.64|                  0.70|                 2.00|                 9.78|                       -3.92|                      93.01|                     -54.92|
|                -32.5|                     23.53|                    90.91|                   400.00|                  0.74|                 3.33|                12.62|                       24.27|                      93.64|                     412.55|
|                -33.0|                    -15.78|                    90.91|                  -536.36|                  0.58|                 1.25|                19.81|                      -15.26|                      93.23|                    -518.79|
|                -33.5|                     14.55|                    90.91|                   363.64|                 -3.48|                 2.00|               -87.02|                       15.21|                      94.73|                     380.17|
|                -34.0|                     -0.40|                    90.91|                    -9.09|                  0.39|                 0.87|                 8.98|                        0.05|                      92.57|                       1.06|
|                -34.5|                    -46.55|                    90.91|                -1,163.64|                  0.44|                 1.46|                11.04|                      -46.38|                      92.82|                  -1,159.55|
|                -35.0|                     -4.55|                    90.91|                  -100.00|                  0.42|                 1.41|                 9.18|                       -4.11|                      93.01|                     -90.33|
|                -35.5|                     14.55|                    90.91|                   290.91|                  0.28|                 1.11|                 5.54|                       14.94|                      93.03|                     298.84|
|                -36.0|                      4.13|                    90.91|                    45.45|                  0.45|                 1.11|                 4.90|                        4.67|                      93.03|                      51.39|
|                -36.5|                    -76.14|                    90.91|                -1,218.18|                -12.16|                 1.33|              -194.52|                      -76.11|                      91.36|                  -1,217.73|
|                -37.0|                     16.67|                    90.91|                   300.00|                  0.42|                 1.25|                 7.50|                       17.12|                      93.03|                     308.09|
|                -37.5|                    -26.57|                    90.91|                  -345.45|                  0.09|                 0.37|                 1.20|                      -26.54|                      91.29|                    -345.07|
|                -38.0|                      0.00|                    90.91|                     0.00|                  0.35|                 2.50|                 7.42|                        0.36|                      95.68|                       7.63|
|                -38.5|                     21.49|                    90.91|                   236.36|                  0.15|                 0.67|                 1.68|                       21.75|                      92.18|                     239.22|
|                -39.0|                      6.06|                    90.91|                    54.55|                  0.29|                 1.00|                 2.64|                        6.29|                      92.82|                      56.65|
|                -39.5|                    -45.45|                    90.91|                  -318.18|                -14.11|                 0.67|               -98.79|                      -45.36|                      91.55|                    -317.55|
|                -40.0|                     93.18|                   100.00|                   372.73|                  0.06|                 0.25|                 0.25|                       43.30|                      91.38|                     173.20|
|                -40.5|                     48.48|                    90.91|                   436.36|                  0.04|                 0.22|                 0.33|                       48.48|                      90.91|                     436.36|
|                -41.0|                     72.73|                   100.00|                   727.27|                  0.09|                 0.21|                 0.89|                       52.85|                      91.30|                     528.45|
|                -41.5|                     14.55|                    90.91|                    72.73|                  0.17|                 0.50|                 0.85|                       14.82|                      91.86|                      74.09|
|                -42.0|                     52.73|                    90.91|                   263.64|                  0.06|                 0.11|                 0.31|                       52.84|                      91.11|                     264.22|
|                -42.5|                     -4.55|                    90.91|                   -36.36|                  0.13|                 0.53|                 1.02|                       -4.49|                      91.12|                     -35.96|
|                -43.0|                     14.55|                    90.91|                    72.73|                  1.62|                 6.90|                 8.08|                       14.83|                      92.18|                      74.13|
|                -43.5|                     14.55|                    90.91|                    72.73|                  0.07|                 0.10|                 0.34|                       14.64|                      91.07|                      73.18|
|                -44.0|                    -51.14|                   100.00|                  -409.09|                  0.05|                 0.11|                 0.41|                      -76.12|                      91.05|                    -608.95|
|                -44.5|                   -100.00|                  -100.00|                  -300.00|                  0.04|                 0.12|                 0.12|                     -100.00|                    -100.00|                    -300.00|
|                -45.0|                     15.45|                   100.00|                   154.55|                  4.40|                43.67|                44.02|                        3.82|                     174.28|                      38.22|
|                -45.5|                     90.91|                    90.91|                   545.45|                  0.02|                 0.12|                 0.12|                       90.95|                      91.14|                     545.69|
|                -46.0|                   -100.00|                  -100.00|                  -200.00|                  0.03|                 0.06|                 0.06|                     -100.00|                    -100.00|                    -200.00|
|                -46.5|                    -23.64|                    90.91|                  -118.18|                  0.09|                 0.33|                 0.43|                      -23.51|                      91.55|                    -117.55|
|                -47.0|                     90.91|                    90.91|                    90.91|                  0.00|                 0.00|                 0.00|                       90.91|                      90.91|                      90.91|
|                -47.5|                     27.27|                    90.91|                    81.82|                  0.00|                 0.00|                 0.00|                       27.27|                      90.91|                      81.82|
|                -48.5|                     -4.55|                    90.91|                   -18.18|                  0.04|                 0.11|                 0.18|                       -4.51|                      91.04|                     -18.05|
|                -49.5|                   -100.00|                  -100.00|                  -100.00|                  0.00|                 0.00|                 0.00|                     -100.00|                    -100.00|                    -100.00|
|                -50.5|                     90.91|                    90.91|                    90.91|                  0.48|                 0.48|                 0.48|                       91.82|                      91.82|                      91.82|
|                -51.5|                    -36.36|                    90.91|                  -109.09|                  0.01|                 0.04|                 0.04|                      -36.36|                      90.91|                    -109.09|
|                -52.0|                   -100.00|                  -100.00|                  -100.00|                  0.00|                 0.00|                 0.00|                     -100.00|                    -100.00|                    -100.00|
|                -54.0|                   -100.00|                  -100.00|                  -200.00|                  0.01|                 0.01|                 0.01|                     -100.00|                    -100.00|                    -200.00|
|                -54.5|                   -100.00|                  -100.00|                  -100.00|                  0.00|                 0.00|                 0.00|                     -100.00|                    -100.00|                    -100.00|
|                -55.0|                   -100.00|                  -100.00|                  -100.00|                  0.00|                 0.00|                 0.00|                     -100.00|                    -100.00|                    -100.00|
|                -56.0|                     -4.55|                    90.91|                    -9.09|                  0.01|                 0.03|                 0.03|                       -4.55|                      90.91|                      -9.09|

#### Table: NCAAF 2007 to Present - Visitor Favorites - Summary by Spread Amount - Opening Line

| Visitor - Spread - Open| Visitor - Spread - PL - Mean| Visitor - Spread - PL - Max| Visitor - Spread - PL - Sum| Visitor - ML - PL - Mean| Visitor - ML - PL - Max| Visitor - ML - PL - Sum| Visitor - 2-Parlay - PL - Mean| Visitor - 2-Parlay - PL - Max| Visitor - 2-Parlay - PL - Sum|
|-----------------------:|----------------------------:|---------------------------:|---------------------------:|------------------------:|-----------------------:|-----------------------:|------------------------------:|-----------------------------:|-----------------------------:|
|                    -0.5|                      -100.00|                     -100.00|                     -300.00|                  -100.00|                 -100.00|                 -300.00|                        -100.00|                       -100.00|                       -300.00|
|                    -1.0|                        -1.56|                      100.00|                     -218.18|                    -6.90|                  120.00|                 -966.19|                          67.10|                        320.00|                      9,393.53|
|                    -1.5|                       -16.07|                       90.91|                   -1,863.64|                    -9.42|                  250.00|               -1,092.79|                          56.51|                        568.18|                      6,554.84|
|                    -2.0|                        -0.14|                      100.00|                      -18.18|                    -6.76|                  135.00|                 -905.78|                          68.41|                        348.64|                      9,166.44|
|                    -2.5|                        -7.57|                       90.91|                   -1,672.73|                   -10.56|                  135.00|               -2,334.08|                          59.95|                        348.64|                     13,249.21|
|                    -3.0|                         2.50|                      100.00|                      745.45|                    -3.75|                  180.00|               -1,116.71|                          46.02|                        434.55|                     13,712.93|
|                    -3.5|                        -1.35|                       90.91|                     -281.82|                    -0.61|                  140.00|                 -127.85|                          62.63|                        358.18|                     13,089.94|
|                    -4.0|                         4.94|                      100.00|                      854.55|                    -0.21|                   86.96|                  -36.25|                          59.44|                        256.92|                     10,283.63|
|                    -4.5|                        -0.96|                       90.91|                     -127.27|                    -0.41|                   84.75|                  -54.58|                          50.06|                        252.70|                      6,658.54|
|                    -5.0|                        -4.45|                      100.00|                     -409.09|                    -6.23|                   95.24|                 -573.38|                          38.61|                        256.92|                      3,552.32|
|                    -5.5|                         1.47|                       90.91|                      163.64|                    -7.04|                   71.43|                 -780.93|                          49.76|                        227.27|                      5,523.71|
|                    -6.0|                         5.13|                      100.00|                      718.18|                     6.84|                   83.33|                  957.42|                          48.47|                        250.00|                      6,786.41|
|                    -6.5|                       -12.08|                       90.91|                   -1,836.36|                    -9.73|                  120.00|               -1,478.93|                          22.79|                        320.00|                      3,464.37|
|                    -7.0|                         2.29|                      100.00|                      500.00|                    -3.18|                   66.67|                 -693.92|                          19.55|                        218.18|                      4,261.35|
|                    -7.5|                       -16.58|                       90.91|                   -1,972.73|                   -14.18|                   58.82|               -1,687.28|                          11.98|                        203.21|                      1,425.48|
|                    -8.0|                         3.27|                      100.00|                      327.27|                    -1.70|                   44.64|                 -170.39|                          29.30|                        176.14|                      2,929.99|
|                    -8.5|                       -33.47|                       90.91|                   -2,209.09|                    -0.97|                   50.00|                  -64.29|                         -14.47|                        167.27|                       -954.74|
|                    -9.0|                         3.06|                      100.00|                      272.73|                    -4.37|                   90.91|                 -388.91|                          30.35|                        177.69|                      2,701.21|
|                    -9.5|                       -10.16|                       90.91|                   -1,036.36|                   -14.00|                   50.00|               -1,428.17|                          15.28|                        186.36|                      1,558.30|
|                   -10.0|                         6.30|                      100.00|                      863.64|                     3.79|                   47.62|                  519.49|                          22.38|                        177.69|                      3,066.14|
|                   -10.5|                        -6.82|                       90.91|                     -572.73|                     2.87|                   45.45|                  240.75|                          16.74|                        154.55|                      1,406.15|
|                   -11.0|                       -19.21|                      100.00|                   -1,363.64|                    -4.20|                   41.67|                 -298.27|                          -3.23|                        170.45|                       -229.07|
|                   -11.5|                         7.39|                       90.91|                      472.73|                    -6.31|                   36.36|                 -403.63|                          31.61|                        160.33|                      2,022.97|
|                   -12.0|                        13.02|                      100.00|                      572.73|                     1.82|                   32.26|                   80.27|                          32.12|                        152.49|                      1,413.47|
|                   -12.5|                        -6.16|                       90.91|                     -363.64|                     9.08|                   40.82|                  535.58|                          13.60|                        168.83|                        802.19|
|                   -13.0|                        -6.22|                      100.00|                     -609.09|                    -1.05|                   37.04|                 -102.64|                           4.54|                        147.90|                        444.95|
|                   -13.5|                         4.36|                       90.91|                      327.27|                     1.94|                  445.00|                  145.28|                          22.90|                        140.50|                      1,717.20|
|                   -14.0|                         2.62|                      100.00|                      254.55|                    -5.70|                   31.25|                 -552.77|                          13.90|                        150.57|                      1,348.43|
|                   -14.5|                        20.72|                       90.91|                    1,409.09|                     4.95|                   35.71|                  336.78|                          37.83|                        147.90|                      2,572.19|
|                   -15.0|                        -2.10|                       90.91|                      -81.82|                     2.88|                   25.00|                  112.20|                          12.06|                        129.09|                        470.16|
|                   -15.5|                        -1.97|                       90.91|                      -72.73|                    -1.37|                   20.41|                  -50.85|                          11.54|                        126.26|                        426.95|
|                   -16.0|                        19.32|                       90.91|                      772.73|                     0.88|                   21.74|                   35.15|                          33.71|                        132.41|                      1,348.42|
|                   -16.5|                         7.60|                       90.91|                      418.18|                     8.16|                   21.51|                  448.78|                          20.87|                        129.09|                      1,147.82|
|                   -17.0|                        37.39|                      100.00|                    1,981.82|                     3.00|                   19.80|                  159.12|                          39.80|                        128.71|                      2,109.16|
|                   -17.5|                        -6.58|                       90.91|                     -309.09|                    -3.39|                   18.02|                 -159.37|                           2.64|                        120.74|                        124.15|
|                   -18.0|                       -26.84|                      100.00|                     -563.64|                    -1.29|                   18.18|                  -26.99|                         -31.57|                        113.37|                       -662.87|
|                   -18.5|                        13.13|                       90.91|                      354.55|                     5.18|                   15.87|                  139.77|                          23.20|                        121.21|                        626.34|
|                   -19.0|                       -21.90|                       90.91|                     -481.82|                     4.75|                   14.29|                  104.41|                         -14.43|                        118.18|                       -317.55|
|                   -19.5|                         5.33|                       90.91|                      154.55|                    47.63|                1,150.00|                1,381.37|                          13.50|                        122.73|                        391.55|
|                   -20.0|                         8.56|                       90.91|                      436.36|                    -0.50|                   18.83|                  -25.47|                          16.83|                        121.70|                        858.13|
|                   -20.5|                        21.49|                       90.91|                      472.73|                     7.26|                   11.11|                  159.69|                          29.35|                        112.12|                        645.70|
|                   -21.0|                        15.15|                      100.00|                      454.55|                    -7.70|                   14.29|                 -231.06|                           8.24|                        118.18|                        247.33|
|                   -21.5|                        18.50|                       90.91|                      536.36|                     6.81|                   14.29|                  197.55|                          26.32|                        118.18|                        763.15|
|                   -22.0|                        -4.04|                      100.00|                      -72.73|                    -6.28|                   11.10|                 -113.01|                         -11.12|                        112.10|                       -200.24|
|                   -22.5|                       -49.09|                       90.91|                     -736.36|                   -14.89|                    9.71|                 -223.36|                         -45.65|                        108.26|                       -684.69|
|                   -23.0|                        -8.70|                       90.91|                     -200.00|                    -3.36|                   12.50|                  -77.23|                          -3.55|                        111.00|                        -81.62|
|                   -23.5|                       -25.76|                       90.91|                     -463.64|                    -1.21|                   11.76|                  -21.72|                         -22.54|                        105.27|                       -405.64|
|                   -24.0|                        22.18|                       90.91|                      554.55|                     4.44|                    8.13|                  111.02|                          27.38|                        106.43|                        684.60|
|                   -24.5|                        14.55|                       90.91|                      290.91|                    -1.47|                    8.00|                  -29.39|                          18.86|                        106.18|                        377.11|
|                   -25.0|                         3.50|                      100.00|                       45.45|                     3.54|                   11.76|                   46.06|                          -9.32|                        103.23|                       -121.20|
|                   -25.5|                         6.06|                       90.91|                      109.09|                    -3.41|                    5.71|                  -61.30|                           8.68|                        101.82|                        156.18|
|                   -26.0|                         6.06|                       90.91|                       54.55|                     2.27|                    5.00|                   20.45|                           8.44|                         95.68|                         75.99|
|                   -26.5|                       -52.27|                       90.91|                     -418.18|                     2.95|                    4.35|                   23.64|                         -50.55|                         99.21|                       -404.43|
|                   -27.0|                        -4.55|                       90.91|                      -90.91|                    -2.50|                    5.88|                  -50.05|                          -1.78|                        102.14|                        -35.65|
|                   -27.5|                         9.09|                       90.91|                       63.64|                     1.79|                    2.50|                   12.50|                          11.24|                         95.17|                         78.71|
|                   -28.0|                         9.09|                       90.91|                      127.27|                    -5.40|                    3.33|                  -75.55|                          11.21|                         97.27|                        156.88|
|                   -28.5|                        33.64|                       90.91|                      336.36|                    -9.14|                    1.63|                  -91.36|                          34.73|                         94.02|                        347.31|
|                   -29.0|                        -4.55|                       90.91|                      -45.45|                     1.04|                    2.05|                   10.44|                          -3.26|                         94.82|                        -32.64|
|                   -29.5|                        90.91|                       90.91|                      181.82|                     1.64|                    2.28|                    3.28|                          94.04|                         95.25|                        188.07|
|                   -30.0|                       -52.27|                       90.91|                     -209.09|                     1.11|                    2.00|                    4.46|                         -51.92|                         92.32|                       -207.68|
|                   -30.5|                       -36.36|                       90.91|                     -109.09|                     0.97|                    1.92|                    2.92|                         -35.14|                         94.58|                       -105.42|
|                   -31.0|                         6.06|                       90.91|                       54.55|                     0.66|                    1.43|                    5.92|                           6.92|                         93.64|                         62.26|
|                   -31.5|                        27.27|                       90.91|                       81.82|                     0.52|                    1.00|                    1.56|                          27.63|                         91.97|                         82.88|
|                   -32.0|                      -100.00|                     -100.00|                     -300.00|                   -32.80|                    1.00|                  -98.40|                        -100.00|                       -100.00|                       -300.00|
|                   -32.5|                        27.27|                       90.91|                       81.82|                     1.08|                    1.67|                    3.24|                          28.64|                         94.09|                         85.91|
|                   -33.0|                        90.91|                       90.91|                      181.82|                     0.52|                    0.67|                    1.04|                          91.90|                         92.18|                        183.80|
|                   -33.5|                      -100.00|                     -100.00|                     -100.00|                     0.51|                    0.51|                    0.51|                        -100.00|                       -100.00|                       -100.00|
|                   -34.0|                        30.30|                      100.00|                       90.91|                     0.43|                    0.90|                    1.30|                         -36.36|                         90.91|                       -109.09|
|                   -34.5|                        27.27|                       90.91|                       81.82|                     0.33|                    1.00|                    1.00|                          27.27|                         90.91|                         81.82|
|                   -35.0|                        90.91|                       90.91|                      363.64|                     0.43|                    0.91|                    1.73|                          91.73|                         92.64|                        366.93|
|                   -35.5|                        -4.55|                       90.91|                      -18.18|                     0.56|                    1.29|                    2.24|                          -3.61|                         93.37|                        -14.44|
|                   -36.0|                        -4.55|                       90.91|                       -9.09|                     0.83|                    1.46|                    1.66|                          -4.35|                         91.30|                         -8.70|
|                   -37.0|                      -100.00|                     -100.00|                     -200.00|                     0.29|                    0.33|                    0.57|                        -100.00|                       -100.00|                       -200.00|
|                   -37.5|                        90.91|                       90.91|                       90.91|                     0.31|                    0.31|                    0.31|                          91.50|                         91.50|                         91.50|
|                   -38.0|                        90.91|                       90.91|                       90.91|                     0.00|                    0.00|                    0.00|                          90.91|                         90.91|                         90.91|
|                   -39.0|                      -100.00|                     -100.00|                     -200.00|                     1.00|                    2.00|                    2.00|                        -100.00|                       -100.00|                       -200.00|
|                   -39.5|                      -100.00|                     -100.00|                     -100.00|                     0.50|                    0.50|                    0.50|                        -100.00|                       -100.00|                       -100.00|
|                   -40.0|                        90.91|                       90.91|                       90.91|                     0.37|                    0.37|                    0.37|                          91.62|                         91.62|                         91.62|
|                   -41.5|                      -100.00|                     -100.00|                     -200.00|                  -100.00|                 -100.00|                 -200.00|                        -100.00|                       -100.00|                       -200.00|
|                   -42.0|                        -4.55|                       90.91|                       -9.09|                     0.38|                    0.77|                    0.77|                          -3.81|                         92.38|                         -7.62|
|                   -43.5|                        90.91|                       90.91|                       90.91|                     0.19|                    0.19|                    0.19|                          91.26|                         91.26|                         91.26|
|                   -50.5|                      -100.00|                     -100.00|                     -100.00|                     0.02|                    0.02|                    0.02|                        -100.00|                       -100.00|                       -100.00|

#### Table: NCAAF 2007 to Present - Home Underdogs - Summary by Spread Amount - Opening Line

| Home - Spread - Open| Home - Spread - PL - Mean| Home - Spread - PL - Max| Home - Spread - PL - Sum| Home - ML - PL - Mean| Home - ML - PL - Max| Home - ML - PL - Sum| Home - 2-Parlay - PL - Mean| Home - 2-Parlay - PL - Max| Home - 2-Parlay - PL - Sum|
|--------------------:|-------------------------:|------------------------:|------------------------:|---------------------:|--------------------:|--------------------:|---------------------------:|--------------------------:|--------------------------:|
|                  0.5|                     90.91|                    90.91|                   272.73|                 94.57|               135.00|               283.71|                      271.45|                     348.64|                     814.35|
|                  1.0|                     -1.56|                   100.00|                  -218.18|                 -2.51|               173.00|              -351.41|                       86.12|                     421.18|                  12,056.41|
|                  1.5|                      6.97|                    90.91|                   809.09|                  5.65|               170.00|               654.87|                      101.69|                     415.45|                  11,795.66|
|                  2.0|                     -5.83|                   100.00|                  -781.82|                 -3.27|               180.00|              -438.26|                       84.67|                     434.55|                  11,345.15|
|                  2.5|                     -1.52|                    90.91|                  -336.36|                  7.66|               210.00|             1,692.07|                      105.53|                     491.82|                  23,321.23|
|                  3.0|                      3.14|                   100.00|                   936.36|                 -5.29|               240.00|            -1,575.08|                       80.82|                     549.09|                  24,083.95|
|                  3.5|                     -7.74|                    90.91|                -1,618.18|                  0.68|               267.00|               141.68|                       92.20|                     600.64|                  19,270.47|
|                  4.0|                    -11.61|                   100.00|                -2,009.09|                 -8.61|               265.00|            -1,489.80|                       74.47|                     596.82|                  12,883.12|
|                  4.5|                     -8.13|                    90.91|                -1,081.82|                 -3.77|               260.00|              -501.76|                       83.71|                     587.27|                  11,133.00|
|                  5.0|                     -2.37|                   100.00|                  -218.18|                  5.26|               375.00|               484.00|                      100.95|                     806.82|                   9,287.64|
|                  5.5|                    -10.57|                    90.91|                -1,172.73|                  4.02|               274.00|               446.14|                       98.58|                     614.00|                  10,942.64|
|                  6.0|                    -11.23|                   100.00|                -1,572.73|                -24.73|               280.00|            -3,461.76|                       43.70|                     625.45|                   6,118.45|
|                  6.5|                      2.99|                    90.91|                   454.55|                  7.90|               330.00|             1,200.67|                      105.99|                     720.91|                  16,110.36|
|                  7.0|                      4.92|                   100.00|                 1,072.73|                 -3.02|               444.00|              -658.00|                       85.15|                     938.55|                  18,562.00|
|                  7.5|                      7.49|                    90.91|                   890.91|                 21.55|               375.00|             2,565.00|                      132.06|                     806.82|                  15,715.00|
|                  8.0|                     -8.18|                   100.00|                  -818.18|                -17.38|               400.00|            -1,738.19|                       57.73|                     854.55|                   5,772.55|
|                  8.5|                     24.38|                    90.91|                 1,609.09|                -10.47|               475.00|              -691.00|                       70.92|                     997.73|                   4,680.82|
|                  9.0|                     -9.81|                   100.00|                  -872.73|                 -2.00|               400.00|              -178.00|                       87.09|                     854.55|                   7,751.09|
|                  9.5|                      1.07|                    90.91|                   109.09|                 19.05|               360.00|             1,943.00|                      127.28|                     778.18|                  12,982.09|
|                 10.0|                     -6.24|                   100.00|                  -854.55|                -25.48|               525.00|            -3,491.00|                       42.26|                   1,093.18|                   5,789.91|
|                 10.5|                     -2.27|                    90.91|                  -190.91|                -24.37|               405.00|            -2,047.00|                       44.39|                     864.09|                   3,728.45|
|                 11.0|                     13.06|                   100.00|                   927.27|                 -0.85|               450.00|               -60.00|                       89.30|                     950.00|                   6,340.00|
|                 11.5|                    -16.48|                    90.91|                -1,054.55|                  8.78|               475.00|               562.00|                      107.67|                     997.73|                   6,891.09|
|                 12.0|                    -17.36|                   100.00|                  -763.64|                -33.07|               400.00|            -1,455.00|                       27.78|                     854.55|                   1,222.27|
|                 12.5|                     -2.93|                    90.91|                  -172.73|                -44.07|               700.00|            -2,600.00|                        6.78|                   1,427.27|                     400.00|
|                 13.0|                      3.53|                   100.00|                   345.45|                -17.68|               550.00|            -1,733.00|                       57.15|                   1,140.91|                   5,600.64|
|                 13.5|                    -13.45|                    90.91|                -1,009.09|                 -0.20|               639.00|               -15.00|                       90.53|                   1,310.82|                   6,789.55|
|                 14.0|                     -5.25|                   100.00|                  -509.09|                 17.18|               840.00|             1,666.00|                      123.70|                   1,694.55|                  11,998.73|
|                 14.5|                    -29.81|                    90.91|                -2,027.27|                -43.13|               800.00|            -2,933.00|                        8.57|                   1,618.18|                     582.45|
|                 15.0|                     -6.99|                    90.91|                  -272.73|                -54.88|               550.00|            -2,140.50|                      -13.87|                   1,140.91|                    -540.95|
|                 15.5|                     -7.13|                    90.91|                  -263.64|                 -8.92|               700.00|              -330.00|                       73.88|                   1,427.27|                   2,733.64|
|                 16.0|                    -28.41|                    90.91|                -1,136.36|                -39.75|               600.00|            -1,590.00|                       15.02|                   1,236.36|                     600.91|
|                 16.5|                    -16.69|                    90.91|                  -918.18|                -73.89|               711.00|            -4,064.00|                      -50.16|                   1,448.27|                  -2,758.55|
|                 17.0|                    -34.65|                   100.00|                -1,836.36|                -41.89|             1,000.00|            -2,220.00|                       10.94|                   2,000.00|                     580.00|
|                 17.5|                     -2.51|                    90.91|                  -118.18|                 17.64|             1,100.00|               829.00|                      124.58|                   2,190.91|                   5,855.36|
|                 18.0|                     27.71|                   100.00|                   581.82|                -20.24|             1,000.00|              -425.00|                       52.27|                   2,000.00|                   1,097.73|
|                 18.5|                    -22.22|                    90.91|                  -600.00|                -70.37|               700.00|            -1,900.00|                      -43.43|                   1,427.27|                  -1,172.73|
|                 19.0|                     12.81|                    90.91|                   281.82|                -67.23|               621.00|            -1,479.00|                      -37.43|                   1,276.45|                    -823.55|
|                 19.5|                    -14.42|                    90.91|                  -418.18|               -100.00|              -100.00|            -2,900.00|                     -100.00|                    -100.00|                  -2,900.00|
|                 20.0|                    -17.65|                    90.91|                  -900.00|                -31.76|             1,100.00|            -1,620.00|                       30.27|                   2,190.91|                   1,543.64|
|                 20.5|                    -30.58|                    90.91|                  -672.73|               -100.00|              -100.00|            -2,200.00|                     -100.00|                    -100.00|                  -2,200.00|
|                 21.0|                     -7.04|                   100.00|                  -218.18|                 68.96|             1,500.00|             2,137.69|                      222.56|                   2,954.55|                   6,899.23|
|                 21.5|                    -27.59|                    90.91|                  -800.00|               -100.00|              -100.00|            -2,900.00|                     -100.00|                    -100.00|                  -2,900.00|
|                 22.0|                      6.57|                   100.00|                   118.18|                  5.56|             1,000.00|               100.00|                      101.52|                   2,000.00|                   1,827.27|
|                 22.5|                     40.00|                    90.91|                   600.00|                162.67|             1,340.00|             2,440.00|                      401.45|                   2,649.09|                   6,021.82|
|                 23.0|                     -0.40|                    90.91|                    -9.09|                 -8.70|             1,200.00|              -200.00|                       74.31|                   2,381.82|                   1,709.09|
|                 23.5|                     16.67|                    90.91|                   300.00|                -16.67|             1,400.00|              -300.00|                       59.09|                   2,763.64|                   1,063.64|
|                 24.0|                    -31.27|                    90.91|                  -781.82|               -100.00|              -100.00|            -2,500.00|                     -100.00|                    -100.00|                  -2,500.00|
|                 24.5|                    -23.64|                    90.91|                  -472.73|                -12.50|             1,650.00|              -250.00|                       67.05|                   3,240.91|                   1,340.91|
|                 25.0|                      3.50|                   100.00|                    45.45|               -100.00|              -100.00|            -1,300.00|                     -100.00|                    -100.00|                  -1,300.00|
|                 25.5|                    -15.15|                    90.91|                  -272.73|                -50.00|               800.00|              -900.00|                       -4.55|                   1,618.18|                     -81.82|
|                 26.0|                    -15.15|                    90.91|                  -136.36|               -100.00|              -100.00|              -900.00|                     -100.00|                    -100.00|                    -900.00|
|                 26.5|                     43.18|                    90.91|                   345.45|               -100.00|              -100.00|              -800.00|                     -100.00|                    -100.00|                    -800.00|
|                 27.0|                     -4.55|                    90.91|                   -90.91|                -94.96|                 0.80|            -1,899.20|                      -90.38|                      92.44|                  -1,807.56|
|                 27.5|                    -18.18|                    90.91|                  -127.27|               -100.00|              -100.00|              -700.00|                     -100.00|                    -100.00|                    -700.00|
|                 28.0|                    -18.18|                    90.91|                  -254.55|                121.43|             3,000.00|             1,700.00|                      322.73|                   5,818.18|                   4,518.18|
|                 28.5|                    -42.73|                    90.91|                  -427.27|                410.00|             5,000.00|             4,100.00|                      873.64|                   9,636.36|                   8,736.36|
|                 29.0|                     -4.55|                    90.91|                   -45.45|               -100.00|              -100.00|            -1,000.00|                     -100.00|                    -100.00|                  -1,000.00|
|                 29.5|                   -100.00|                  -100.00|                  -200.00|               -100.00|              -100.00|              -200.00|                     -100.00|                    -100.00|                    -200.00|
|                 30.0|                     43.18|                    90.91|                   172.73|               -100.00|              -100.00|              -400.00|                     -100.00|                    -100.00|                    -400.00|
|                 30.5|                     27.27|                    90.91|                    81.82|               -100.00|              -100.00|              -300.00|                     -100.00|                    -100.00|                    -300.00|
|                 31.0|                    -15.15|                    90.91|                  -136.36|               -100.00|              -100.00|              -900.00|                     -100.00|                    -100.00|                    -900.00|
|                 31.5|                    -36.36|                    90.91|                  -109.09|               -100.00|              -100.00|              -300.00|                     -100.00|                    -100.00|                    -300.00|
|                 32.0|                     90.91|                    90.91|                   272.73|                933.33|             3,000.00|             2,800.00|                    1,872.73|                   5,818.18|                   5,618.18|
|                 32.5|                    -36.36|                    90.91|                  -109.09|               -100.00|              -100.00|              -300.00|                     -100.00|                    -100.00|                    -300.00|
|                 33.0|                   -100.00|                  -100.00|                  -200.00|               -100.00|              -100.00|              -200.00|                     -100.00|                    -100.00|                    -200.00|
|                 33.5|                     90.91|                    90.91|                    90.91|               -100.00|              -100.00|              -100.00|                     -100.00|                    -100.00|                    -100.00|
|                 34.0|                     30.30|                   100.00|                    90.91|               -100.00|              -100.00|              -300.00|                     -100.00|                    -100.00|                    -300.00|
|                 34.5|                    -36.36|                    90.91|                  -109.09|               -100.00|              -100.00|              -300.00|                     -100.00|                    -100.00|                    -300.00|
|                 35.0|                   -100.00|                  -100.00|                  -400.00|               -100.00|              -100.00|              -400.00|                     -100.00|                    -100.00|                    -400.00|
|                 35.5|                     -4.55|                    90.91|                   -18.18|               -100.00|              -100.00|              -400.00|                     -100.00|                    -100.00|                    -400.00|
|                 36.0|                     -4.55|                    90.91|                    -9.09|               -100.00|              -100.00|              -200.00|                     -100.00|                    -100.00|                    -200.00|
|                 37.0|                     90.91|                    90.91|                   181.82|               -100.00|              -100.00|              -200.00|                     -100.00|                    -100.00|                    -200.00|
|                 37.5|                   -100.00|                  -100.00|                  -100.00|               -100.00|              -100.00|              -100.00|                     -100.00|                    -100.00|                    -100.00|
|                 38.0|                   -100.00|                  -100.00|                  -100.00|               -100.00|              -100.00|              -100.00|                     -100.00|                    -100.00|                    -100.00|
|                 39.0|                     90.91|                    90.91|                   181.82|               -100.00|              -100.00|              -200.00|                     -100.00|                    -100.00|                    -200.00|
|                 39.5|                     90.91|                    90.91|                    90.91|               -100.00|              -100.00|              -100.00|                     -100.00|                    -100.00|                    -100.00|
|                 40.0|                   -100.00|                  -100.00|                  -100.00|               -100.00|              -100.00|              -100.00|                     -100.00|                    -100.00|                    -100.00|
|                 41.5|                     90.91|                    90.91|                   181.82|                  0.14|                 0.18|                 0.28|                       91.17|                      91.25|                     182.34|
|                 42.0|                     -4.55|                    90.91|                    -9.09|               -100.00|              -100.00|              -200.00|                     -100.00|                    -100.00|                    -200.00|
|                 43.5|                   -100.00|                  -100.00|                  -100.00|               -100.00|              -100.00|              -100.00|                     -100.00|                    -100.00|                    -100.00|
|                 50.5|                     90.91|                    90.91|                    90.91|               -100.00|              -100.00|              -100.00|                     -100.00|                    -100.00|                    -100.00|

#### Table: NCAAF 2007 to Present - Visitor Underdogs - Summary by Spread Amount - Opening Line

| Visitor - Spread - Open| Visitor - Spread - PL - Mean| Visitor - Spread - PL - Max| Visitor - Spread - PL - Sum| Visitor - ML - PL - Mean| Visitor - ML - PL - Max| Visitor - ML - PL - Sum| Visitor - 2-Parlay - PL - Mean| Visitor - 2-Parlay - PL - Max| Visitor - 2-Parlay - PL - Sum|
|-----------------------:|----------------------------:|---------------------------:|---------------------------:|------------------------:|-----------------------:|-----------------------:|------------------------------:|-----------------------------:|-----------------------------:|
|                     0.5|                        27.27|                       90.91|                       81.82|                     0.00|                     100|                    0.00|                          90.91|                        281.82|                        272.73|
|                     1.0|                        -0.84|                      100.00|                     -136.36|                     0.79|                     200|                  128.37|                          92.42|                        472.73|                     14,972.35|
|                     1.5|                         3.15|                       90.91|                      390.91|                     4.10|                     210|                  508.55|                          98.74|                        491.82|                     12,243.59|
|                     2.0|                         7.37|                      100.00|                      972.73|                     5.92|                     185|                  781.05|                         102.21|                        444.09|                     13,491.10|
|                     2.5|                        -1.52|                       90.91|                     -381.82|                     1.17|                     260|                  294.27|                          93.14|                        587.27|                     23,470.88|
|                     3.0|                         6.65|                      100.00|                    2,327.27|                    -4.88|                     235|               -1,707.99|                          81.59|                        539.55|                     28,557.48|
|                     3.5|                        -9.32|                       90.91|                   -2,236.36|                    -9.39|                     246|               -2,252.61|                          72.99|                        560.55|                     17,517.75|
|                     4.0|                        12.99|                      100.00|                    2,663.64|                     7.96|                     305|                1,631.91|                         106.11|                        673.18|                     21,751.83|
|                     4.5|                        -0.40|                       90.91|                      -54.55|                     4.61|                     380|                  635.51|                          99.70|                        816.36|                     13,758.70|
|                     5.0|                        -0.26|                      100.00|                      -36.36|                    -2.32|                     255|                 -320.67|                          86.47|                        577.73|                     11,933.27|
|                     5.5|                         7.16|                       90.91|                    1,109.09|                     6.92|                     270|                1,072.82|                         104.12|                        606.36|                     16,139.03|
|                     6.0|                        -3.45|                      100.00|                     -690.91|                    15.27|                     330|                3,053.91|                         120.06|                        720.91|                     24,012.01|
|                     6.5|                        -4.15|                       90.91|                   -1,009.09|                   -10.86|                     390|               -2,638.00|                          70.18|                        835.45|                     17,054.73|
|                     7.0|                         4.91|                      100.00|                    1,354.55|                     6.67|                     350|                1,841.00|                         103.64|                        759.09|                     28,605.55|
|                     7.5|                         7.74|                       90.91|                    1,563.64|                     2.86|                     365|                  577.62|                          96.37|                        787.73|                     19,466.36|
|                     8.0|                         8.03|                      100.00|                    1,163.64|                    26.39|                     360|                3,826.00|                         141.28|                        778.18|                     20,486.00|
|                     8.5|                        -6.45|                       90.91|                     -645.45|                    -4.09|                     400|                 -409.00|                          83.10|                        854.55|                      8,310.09|
|                     9.0|                       -10.36|                       90.91|                   -1,190.91|                    -4.68|                     400|                 -538.00|                          81.98|                        854.55|                      9,427.45|
|                     9.5|                        -1.94|                       90.91|                     -354.55|                    12.23|                     420|                2,238.00|                         114.26|                        892.73|                     20,908.91|
|                    10.0|                        -9.35|                      100.00|                   -1,936.36|                   -24.55|                     472|               -5,082.23|                          44.04|                        992.00|                      9,115.74|
|                    10.5|                         5.06|                       90.91|                      854.55|                    -0.12|                     850|                  -21.00|                          90.67|                      1,713.64|                     15,323.55|
|                    11.0|                         2.82|                      100.00|                      372.73|                    -1.12|                     550|                 -148.00|                          88.77|                      1,140.91|                     11,717.45|
|                    11.5|                         2.47|                       90.91|                      336.36|                    10.96|                     650|                1,490.41|                         111.83|                      1,331.82|                     15,208.97|
|                    12.0|                         9.29|                      100.00|                      845.45|                   -18.60|                     475|               -1,692.82|                          55.40|                        997.73|                      5,040.98|
|                    12.5|                         9.09|                       90.91|                    1,081.82|                    28.03|                     530|                3,335.67|                         144.42|                      1,102.73|                     17,186.27|
|                    13.0|                        -0.57|                       90.91|                      -81.82|                    21.99|                     700|                3,167.00|                         132.90|                      1,427.27|                     19,137.00|
|                    13.5|                       -12.73|                       90.91|                   -1,781.82|                   -19.85|                     711|               -2,779.00|                          53.01|                      1,448.27|                      7,421.91|
|                    14.0|                        -7.63|                      100.00|                   -1,663.64|                    -7.03|                     700|               -1,532.00|                          77.49|                      1,427.27|                     16,893.45|
|                    14.5|                        -6.80|                       90.91|                     -863.64|                   -10.24|                     650|               -1,300.61|                          71.36|                      1,331.82|                      9,062.47|
|                    15.0|                         1.92|                      100.00|                      172.73|                    -7.49|                     850|                 -674.00|                          76.61|                      1,713.64|                      6,895.09|
|                    15.5|                        -3.20|                       90.91|                     -227.27|                   -42.54|                     800|               -3,020.00|                           9.71|                      1,618.18|                        689.09|
|                    16.0|                         2.71|                      100.00|                      181.82|                   -24.28|                     620|               -1,627.00|                          44.55|                      1,274.55|                      2,984.82|
|                    16.5|                         0.80|                       90.91|                      100.00|                    -0.80|                     700|                 -100.00|                          89.38|                      1,427.27|                     11,172.73|
|                    17.0|                        -5.85|                      100.00|                   -1,000.00|                   -37.51|                     800|               -6,414.47|                          19.30|                      1,618.18|                      3,299.64|
|                    17.5|                        -4.55|                       90.91|                     -463.64|                   -72.79|                     850|               -7,425.00|                         -48.06|                      1,713.64|                     -4,902.27|
|                    18.0|                       -26.22|                      100.00|                   -2,045.45|                   -27.63|                   1,300|               -2,155.00|                          38.16|                      2,572.73|                      2,976.82|
|                    18.5|                         3.88|                       90.91|                      263.64|                   -41.62|                   1,000|               -2,830.00|                          11.46|                      2,000.00|                        779.09|
|                    19.0|                         4.69|                       90.91|                      290.91|                   -14.76|                   1,000|                 -915.00|                          62.73|                      2,000.00|                      3,889.55|
|                    19.5|                       -14.92|                       90.91|                   -1,372.73|                   -35.59|                   1,362|               -3,274.00|                          22.97|                      2,691.09|                      2,113.27|
|                    20.0|                       -13.98|                      100.00|                   -1,118.18|                   -60.00|                   1,150|               -4,800.00|                         -23.64|                      2,286.36|                     -1,890.91|
|                    20.5|                       -12.50|                       90.91|                     -900.00|                   -32.40|                   1,000|               -2,333.00|                          29.05|                      2,000.00|                      2,091.55|
|                    21.0|                       -15.68|                      100.00|                   -1,709.09|                   -65.24|                     900|               -7,111.00|                         -33.64|                      1,809.09|                     -3,666.45|
|                    21.5|                        -5.72|                       90.91|                     -463.64|                   -22.52|                   1,300|               -1,824.00|                          47.92|                      2,572.73|                      3,881.45|
|                    22.0|                       -13.42|                      100.00|                     -563.64|                    42.86|                   1,440|                1,800.00|                         172.73|                      2,840.00|                      7,254.55|
|                    22.5|                        -0.73|                       90.91|                      -36.36|                   -55.06|                   1,247|               -2,753.00|                         -14.21|                      2,471.55|                       -710.27|
|                    23.0|                        -7.91|                      100.00|                     -427.27|                   -81.02|                     925|               -4,375.00|                         -63.76|                      1,856.82|                     -3,443.18|
|                    23.5|                       -14.83|                       90.91|                     -963.64|                    31.54|                   1,700|                2,050.00|                         151.12|                      3,336.36|                      9,822.73|
|                    24.0|                        -9.76|                      100.00|                     -790.91|                   -38.58|                   1,375|               -3,125.00|                          17.26|                      2,715.91|                      1,397.73|
|                    24.5|                       -27.40|                       90.91|                   -1,945.45|                   -52.77|                   1,753|               -3,747.00|                          -9.84|                      3,437.55|                       -698.82|
|                    25.0|                         9.52|                      100.00|                      400.00|                    -1.19|                   1,500|                  -50.00|                          88.64|                      2,954.55|                      3,722.73|
|                    25.5|                       -15.15|                       90.91|                     -545.45|                   -55.56|                   1,400|               -2,000.00|                         -15.15|                      2,763.64|                       -545.45|
|                    26.0|                       -37.95|                       90.91|                   -1,518.18|                   -60.00|                   1,500|               -2,400.00|                         -23.64|                      2,954.55|                       -945.45|
|                    26.5|                       -22.60|                       90.91|                     -836.36|                   108.11|                   4,500|                4,000.00|                         297.30|                      8,681.82|                     11,000.00|
|                    27.0|                        -2.51|                       90.91|                     -118.18|                  -100.00|                    -100|               -4,700.00|                        -100.00|                       -100.00|                     -4,700.00|
|                    27.5|                        -2.33|                       90.91|                     -100.00|                   -51.16|                   2,000|               -2,200.00|                          -6.77|                      3,909.09|                       -290.91|
|                    28.0|                        10.25|                      100.00|                      645.45|                    38.73|                   5,475|                2,440.00|                         164.85|                     10,543.18|                     10,385.45|
|                    28.5|                       -18.86|                       90.91|                     -754.55|                   -97.50|                       0|               -3,900.00|                         -95.23|                         90.91|                     -3,809.09|
|                    29.0|                       -12.50|                       90.91|                     -300.00|                     8.33|                   2,500|                  200.00|                         106.82|                      4,863.64|                      2,563.64|
|                    29.5|                       -13.22|                       90.91|                     -290.91|                   -95.45|                       0|               -2,100.00|                         -91.32|                         90.91|                     -2,009.09|
|                    30.0|                       -11.54|                      100.00|                     -300.00|                    47.31|                   3,730|                1,230.00|                         181.22|                      7,211.82|                      4,711.82|
|                    30.5|                        -1.01|                       90.91|                      -27.27|                    -3.70|                   2,500|                 -100.00|                          83.84|                      4,863.64|                      2,263.64|
|                    31.0|                        11.14|                      100.00|                      345.45|                  -100.00|                    -100|               -3,100.00|                        -100.00|                       -100.00|                     -3,100.00|
|                    31.5|                         5.50|                       90.91|                      209.09|                     7.89|                   4,000|                  300.00|                         105.98|                      7,727.27|                      4,027.27|
|                    32.0|                        -4.55|                       90.91|                      -63.64|                  -100.00|                    -100|               -1,400.00|                        -100.00|                       -100.00|                     -1,400.00|
|                    32.5|                       -32.62|                       90.91|                     -554.55|                  -100.00|                    -100|               -1,700.00|                        -100.00|                       -100.00|                     -1,700.00|
|                    33.0|                         6.68|                       90.91|                      227.27|                  -100.00|                    -100|               -3,400.00|                        -100.00|                       -100.00|                     -3,400.00|
|                    33.5|                       -23.64|                       90.91|                     -590.91|                    24.00|                   3,000|                  600.00|                         136.73|                      5,818.18|                      3,418.18|
|                    34.0|                        -8.70|                       90.91|                     -200.00|                  -100.00|                    -100|               -2,300.00|                        -100.00|                       -100.00|                     -2,300.00|
|                    34.5|                        37.45|                       90.91|                      936.36|                  -100.00|                    -100|               -2,500.00|                        -100.00|                       -100.00|                     -2,500.00|
|                    35.0|                        -4.55|                       90.91|                     -100.00|                  -100.00|                    -100|               -2,200.00|                        -100.00|                       -100.00|                     -2,200.00|
|                    35.5|                       -23.64|                       90.91|                     -472.73|                  -100.00|                    -100|               -2,000.00|                        -100.00|                       -100.00|                     -2,000.00|
|                    36.0|                       -13.22|                       90.91|                     -145.45|                  -100.00|                    -100|               -1,100.00|                        -100.00|                       -100.00|                     -1,100.00|
|                    36.5|                        67.05|                       90.91|                    1,072.73|                 1,315.62|                  14,950|               21,050.00|                       2,602.56|                     28,631.82|                     41,640.91|
|                    37.0|                       -25.76|                       90.91|                     -463.64|                  -100.00|                    -100|               -1,800.00|                        -100.00|                       -100.00|                     -1,800.00|
|                    37.5|                        17.48|                       90.91|                      227.27|                  -100.00|                    -100|               -1,300.00|                        -100.00|                       -100.00|                     -1,300.00|
|                    38.0|                        -9.09|                       90.91|                     -190.91|                  -100.00|                    -100|               -2,100.00|                        -100.00|                       -100.00|                     -2,100.00|
|                    38.5|                       -30.58|                       90.91|                     -336.36|                  -100.00|                    -100|               -1,100.00|                        -100.00|                       -100.00|                     -1,100.00|
|                    39.0|                       -15.15|                       90.91|                     -136.36|                  -100.00|                    -100|                 -900.00|                        -100.00|                       -100.00|                       -900.00|
|                    39.5|                        36.36|                       90.91|                      254.55|                   485.71|                   4,000|                3,400.00|                       1,018.18|                      7,727.27|                      7,127.27|
|                    40.0|                       -50.00|                      100.00|                     -200.00|                  -100.00|                    -100|                 -400.00|                        -100.00|                       -100.00|                       -400.00|
|                    40.5|                       -57.58|                       90.91|                     -518.18|                  -100.00|                    -100|                 -900.00|                        -100.00|                       -100.00|                       -900.00|
|                    41.0|                       -60.91|                      100.00|                     -609.09|                  -100.00|                    -100|               -1,000.00|                        -100.00|                       -100.00|                     -1,000.00|
|                    41.5|                       -23.64|                       90.91|                     -118.18|                  -100.00|                    -100|                 -500.00|                        -100.00|                       -100.00|                       -500.00|
|                    42.0|                       -61.82|                       90.91|                     -309.09|                  -100.00|                    -100|                 -500.00|                        -100.00|                       -100.00|                       -500.00|
|                    42.5|                        -4.55|                       90.91|                      -36.36|                  -100.00|                    -100|                 -800.00|                        -100.00|                       -100.00|                       -800.00|
|                    43.0|                       -23.64|                       90.91|                     -118.18|                  -100.00|                    -100|                 -500.00|                        -100.00|                       -100.00|                       -500.00|
|                    43.5|                       -23.64|                       90.91|                     -118.18|                  -100.00|                    -100|                 -500.00|                        -100.00|                       -100.00|                       -500.00|
|                    44.0|                        68.18|                      100.00|                      545.45|                  -100.00|                    -100|                 -800.00|                        -100.00|                       -100.00|                       -800.00|
|                    44.5|                        90.91|                       90.91|                      272.73|                  -100.00|                    -100|                 -300.00|                        -100.00|                       -100.00|                       -300.00|
|                    45.0|                        -3.64|                      100.00|                      -36.36|                  -100.00|                    -100|               -1,000.00|                        -100.00|                       -100.00|                     -1,000.00|
|                    45.5|                      -100.00|                     -100.00|                     -600.00|                  -100.00|                    -100|                 -600.00|                        -100.00|                       -100.00|                       -600.00|
|                    46.0|                        90.91|                       90.91|                      181.82|                  -100.00|                    -100|                 -200.00|                        -100.00|                       -100.00|                       -200.00|
|                    46.5|                        14.55|                       90.91|                       72.73|                  -100.00|                    -100|                 -500.00|                        -100.00|                       -100.00|                       -500.00|
|                    47.0|                      -100.00|                     -100.00|                     -100.00|                  -100.00|                    -100|                 -100.00|                        -100.00|                       -100.00|                       -100.00|
|                    47.5|                       -36.36|                       90.91|                     -109.09|                  -100.00|                    -100|                 -300.00|                        -100.00|                       -100.00|                       -300.00|
|                    48.5|                        -4.55|                       90.91|                      -18.18|                  -100.00|                    -100|                 -400.00|                        -100.00|                       -100.00|                       -400.00|
|                    49.5|                        90.91|                       90.91|                       90.91|                  -100.00|                    -100|                 -100.00|                        -100.00|                       -100.00|                       -100.00|
|                    50.5|                      -100.00|                     -100.00|                     -100.00|                  -100.00|                    -100|                 -100.00|                        -100.00|                       -100.00|                       -100.00|
|                    51.5|                        27.27|                       90.91|                       81.82|                  -100.00|                    -100|                 -300.00|                        -100.00|                       -100.00|                       -300.00|
|                    52.0|                        90.91|                       90.91|                       90.91|                  -100.00|                    -100|                 -100.00|                        -100.00|                       -100.00|                       -100.00|
|                    54.0|                        90.91|                       90.91|                      181.82|                  -100.00|                    -100|                 -200.00|                        -100.00|                       -100.00|                       -200.00|
|                    54.5|                        90.91|                       90.91|                       90.91|                  -100.00|                    -100|                 -100.00|                        -100.00|                       -100.00|                       -100.00|
|                    55.0|                        90.91|                       90.91|                       90.91|                  -100.00|                    -100|                 -100.00|                        -100.00|                       -100.00|                       -100.00|
|                    56.0|                        -4.55|                       90.91|                       -9.09|                  -100.00|                    -100|                 -200.00|                        -100.00|                       -100.00|                       -200.00|


